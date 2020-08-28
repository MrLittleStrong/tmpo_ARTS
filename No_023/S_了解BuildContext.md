# 了解BuildContext

### Notes

##### Navigator和MaterialApp

- Navigator 负责管理维护页面堆栈的
- 我们在构建应用时没有手动创建Navigator，也能进行页面导航。这是因为Navigator由MaterialApp提供，但是如果home，routes，onGenerateRoute和onUnknownRoute都为null，并且builder不为null，MaterialApp不会创建任何Navigator。

##### BuildContext

```dart
abstract class BuildContext {
  /// The current configuration of the [Element] that is this [BuildContext].
  Widget get widget;

  /// The [BuildOwner] for this context. The [BuildOwner] is in charge of
  /// managing the rendering pipeline for this context.
  BuildOwner get owner;
  ...
```

##### Flutter如何构建视图

Widget只是一个配置信息，永远都是immutable的，而且可以在多处重复使用。真正显示在屏幕上的视图树是Element Tree。

```dart
abstract class StatelessWidget extends Widget {
  const StatelessWidget({ Key key }) : super(key: key);
  @override
  StatelessElement createElement() => StatelessElement(this);
  ...
```

当要把StatelessWidget装进视图树时，会先createElement，然后将当前Widget传给Element。

```dart
class StatelessElement extends ComponentElement {
  /// Creates an element that uses the given widget as its configuration.
  StatelessElement(StatelessWidget widget) : super(widget);

  @override
  StatelessWidget get widget => super.widget;

  @override
  Widget build() => widget.build(this);

  @override
  void update(StatelessWidget newWidget) {
    super.update(newWidget);
    assert(widget == newWidget);
    _dirty = true;
    rebuild();
  }
}
```

通过将widget传入StatelessElement的构造函数，StatelessElement保留了widget的引用，并且会调用build方法。而build方法调用的widget的build方法，并将StatelessElement对象传进去。

```dart
class StatelessElement extends ComponentElement
...
abstract class ComponentElement extends Element
...
abstract class Element extends DiagnosticableTree implements BuildContext 

```

实际上是Element实现了BuildContext，并由ComponentElement->StatelessElement继承。

BuildContext实际上就是Element对象，BuildContext接口用于阻止对Element对象直接操作。

##### 视图树装载过程

**StatelessWidget**

- 先调用StatelessWidget的CreateElement方法，并根据Widget生成StatelessElement对象
- 将StatelessElement对象挂载到element树上
- StatelessElement调用widget的build方法，并将element自身作为BuildContext传入。

**StatefulWidget**

- 先调用StatefulWidget的createElement方法，并根据widget生成StatefulElement对象，且保留widget的引用。
- 将StatefulElement挂载到Element树上
- 根据widget的createState 创建State
- StatefulElement对象调用state的build方法，并将element自身作为BuildContext传入。

**我们在build函数中的context，就是widget创建的element对象**

##### of(context)方法

我们经常会用到这样的代码

```dart
//打开一个新的页面
Navigator.of(context).push
//打开Scaffold的Drawer
Scaffold.of(context).openDrawer
//获取display1样式文字主题
Theme.of(context).textTheme.display1

```

关键代码

```dart
static NavigatorState of(
    BuildContext context, {
      bool rootNavigator = false,
      bool nullOk = false,
    }) {
//关键代码-----------------------------------------v
    
    final NavigatorState navigator = rootNavigator
        ? context.rootAncestorStateOfType(const TypeMatcher<NavigatorState>())
        : context.ancestorStateOfType(const TypeMatcher<NavigatorState>());
        
//关键代码----------------------------------------^
    assert(() {
      if (navigator == null && !nullOk) {
        throw FlutterError(
          'Navigator operation requested with a context that does not include a Navigator.\n'
          'The context used to push or pop routes from the Navigator must be that of a '
          'widget that is a descendant of a Navigator widget.'
        );
      }
      return true;
    }());
    return navigator;
  }

```

可以看到，关键代码部分就是通过`context.ancestorStateOfType`向上遍历Element Tree，找到最近匹配的NavigatorState。也就是说of是对context跨组件获取数据的封装。

BuildContext还有很多方法可以跨组件获取对象

```dart
ancestorInheritedElementForWidgetOfExactType(Type targetType) → InheritedElement

ancestorRenderObjectOfType(TypeMatcher matcher) → RenderObject

ancestorStateOfType(TypeMatcher matcher) → State

ancestorWidgetOfExactType(Type targetType) → Widget

findRenderObject() → RenderObject

inheritFromElement(InheritedElement ancestor, { Object aspect }) → InheritedWidget

inheritFromWidgetOfExactType(Type targetType, { Object aspect }) → InheritedWidget

rootAncestorStateOfType(TypeMatcher matcher) → State

visitAncestorElements(bool visitor(Element element)) → void

visitChildElements(ElementVisitor visitor) → void

```

需要注意的是，如果我们需要与祖先 Inherit 对象建立长期联系，`dependOnInheritedWidgetOfExactType` 系列的方法不能在 `initState` 中调用，为了确保 Widget 在 Inherit 值更改时正确更新自身，请在 `State.didChangeDependencies` 阶段调用 `of` 方法。

##### 问题解释

```dart
class  MyApp  extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        body: Center(
          child: FlatButton(
              onPressed: () {
                Navigator.of(context).push(
                    MaterialPageRoute(builder: (context) => SecondPage()));
              },
              child: Text('跳转')),
        ),
      ),
    );
  }
}

```

跳转中的context是App这个widget的element对象，而of往上找祖先节点的时候没有MaterialApp，也没有Navigator，所以会报错。

