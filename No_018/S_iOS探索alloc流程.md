# iOS探索 alloc流程

### Link

[iOS探索 alloc流程](https://juejin.im/post/5df6323d6fb9a016317813b0)

### Notes

#### 1、前言

可以看到苹果的[官方源码](https://opensource.apple.com/source/objc4/)了

#### 2、alloc的步骤

```objc
+ (id)alloc {
    return _objc_rootAlloc(self);
}
```

```c++
_objc_rootAlloc(Class cls)
{
    return callAlloc(cls, false/*checkNil*/, true/*allocWithZone*/);
}
```

```c++
// Call [cls alloc] or [cls allocWithZone:nil], with appropriate 
// shortcutting optimizations.
static ALWAYS_INLINE id
callAlloc(Class cls, bool checkNil, bool allocWithZone=false)
{
    if (slowpath(checkNil && !cls)) return nil;

#if __OBJC2__
    if (fastpath(!cls->ISA()->hasCustomAWZ())) {
        // No alloc/allocWithZone implementation. Go straight to the allocator.
        // fixme store hasCustomAWZ in the non-meta class and 
        // add it to canAllocFast's summary
        if (fastpath(cls->canAllocFast())) {
            // No ctors, raw isa, etc. Go straight to the metal.
            bool dtor = cls->hasCxxDtor();
            id obj = (id)calloc(1, cls->bits.fastInstanceSize());
            if (slowpath(!obj)) return callBadAllocHandler(cls);
            obj->initInstanceIsa(cls, dtor);
            return obj;
        }
        else {
            // Has ctor or raw isa or something. Use the slower path.
            id obj = class_createInstance(cls, 0);
            if (slowpath(!obj)) return callBadAllocHandler(cls);
            return obj;
        }
    }
#endif

    // No shortcuts available.
    if (allocWithZone) return [cls allocWithZone:nil];
    return [cls alloc];
}

```

1. `fastpath(x)`表示x很可能不为0，需要编译器进行优化，而`slowpath(x)`表示x很可能为0，需要编译器优化。

2. `if (slowpath(checkNil && !cls)) return nil;`这里表示cls大概率是有值的，编译器不用每次都读取return nil指令

3. `id obj = class_createInstance(cls, 0)`内部调用了`_class_createInstanceFromZone(cls, extraBytes, nil)`

4. `_class_createInstanceFromZone`方法

   ```c++
   /***********************************************************************
   * class_createInstance
   * fixme
   * Locking: none
   **********************************************************************/
   
   static __attribute__((always_inline)) 
   id
   _class_createInstanceFromZone(Class cls, size_t extraBytes, void *zone, 
                                 bool cxxConstruct = true, 
                                 size_t *outAllocatedSize = nil)
   {
       if (!cls) return nil;
   
       assert(cls->isRealized());
   
       // Read class's info bits all at once for performance
       bool hasCxxCtor = cls->hasCxxCtor();
       bool hasCxxDtor = cls->hasCxxDtor();
       bool fast = cls->canAllocNonpointer();
   
       size_t size = cls->instanceSize(extraBytes);
       if (outAllocatedSize) *outAllocatedSize = size;
   
       id obj;
       if (!zone  &&  fast) {
           obj = (id)calloc(1, size);
           if (!obj) return nil;
           obj->initInstanceIsa(cls, hasCxxDtor);
       } 
       else {
           if (zone) {
               obj = (id)malloc_zone_calloc ((malloc_zone_t *)zone, 1, size);
           } else {
               obj = (id)calloc(1, size);
           }
           if (!obj) return nil;
   
           // Use raw pointer isa on the assumption that they might be 
           // doing something weird with the zone or RR.
           obj->initIsa(cls);
       }
   
       if (cxxConstruct && hasCxxCtor) {
           obj = _objc_constructOrFree(obj, cls);
       }
   
       return obj;
   }
   
   ```

   - hasCxxCtor()是判断当前class或者superclass是否有`.cxx_construct` 构造方法的实现

   - hasCxxDtor()是判断判断当前class或者superclass是否有`.cxx_destruct` 析构方法的实现

   - anAllocNonpointer()是具体标记某个类是否支持优化的isa

   - instanceSize()获取类的大小（传入额外字节的大小）

     > 已知zone=false，fast=true，则(!zone && fast)=true

   - initInstanceIsa()内部调用`initIsa(cls, true, hasCxxDtor)`初始化isa

   - calloc()用来动态开辟内存

5. 字节对齐

   ```c++
   #ifdef __LP64__
   #   define WORD_SHIFT 3UL
   #   define WORD_MASK 7UL
   #   define WORD_BITS 64
   #else
   #   define WORD_SHIFT 2UL
   #   define WORD_MASK 3UL
   #   define WORD_BITS 32
   #endif
   
   static inline uint32_t word_align(uint32_t x) {
       return (x + WORD_MASK) & ~WORD_MASK;
   }
   
   // May be unaligned depending on class's ivars.
   uint32_t unalignedInstanceSize() {
       assert(isRealized());
       return data()->ro->instanceSize;
   }
   
   // Class's ivar size rounded up to a pointer-size boundary.
   uint32_t alignedInstanceSize() {
       return word_align(unalignedInstanceSize());
   }
   
   size_t instanceSize(size_t extraBytes) {
       size_t size = alignedInstanceSize() + extraBytes;
       // CF requires all objects be at least 16 bytes.
       if (size < 16) size = 16;
       return size;
   }
   
   ```

   1. `data()->ro->instanceSize`就是获取这个类所有属性内存的大小。这里只有继承`NSObject`的一个属性`isa`——返回8字节
   2. `if (size < 16) size = 16` CoreFoundation需要所有对象之和至少是16字节
   3. `word_align`——64位系统下，对象大小采用8字节对齐。**对象大小为16字节，必定是8的倍数。运用空间换时间，8字节的指针数量多。**

#### 3、init和new

init什么也没做

关于子类中`if (self = [super init])`为什么要这么写——子类先继承父类的属性，再判断是否为空，如若为空没必要进行一系列操作了直接返回nil

new是先调alloc，再调init