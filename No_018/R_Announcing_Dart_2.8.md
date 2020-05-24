# Announcing Dart 2.8

### Links

[Announcing Dart 2.8]()

### Notes

1. 2.8在为了null的安全性铺路。这个是一个很大的工程量，一方面要权衡类型检查与null安全性两个的取舍，另一方面也要考虑效率性。

   这点我深有同感，很多情况在后端返回的字段里有大量的null，每个地方都要进行判断，要写很多安全性的代码，这点是很烦的。

Codes as below:

2. 更高效地`pub get`；还增加了`pub outdated`命令来检查pub的版本情况。

