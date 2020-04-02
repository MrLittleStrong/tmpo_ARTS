# iOS逆向-APP重签名

### Links

[iOS逆向-APP重签名](https://juejin.im/post/5e8443636fb9a03c3c350c71)

### Notes

1. 苹果使用双重签名机制。苹果存在公钥A和私钥A，Mac电脑存在公钥M和私钥M。
   - CSR文件上传，取出公钥M，使用私钥A对公钥M签名，得到描述文件，描述文件包括开发证书，bundleid，deviceid和权限文件。
   - mac打包app，将私钥M加密APP，得到ipa，包括MachO文件，APP签名文件及描述文件
   - 公钥A保存在iPhone里，通过公钥A解开ipa的描述文件里的证书得到公钥M（bundleid验证）
   - 公钥M进行hash，对比证书的hash（描述文件的验证）
   - 公钥M对加密APP解密，得到APP（APP签名验证）
2. **重签名不要用自己账号登录，可能会封号**
3. 可以使用双层签名原理对APP（越狱后的）进行重签，使用LLDB可以进行调试。

