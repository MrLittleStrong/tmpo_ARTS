## APP网络优化之DNS优化实践

### Links

[APP网络优化之DNS优化实践](http://www.cocoachina.com/articles/895979?filter=ios)

### Notes

1. 每日优鲜团队针对DNS解析劫持通过使用腾讯云的HTTPDNS的SDK的集成做了优化。

2. iOS接入SDK架构如下

   ![接入后架构如下](http://api.cocoachina.com/uploads//20200103/1578018842348862.png)

3. 如果对性能有很高的要求，同时又需要处理SNI场景的问题，我建议**不要直接主动使用HTTPDNS，而是在运营商LocalDNS获取的IP请求失败的情况下，可以在底层直接使用基于CFNetwork的网络请求进行重试**，这样就能在请求DNS劫持和性能中间得到一个平衡，既能保证在运营商的LocalDNS解析出现问题时能够走HTTPDNS，保证成功率和可用性；同时又能够在运营商的LocalDNS可用时，使用基于NSURLSession的请求，享受系统实现的HTTP2.0特性带来的性能提升。

   **如果不需要处理SNI的问题，就老老实实使用基于NSURLSession的实现方案**。

