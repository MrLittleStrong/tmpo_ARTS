### 背景

这篇文章为《如何利用Clang为App提质》学习心得

### 知识点

1. Clang可以开发静态分析工具、代码增量分析、代码可视化、代码质量报告等。例如[CodeChecker](https://github.com/Ericsson/codechecker)

2. iOS编译过程

   OC使用Clang作为前端，swift使用swift()作为前端。LLVM作为后端。

   编译过程为：Clang->LLVM Optimizer->LLVM Code Generator.

   编译器前端任务：语法分析（输出token流），语义分析（生成AST），生成中间代码IR（Intermediate representation）

3. Clang提供了LibClang、Clang Plugin和LibTooling来支持语法分析，语义分析。

4. LibClang可以访问Clang的上层高级抽象的能力，如获取所有的Token，遍历语法树，代码补全等。API很稳定，Clang升级没有影响。但是不能完全访问Clang AST信息。

5. Clang Plugin可以在AST上做些操作，并能够集成到编译中，成为编译的一部分。一般用来控制Clang AST，影响编译过程，进行中断或者提示。

6. LibTooling能够编写独立运行的语法检查和代码重构工具。通过LibTooling写的工具不依赖于构建系统，可以作为一个命令单独使用；可以完全控制AST，可以和Clang Plugins共用一份代码。但它无法影响编译过程，也没有那么稳定。在LibTooling上有个Clang Tools包含了clang-checker, clang-fixit,clang-format等工具。



