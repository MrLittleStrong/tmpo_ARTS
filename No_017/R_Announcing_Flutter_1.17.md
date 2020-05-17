# Announcing Flutter 1.17

### Links

[Announcing Flutter 1.17](https://medium.com/flutter/announcing-flutter-1-17-4182d8af7f8e)

### Notes

1. 今年会一个季度左右出一个stable的SDK，1.17主要包含了各种issue的修复，性能提升，及Dart2.8的引入。
2. 优化了CPU和GPU的利用，优化了快速滚动的大图的内存，优化了包体积。
3. iOS端默认使用Metal而不是默认OpenGL。
4. 增加了NavigationRail，DatePicker的widget，优化了text selection的menu不会overflow了，还做了一些widget到全屏过渡的动画组件。
5. 调整了Flutter的文字主题
6. 增加了Google Font（感觉国内可能不能用）
7. Accessibility修复了一堆有关Scrolliing和TextField之类的问题，Internationalization也修了一些有关东亚语言的问题。
8. Dart DevTools增加了Network日志；增加了Android的快速启动选项；AndroidX现在是默认选项了；增加了`flutter pub outdated`功能；现在可以用工具来上报flutter的崩溃问题。
