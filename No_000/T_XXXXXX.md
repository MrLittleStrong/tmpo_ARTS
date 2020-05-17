# 如何通过自动化测试提高交付质量

### 背景

学了陈航的课，记录下知识点

### 知识点

1. 可以用`test`包来编写单元测试用例，`test`包放在`dev_dependencies`下

2. 编写测试用例

   ```dart
   
   import 'package:test/test.dart';
   import 'package:flutter_app/main.dart';
   
   void main() {
     //第一个用例，判断Counter对象调用increase方法后是否等于1
     test('Increase a counter value should be 1', () {
       final counter = Counter();
       counter.increase();
       expect(counter.value, 1);
     });
     //第二个用例，判断1+1是否等于2
     test('1+1 should be 2', () {
       expect(1+1, 2);
     });
   }
   ```

3. 可以用`group`把多个测试用例组合在一起

   ```dart
   
   import 'package:test/test.dart';
   import 'package:counter_app/counter.dart';
   void main() {
     //组合测试用例，判断Counter对象调用increase方法后是否等于1，并且判断Counter对象调用decrease方法后是否等于-1
     group('Counter', () {
       test('Increase a counter value should be 1', () {
         final counter = Counter();
         counter.increase();
         expect(counter.value, 1);
       });
   
       test('Decrease a counter value should be -1', () {
         final counter = Counter();
         counter.decrease();
         expect(counter.value, -1);
       });
     });
   }
   ```

4. 可以使用`mockito`来mock外部依赖的数据

   ```dart
   dev_dependencies: 
      test: 
      mockito:
   
   test('returns a Todo if successful', () async { 
     final client = MockClient(); //使用Mockito注入请求成功的JSON字段 
     when(client.get('https://xxx.com/todos/1'))
       .thenAnswer((_) async => http.Response('{"title": "Test"}', 200)); 
     //验证请求结果是否为Todo实例 
     expect(await fetchTodo(client), isInstanceOf());
   });
   ```

5. ui test 也可以，不过要考虑成本，因为ui变动很频繁，维护这样的测试用例很容易得不偿失

   使用`testWidgets`来对widget的测试用例封装，然后要用`tester`来进行测试

   ```dart
   
   import 'package:flutter_test/flutter_test.dart';
   import 'package:flutter_app_demox/main.dart';
   
   void main() {
     testWidgets('Counter increments UI test', (WidgetTester tester) async {
     //声明所需要验证的Widget对象(即MyApp)，并触发其渲染
     await tester.pumpWidget(MyApp());
   
     //查找字符串文本为'0'的Widget，验证查找成功
     expect(find.text('0'), findsOneWidget);
     //查找字符串文本为'1'的Widget，验证查找失败
     expect(find.text('1'), findsNothing);
   
     //查找'+'按钮，施加点击行为
     await tester.tap(find.byIcon(Icons.add));
     //触发其渲染
     await tester.pump();
   
     //查找字符串文本为'0'的Widget，验证查找失败
     expect(find.text('0'), findsNothing);
     //查找字符串文本为'1'的Widget，验证查找成功
     expect(find.text('1'), findsOneWidget);
   });
   }
   ```

   

