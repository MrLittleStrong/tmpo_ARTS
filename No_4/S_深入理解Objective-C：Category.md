## 深入理解Objective-C：Category

### Links

[深入理解Objective-C：Category](https://tech.meituan.com/2015/03/03/diveintocategory.html)

### Notes

This article introduced three parts:

1. what can category do, How we use category in coding.
2. How category works. Especially introduced how it works with (+)load method.
3. How can we add properties in category and how it works.

### Summary

What I get from this article:

1. Rewrite objc file operation line:

   ```shell
   clang -rewrite-objc MyClass.m
   ```

2. when meet with +load method.  first excute class method, then category method. If there are more than 1 category, it will depend on the sequence of Compile Sources( higher file loads earlier)

3. Category can use association to add property. However the property is not added to class stuct. Instead, it is managed by AssociationsManager, saved in a global map——AssociationsHashMap. Map's key is pointer address of this object, and value is another AssociationsHashMap which saved all kv of the associated object . As to memory management, system uses _object_remove_assocations methods

   ```c++
   void *objc_destructInstance(id obj) 
   {
       if (obj) {
           Class isa_gen = _object_getClass(obj);
           class_t *isa = newcls(isa_gen);
   
           // Read all of the flags at once for performance.
           bool cxx = hasCxxStructors(isa);
           bool assoc = !UseGC && _class_instancesHaveAssociatedObjects(isa_gen);
   
           // This order is important.
           if (cxx) object_cxxDestruct(obj);
           if (assoc) _object_remove_assocations(obj);
           
           if (!UseGC) objc_clear_deallocating(obj);
       }
   
       return obj;
   }
   ```

   