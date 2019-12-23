# 美团点评移动端基础日志库——Logan

### Links

[美团点评移动端基础日志库——Logan](https://tech.meituan.com/2018/02/11/logan.html)

### Notes

这篇文章介绍了一个移动端日志库的思路。

解决卡顿，影响性能——使用C来写底层库，在C层使用流式压缩，加密。

日志丢失——mmap，直接映射

日志分散——本地聚合存储，通过自研的日志协议进行统一格式处理。上报之后进行反解。

架构如下：

![Logan的整体架构图](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2018a/889ed888.png)

分片——20k来分片

日志回捞——通过Push来触达用户

主动上报——客服引导用户

