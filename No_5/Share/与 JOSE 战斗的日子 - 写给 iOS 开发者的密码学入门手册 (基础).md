## 与 JOSE 战斗的日子 - 写给 iOS 开发者的密码学入门手册 (基础)

### Links

[与 JOSE 战斗的日子 - 写给 iOS 开发者的密码学入门手册 (基础)](https://onevcat.com/2018/12/jose-1/)

### Notes

This article introduced JWT and JOSE.

JWT is an encoded string made of three part and combined by ```.```. Each part is a string encoded by Base64Url. First part is Header contains algorithm and Key Id. Second part is payload part contains data such as issuer, subject expiration time and issued time. Then the signature, firstly Base64Url the Header and Payload parts. Use ```.``` to combine them and sign it. Then Base64Url the result. So we can get the signature. 

JOSE (Javascript Object Signing and Encryption) made a series of standards of how to use JSON in transportation of network. Leads to two concept—— JWK and JWA. JWK  (JSON Web Key) solved how to use JSON to present a secret key. JWA (JSON Web Algorithms) defined algorithms.



### Summary

Learnd some cryptography knowledge. 

