# 【flutter】如何通过工具链优化开发调试效率

### 背景

学了陈航的课记录下知识点。

### 知识点

1. 输出日志，使用debugPrint替换print，将生产的print隐藏掉

2. 断点调试。

3. 布局调试可以开启debug painting，就可以显示辅助线。

   ```dart
   import 'package:flutter/rendering.dart';
   void main() { 
     debugPaintSizeEnabled = true; //打开Debug Painting调试开关 
     runApp(new MyApp());
   }
   ```

   还可以用Flutter Inspector

