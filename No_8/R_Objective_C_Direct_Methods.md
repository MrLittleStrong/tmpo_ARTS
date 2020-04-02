# Objective-C Direct Methods

### Links

[Objective-C Direct Methods](https://nshipster.com/direct/)

### Notes

Objective-C adds a new feature ——Direct Methods Attribute. 

```objective-c
@interface MyClass: NSObject
@property(nonatomic) BOOL dynamicProperty;
@property(nonatomic, direct) BOOL directProperty;

- (void)dynamicMethod;
- (void)directMethod __attribute__((objc_direct));
@end
```

> A dynamic method can’t be overridden in a subclass by a direct method, and a direct method can’t be overridden at all.
>
> Protocols can’t declare direct method requirements, and a class can’t implement a protocol requirement with a direct method.

**In most cases, making a method direct probably won’t have a noticeable performance advantage.**

Guessing this feature may intend to reducing binary size.  Reportedly, the weight of unused Objective-C metadata can account for 5 – 10% of the `__text` section in the compiled binary.

So, this feature can be used to tighten SDK frameworks.