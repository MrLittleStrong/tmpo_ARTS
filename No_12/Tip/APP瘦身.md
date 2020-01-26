# APP瘦身

### 背景

学了戴铭的APP瘦身课，总结一下。

### 知识点

1. APP Store规定超过150MB的APP无法通过OTA环境下载，只能通过WIFI。

2. 兼容iOS7或者iOS8的话，主二进制text段不能超过60MB。

3. 方法包括：

   - APP Thinning
   - 图片资源优化
   - 代码优化

4. APP Thinning 有三种方式：APP Slicing、bitcode、On-Demand Resources。

   - APP Slicing——切割APP，创建不同的变体。（例如图片资源及库文件进行拆分）
   - On-Demand Resources——游戏使用的，根据用户关卡进度下载，还可以删除。
   - bitcode——针对特定设备进行包大小优化，不明显。

5. 图片资源优化包括：删除无用图片、图片压缩。

6. 删除无用图片步骤：

   - find命令找到所有资源
   - 使用正则匹配
   - 删除使用的

7. 删除无用图片可以用LSUnusedResources工具。

8. 图片资源压缩可以将图片转为WebP的方式。如果图片超过100KB考虑使用webP，

9. WebP

   优点：

   - 压缩率高
   - 支持Alpha透明和24bit颜色数

   缺点：需要用libwebp，CPU消耗和解码时间上会比PNG高两倍。

10. 转webP工具有：

    - cwebp——google
    - iSParta——腾讯

11. 代码瘦身：删掉无用代码。方法：LinkMap，APPCode。