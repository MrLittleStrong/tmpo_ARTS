# 【flutter】如何理解flutter的编译模式

### 背景

学了陈航的课记录下知识点。

### 知识点

- Flutter 编译模式为Debug、Profile和Release。
- Debug对应JIT模式，真机和模拟器上都能运行。会打开所有的assert，调试信心，服务扩展和调试辅助，支持Hot reload
- Release对应AOT模式，只能真机上运行。会关闭所有的assert，调试信息，服务扩展和调试辅助。还有话了快速启动，快速执行和二进制包的大小。
- Profile与Release模式一致，只是加了服务扩展支持，可以用于分析性能。
- 通过assert和kReleaseMode常量，可以识别APP编译环境，从而对代码进行微调。
- 作者通过`InheritedWidget`来实现了分环境配置能力，再通过`flutter build ios -t lib/main_dev.dart `来实现了不同的打包入口。
- 通过assert方式打包会在release构建中被删除，但是kReleaseMode方式的则会被打包进二进制中，虽然不会生效。

