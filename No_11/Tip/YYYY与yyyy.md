# YYYY与yyyy

今天看了一个文章[就在几天前，听说用了 YYYY-MM-dd 的程序员，都在加班改 Bug !](https://mp.weixin.qq.com/s?__biz=MzAxMTI4MTkwNQ==&mid=2650829714&idx=1&sn=40fd86a62b55059aff4bc9c09db36608&chksm=80b7a70cb7c02e1a32e6206efebb31d4f008c3fb1a777c376a006ed86b7ee7249951a9d12e42&mpshare=1&scene=1&srcid=&sharer_sharetime=1579018234912&sharer_shareid=e8873157136b90f0eab5535a82650fb6&rd2werd=1#wechat_redirect)

把yyyy-MM-dd写成 YYYY-MM-dd，会导致跨年的周的年份解析出错。

我进行了尝试

```objective-c
/// NSDate转成字符串的时候yyyy表示正正经经的年份，而YYYY表示如果有跨年的周即为下一年。
- (void)testYYYYString {
    //2019-12-31
    NSDate *date = [NSDate dateWithTimeIntervalSince1970:1577779666];
    NSDateFormatter *yyyyFormatter = [[NSDateFormatter alloc]init];
    [yyyyFormatter setDateFormat:@"yyyy-MM-dd"];
    NSDateFormatter *YYYYFormatter = [[NSDateFormatter alloc]init];
    [YYYYFormatter setDateFormat:@"YYYY-MM-dd"];
    NSLog(@"2019-12-31 \nyyyy:%@\nYYYY:%@",[yyyyFormatter stringFromDate:date],[YYYYFormatter stringFromDate:date]);
}
```

输出

```
yyyy:2019-12-31
YYYY:2020-12-31
```

![image](http://mmbiz.qpic.cn/mmbiz_png/FjoD2pNz8FHoMlJ3DsOEnwicqvb5sXibyI8aeZDdDoHymiclg929icm9SFvdwMxywiaRTyCa927VDUqficNmKqdU3xDg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)