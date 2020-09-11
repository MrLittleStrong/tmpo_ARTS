# [iOS安全](https://medium.com/swlh/how-to-make-an-ios-app-secure-831e310c79e2)

### Notes

1. 开启ATS

2. 使用SSLPinning防止中间人攻击

3. 使用KeyChain来存储而不是NSUserDefaults

4. 避免把机密信息放在代码仓库中而是通过配置文件或者环境变量在编译的时候注入。

5. 检测越狱，防止越狱机使用

6. 只在Debug模式下进行打印Log

7. 三方库使用时，必须对License、代码、安全进行检查

8. 文件数据保护

   - Complete Protection (NSFileProtectionComplete)
   - Protected Unless Open (NSFileProtectionCompleteUnlessOpen)
   - Protected Until First User Authentication (NSFileProtectionCompleteUntilFirstUserAuthentication)
   - No Protection (NSFileProtectionNone)

   尽量避免使用NoProtection，而使用NSFileProtectionCompleteUnlessOpen或NSFileProtectionCompleteUntilFirstUserAuthentication

9. 录屏

   某些场景下需要对录屏进行监控，防止录屏后把用户的某些信息传播出去。

10. 

