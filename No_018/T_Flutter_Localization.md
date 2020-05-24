# 适配国际化

### 背景

学了陈航的课，记录下知识点

### 知识点

1. 适配国际化，我们不能只关注文案，还有图片。

2. Flutter官方的文本国际化方案

   - 实现一个LocalizationsDelegate，将所有需要翻译的文案注明成它的属性
   - 对需要支持的语言地区进行翻译适配
   - 在MaterialApp初始化时，把这个delegate设置为翻译的回调。
   
3. 官方的方案，量大，繁琐还容易出错。我们可以用Android Studio中的Flutter i18n插件来完成。它依赖flutter_localizations插件，需要在pubspec.yaml里声明.

4. Flutter i18n用法

   - 添加依赖配置
   - res文件夹下 values/string_en.arb的JSON格式文件进行修改，同步会生成对应的代码
   - 支持中文的话需要增加string_zh.arb文件，进行修改。可以支持资源标识符属性。
   - 在MaterialApp的初始化添加翻译回调等配置。
   - 在iOS上需要配置多语言，否则不生效。

   ```dart
   
   //应用程序入口
   class MyApp extends StatelessWidget {
     @override
     Widget build(BuildContext context) {
       return MaterialApp(
         localizationsDelegates: const [
           S.delegate,//应用程序的翻译回调
           GlobalMaterialLocalizations.delegate,//Material组件的翻译回调
           GlobalWidgetsLocalizations.delegate,//普通Widget的翻译回调
         ],
         supportedLocales: S.delegate.supportedLocales,//支持语系
         //title的国际化回调
         onGenerateTitle: (context){
           return S.of(context).app_title;
         },
         home: MyHomePage(),
       );
     }
   }
   ```

5. 应用名称我们需要在原生进行配置

