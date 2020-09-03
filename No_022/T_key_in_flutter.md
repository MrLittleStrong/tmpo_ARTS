# Flutter中的Key

### 背景

看了[掘金上的文章](https://juejin.im/post/6844903811870359559)。记录下知识点。

### 知识点

##### widget更新机制

```dart
@immutable
abstract class Widget extends DiagnosticableTree {
  const Widget({ this.key });
  final Key key;
  ···
  static bool canUpdate(Widget oldWidget, Widget newWidget) {
    return oldWidget.runtimeType == newWidget.runtimeType
        && oldWidget.key == newWidget.key;
  }
}

```

canUpdate对两个widget的runtimeType和key进行比较，从而判断是否需要更新，如果为true则说明不需要更新Element，直接更新widget就行了。

##### 例子：

展示两个 StatelessContainer 小部件，当我们点击 floatingActionButton 时，将会交换它们的顺序，然而将其改为StatefulContainer时，不生效，加uniqueKey生效。

##### StatelessContainer比较过程

runtimeType一致，key没传，所以canUpdate返回true，两个widget交换了位置，StatelessElement调用新持有的Widget的build方法重新构建，而color储存在widget中，因此可以互换。

##### StatefulContainer比较过程

color放在State中，widget不保存state，持有state的是Stateful Element。

runtimeType一致，key没传，所以canUpdate返回true，两个widget交换了位置，而element不用动。但是原来的element只会从它持有的state实例中build新的widget。因为element没变，所以state也没变。所以颜色不会变。

而我们用了key之后，canUpdate为false了，这时候RenderObjectElement会用新的widget的key在老element列表里找，找到后悔更新Element位置并更新对应renderObject位置。在这里就是交换了Element的位置，交换了renderObject的位置。所以颜色变了。

##### 比较范围

比较算法是有范围的，之比较同一层级的widget。

如果StatefulContainer上层包了Padding，会被重新创建。

##### 扩展内容

slot 能够描述子级在其父级列表中的位置。多子部件 Widget 例如 Row，Column 都为它的子级提供了一系列 slot。

在调用 Element.updateChild 的时候有一个细节，若新老 Widget 的实例相同，注意这里是**实例相同**而不是类型相同， slot 不同的时候，Flutter 所做的仅仅是更新 slot，也就给他换个位置。

|             | 新widget为空             | 新widget不为空                                               |
| ----------- | ------------------------ | ------------------------------------------------------------ |
| child为空   | 返回null                 | 返回新的element                                              |
| child不为空 | 移除旧的widget，返回null | 如果旧的child Element的canUpdate为true，则更新并将其返回，否则返回一个新的Element。 |

#### key的种类

##### key

```dart
@immutable
abstract class Key {
  const factory Key(String value) = ValueKey<String>;

  @protected
  const Key.empty();
}
```

默认创建 Key 将会通过工厂方法根据传入的 value 创建一个 ValueKey。

Key 派生出两种不同用途的 Key：LocalKey 和 GlobalKey。

##### ValueKey

如果您有一个 Todo List 应用程序，它将会记录你需要完成的事情。我们假设每个 Todo 事情都各不相同，而你想要对每个 Todo 进行滑动删除操作。

这时候就需要使用 ValueKey！

```dart
return TodoItem(
    key: ValueKey(todo.task),
    todo: todo,
    onDismissed: (direction){
        _removeTodo(context, todo);
    },
);
```

##### ObjectKey

如果你有一个生日应用，它可以记录某个人的生日，并用列表显示出来，同样的还是需要有一个滑动删除操作。

我们知道人名可能会重复，这时候你无法保证给 Key 的值每次都会不同。但是，当人名和生日组合起来的 Object 将具有唯一性。

这时候你需要使用 ObjectKey！

##### UniqueKey

如果组合的 Object 都无法满足唯一性的时候，你想要确保每一个 Key 都具有唯一性。那么，你可以使用 UniqueKey。它将会通过该对象生成一个具有唯一性的 hash 码。

不过这样做，每次 Widget 被构建时都会去重新生成一个新的 UniqueKey，失去了一致性。也就是说你的小部件还是会改变。（还不如不用😂）

##### PageStorageKey

当你有一个滑动列表，你通过某一个 Item 跳转到了一个新的页面，当你返回之前的列表页面时，你发现滑动的距离回到了顶部。这时候，给 Sliver 一个 PageStorageKey！它将能够保持 Sliver 的滚动状态。

##### GlobalKey

GlobalKey 能够跨 Widget 访问状态。 在这里我们有一个 Switcher 小部件，它可以通过 changeState 改变它的状态。

```dart
class SwitcherScreenState extends State<SwitcherScreen> {
  bool isActive = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: Switch.adaptive(
            value: isActive,
            onChanged: (bool currentStatus) {
              isActive = currentStatus;
              setState(() {});
            }),
      ),
    );
  }

  changeState() {
    isActive = !isActive;
    setState(() {});
  }
}
```

但是我们想要在外部改变该状态，这时候就需要使用 GlobalKey。

```dart
class _ScreenState extends State<Screen> {
  final GlobalKey<SwitcherScreenState> key = GlobalKey<SwitcherScreenState>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: SwitcherScreen(
        key: key,
      ),
      floatingActionButton: FloatingActionButton(onPressed: () {
        key.currentState.changeState();
      }),
    );
  }
}
```

这里我们通过定义了一个 GlobalKey 并传递给 SwitcherScreen。然后我们便可以通过这个 key 拿到它所绑定的 SwitcherState 并在外部调用 changeState 改变状态了。