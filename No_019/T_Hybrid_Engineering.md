# Flutter混合工程

### 背景

看了闲鱼技术部的《Flutter技术解析与实战》，记录下知识点

### 知识点

##### 1. 混合工程研发体系介绍

- 混合工程下的Flutter研发结构。
- 工程结构。已有的Native工程如何引入Flutter，工程结构如何组织，如何管理Flutter环境，如何编译构建和集成打包等。
- 构建优化。如何针对Flutter的工具链进行调试和优化（主要针对Android构建缓慢）
- Native启动下的Flutter调试。
- Native启动下的Flutter热重载
- 联合调试。同时调试Flutter和Android/iOS
- 持续集成。

##### 2. 混合工程下的Flutter研发结构

![image-20200727111109424](/Users/gzh/Library/Application Support/typora-user-images/image-20200727111109424.png)

##### 3. Native启动下的Flutter调试

在flutter启动下做的工作主要包括：

1. 检查是否需要重新生成flutter_tools.snapshot
2. 基于pubspec.yaml获取依赖，并生成插件描述文件.flutter-plugins和pubspec.lock
3. 基于flutter配置生成Generated.xcconfig和local.properties.
4. 基于Xcodebuild和Gradle构建应用
5. 基于lldb和adb启动应用
6. 等待应用中的flutter启动，寻找observatory端口，通过dart debugger连接以便调试
7. 寻找到端口后同步hot reload依赖的文件，同时通过Daemon监听命令（如用户点击插件按钮）实现Full Restart或者hot reload。

我们只需要考虑6和7步骤就可以实现优化调试，具体步骤如下：

1. 在iOS设备上通过`idevicesyslog | grep listening`寻找Observatory端口，可以看到iOS设备上的端口和认证码。
2. 通过iproxy将iOS设备上的端口映射到本机端口`iproxy 本机端口 iOS设备端口 iOS设备的uuid`，之后就可以通过http://127.0.0.1:本机端口/认证码/#/vm，来打开Observatory。
3. 可以通过配置IDE链接去调试，配置Dart Remote Debug。为了避免出现因为认证码造成的无法连接的问题，启动时需要传入`--disable-service-auth-codes`标志。
4. 单击调试按钮，链接到调试端口。成功后可以看到Debugger显示connected。

##### Native启动下的热重载

1. 启动App，进入Flutter页面，查找Obervatory端口x和认证码y。
2. 在flutter工程目录下，执行`flutter attach --debug-uri=http://127.0.0.1:x/y/`
3. 修改dart源码，然后在terminal里输入r就可以热重载了。
4. Android通过IDE Logcat或者`ADB Logcat|grep Observatory`获取端口，端口转发使用`adb forward`。

##### Native与flutter联调

可以使用Android Studio的`Attach Debugger to Android Process`调试Android，同样，结合Xcode的`Attach to Process`,可以实现iOS与Flutter联调

##### 持续集成

1. 一台公共设备安装Flutter环境并负责Flutter的构建
2. 构建产物aar或pod库集成到Native工程
3. ci平台通过产物方式继承flutter并打包。

##### 混合工程改造实践

两种依赖引入策略

1. **本地依赖**。

   优点：将flutter相关内容改动同步比较方便

   缺点：flutter原有构造流程改动较为复杂，后续flutter代码合并会有冲突。

2. **远端依赖**

   优点：flutter构建流程改动较小，本地不耦合

   缺点：同步变得更加繁琐，需要先同步到远端，再同步到Native。

策略上还是使用远端依赖，通过一系列脚本工具，将产物复制到远端仓库，提交并打好tag，然后根据标签生成新的远程依赖说明。

##### 快速完成混合工程搭建

flutter-boot 

> 就我感觉而言，并不怎么好用

##### 使用混合栈框架开发

flutter_boost



