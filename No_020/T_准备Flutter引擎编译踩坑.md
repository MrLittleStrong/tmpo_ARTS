# 准备Flutter引擎编译踩坑

### 背景

由于flutter官方修复到最后输出stable的sdk时间周期太长，所以准备自己搞引擎，提前引入修复来搞。

### 过程

##### 准备工具

- git
- ssh
- [depot_tools](https://www.chromium.org/developers/how-tos/depottools/gclient)，使用其中的`gclient`，通过它来获取所有编译需要的源码和依赖
- [ninja](https://ninja-build.org/ )，负责最终编译
- [gn](https://gn.googlesource.com/gn)，负责生成ninja编译需要的build文件

##### 准备过程

1. fork `https://github.com/flutter/engine`到自己的github账户

2. 设置github的ssh keys。

3. 创建一个空文件夹来当工作台。

4. 创建`.gclient`文件。内容类似于下面去填写

   ```
   solutions = [
     {
       "managed": False,
       "name": "src/flutter",
       "url": "git@github.com:<your_name_here>/engine.git",
       "custom_deps": {},
       "deps_file": "DEPS",
       "safesync_url": "",
     },
   ]
   ```

5. `gclient sync`，会把源码和依赖拉取下来

6. 增加一个远端的源，主要是为了后续跟flutter engine源进行同步。

   - `cd src/flutter`
   - `git remote add upstream git@github.com:flutter/engine.git`
   - `cd ..`

7. Mac 安装JDK和ant

### 坑

1. gclient被墙，这个还好，翻墙解决了。

2. `gclient sync`命令时间太长且容易断。这一步各种设置代理。这一步尝试了两种办法。

   - 设置终端的代理`export http_proxy=http://127.0.0.1:1081;export https_proxy=http://127.0.0.1:1081;`
   - 修改hosts，直连github。通过在`https://www.ipaddress.com/`查询`github.com`和`github.global.ssl.fastly.net`的ip，在hosts文件里直接填上，刷新dns缓存`sudo killall -HUP nDNSResponder`。

   整个过程尝试了差不多两天多。是在太慢了。





