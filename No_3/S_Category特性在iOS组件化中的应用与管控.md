## Category 特性在 iOS 组件化中的应用与管控

### Links

[Category 特性在 iOS 组件化中的应用与管控](https://tech.meituan.com/2018/11/08/ios-category-module-communicate.html)

### Notes

This article actually is such so long for me to comprehend. First it introduced problems in component-based design. Then introduced four solutions to solve these problems:

1. Dependency injection.
2. SPI - Service Provider Interfaces.
3. Notification Center
4. Objc_msgSend

As they compared these four solutions they find SPI and objc_msgSend were much more potential, but still had some space to improve. 

Then they came up with two solutions:

5.  Category+NSInvocation
6. CategoryCoverOrigin

Two solution are much better, but still have some risks in coding. 

**Category may function errorly when there are functions that share the same name. **

 Then they use linkmap analyze to manage this risk. 

### Summary

Although they managed to control the risk, still I think this category thing is not a good solution for some different teams to maintain one central-control file.