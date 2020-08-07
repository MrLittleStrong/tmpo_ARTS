# Flutter 1.20 发布

### Links

[Announcing Flutter 1.20](https://medium.com/flutter/announcing-flutter-1-20-2aaf68c89c75)

### Notes

Flutter 1.20包含了3029个PR，关闭了5485个issue，有359个贡献者。

谷歌商店现在有超过9万个flutter的app，现在印度成为了flutter的第一位的使用群体。Dart上升了4位成为了第12位使用 的语言。

#### 性能优化

1. 通过优化字体的build_tools，优化掉了没被引用的icon和font，这样可以减少100kb的包体积。
2. 增加动画预热阶段来让动画更加流畅。
3. 改善了桌面应用的鼠标支持。通过更快的hit-test，鼠标的类型可以根据输入框，按钮进行变化。
4. dart 2.9支持了更快的UTF-8decode。

#### 增加了手机TextField的自动填入

#### 增加了一个新的widget--InteractiveViewer

#### 更新了Material Slider, RangeSlider, TimePicker 和DatePicker.

#### 发布plugins现在有新的pubspec.yaml格式

#### 在Visual Studio 增加了Dart DevTools

#### 更新了网络请求跟踪记录

#### 更改文件路径会直接将引用的代码路径也进行调整

#### 增加了各个IDE对于颜色，icon等直接显示的支持

#### 增加了typesafe的platform channels提供给plugins和add to app

#### 增加了一系列的IDE的插件









