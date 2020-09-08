# Flutter Performance Optimization&How to Cancel a Future in Flutter

### Links

[Flutter Performance Optimization](https://medium.com/flutterdevs/flutter-performance-optimization-17c99bb31553)

[How to Cancel a Future in Flutter](https://levelup.gitconnected.com/how-to-cancel-a-future-in-flutter-bb1f7ad19381)

### Notes

#### Flutter Performance

1. Isolate 是单线程的，是运行所有dart代码的地方，拥有自己的内存块和运行EventLoop的单一线程。

2. 如果需要可以的话可以用Isolate.spawan或者compute来创建新的Isolate。

3. Isolate互相联系是互相传递消息，即使是父Isolate也不能访问子Isolate。

4. event loop负责事件队列，将一个个事件添加在队列里顺序执行

5. compute方法使用

   ```dart
   var getData = await compute(function, parameter)
   ///需要两个参数,
   ///1. 一个future或者function，但是必须是static方法（因为他们没办法共享内存，所以必须是class level而不能是object level）
   ///2. 需要传给方法的argument，如果要多参数的话，需要把它转成map传入
   /// 返回参数是一个 future
   ```

6. 任何async-await和function其实都是在主线程去处理的，所以如果async-await一个sleep，还是会阻塞主线程。

#### Cancel a future

如果一个future想要获取用户的定位，假如获取失败了，他的return永远都无法满足，那就卡住了，只能重启app。

有两个方法

##### 设置超时时间

设置超时之后如果指定时间返回不了会返回一个`TimeoutException`

```dart
try {
  // TODO: show loading 
  fianl userLocation = await location.current.timeout(Duration(seconds: 30));
  // TODO: access userLocation here
} catch (error) {
  print("error");
  // TODO: handle future timed out or some other error
} finally {
  // TODO: hide loading
}
```

##### 手动取消

在某些情况下，我们需要手动取消一个future，例如input event。我们可以通过把future的方法`asStream`来把Future转成stream，然后可以cancel`StreamSubscription`。

```dart
StreamSubscription locationSub;
handleGetLocation() {
  // TODO: show loading
  locationSub = location.current.asStream().listen((location) {
    print(location);
    // TODO: hide loading
    // TODO: access location here
  });
};
handleCancelGetLocation() {
  sub?.cancel();
  // TODO: hide loading
}
```



##### 

