最近在封装网络库，其实是对[Dio库](https://pub.dev/packages/dio)进行了进一步的封装。

在调用dio的get和post的时候，发现回调里面的代码没有运行。

又回过头去看了Dart的语法书。大概了解了dart使用async异步编程，(Future还不太扎实）

使用`async`和`await`的代码是异步的，但是看起来很像同步代码。例如，以下代码使用`await`来等待异步函数的结果:

```dart
await lookUpVersion();
```

使用`await`，代码必须是在一个**异步函数(async function)**里-标记为`async`的异步函数:

```dart
Future checkVersion() async {
  var version = await lookUpVersion();
  // 用version变量做点什么
}
```

> 注意:虽然异步函数可能会执行耗时的操作，但它不会等待这些操作。相反，异步函数只在遇到它的第一个`await`表达式之前执行。然后，它返回一个Future的对象，只有在`await`表达式完成后，才恢复执行。

在`await 表达式`中，`表达式`的值通常是一个Future；如果不是，则该值将在Future中自动包装。这个Future的对象表示承诺返回一个对象。`await expression`的值就是那返回的对象。await表达式使程序运行停止，直到对象可用。

**使用await时，必须得用在async函数中，否则会编译错误。**

**如果我们要在函数里调用异步函数，那调用的时候异步函数前面基本都会await，而这个函数基本上也得是async了**