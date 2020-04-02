# @

### Links

[@](https://nshipster.com/at-compiler-directives/)

### Notes

This article introduced almost all of the ```@```  usage in ObjC. I put them below.

- Interface & Implementation

- Properties(property, synthesize, dynamic)

- Property Attributes

- Forward Class Declarations

- Instance Variable Visibility ( public, protected, private )

- Protocols ( and Requirement Options)

- Exception Handling

- Literals(Object Literals: NSString, Objective-C Literals:@selector)

- C Literals (@encode(), @defs())

- Optimizations(@autoreleasepool, @synchronized)

- **Compatibility** (which I didn't know before)

  ```objc
  #if __IPHONE_OS_VERSION_MAX_ALLOWED < 60000
  @compatibility_alias UICollectionViewController PSTCollectionViewController;
  @compatibility_alias UICollectionView PSTCollectionView;
  @compatibility_alias UICollectionReusableView PSTCollectionReusableView;
  @compatibility_alias UICollectionViewCell PSTCollectionViewCell;
  @compatibility_alias UICollectionViewLayout PSTCollectionViewLayout;
  @compatibility_alias UICollectionViewFlowLayout PSTCollectionViewFlowLayout;
  @compatibility_alias UICollectionViewLayoutAttributes PSTCollectionViewLayoutAttributes;
  @protocol UICollectionViewDataSource <PSTCollectionViewDataSource> @end
  @protocol UICollectionViewDelegate <PSTCollectionViewDelegate> @end
  #endif
  ```

- Availability(@available)

