## 与 JOSE 战斗的日子 - 写给 iOS 开发者的密码学入门手册 (理论)

### Links

[与 JOSE 战斗的日子 - 写给 iOS 开发者的密码学入门手册 (理论)](https://onevcat.com/2018/12/jose-2/)

### Notes

This article introduced Secret Key and Progress of sign and verification.

Secret Key is wrote in a file by pure ASCII bit. It is encoded by Base64. Decode it we can get a  binary data. Then DER can help us to understand it. 

The Progress of sign and verification is like below (RSA)

1. Server use summary algorithms like SHA-256 to substract data from origin data.  Then use Secret key and sign algorithms like RSA to sign substracted data. 
2. Client use summary algorithms that server used to substract data from origin data. And use Public key and verification algorithms that server used to decode the signature server sent. Then client will get two substracted data( one from origin data, one from decode). Compare them, we can verify the data is correct or not.

For ECDSA, signature contains two big integer. It compares integers to decide whether is correct.

### Summary

Learnd some cryptography knowledge. 

