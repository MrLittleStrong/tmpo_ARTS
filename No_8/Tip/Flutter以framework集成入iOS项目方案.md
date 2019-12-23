### 现状

Flutter官方的执行方案对Flutter工程及环境有很强的依赖性，非Flutter的成员在对iOS主工程进行迭代开发时需要依赖Flutter环境，团队合作十分不便。在通过Jenkins等打包时会有各种问题。

[1]: https://github.com/flutter/flutter/wiki/Add-Flutter-to-existing-apps#experiment-turn-the-flutter-project-into-a-module	"Add Flutter to existing apps"

> 基于Flutter版本**1.9.1+hotfix 4**
>
> 适用于以flutter module进行开发，作为iOS一个组件来引入的情况

（可以直接跳至方案查看结果）

### 期望

flutter 集成进iOS项目脱离Flutter环境及工程，Flutter工程开发与iOS原生开发互不影响。

### 分析

#### Flutter执行build之后产物介绍

在Flutter的module中执行```flutter build ios --release```后，我们的工程目录里有隐藏文件夹```.ios```，我们需要的Flutter产物基本都在其下的Flutter文件夹中。

![image-20191031164025401](/Users/gzh/Library/Application Support/typora-user-images/image-20191031164025401.png)

​	挨个分析一下内部我们需要的文件。

- **.symlinks**

  我们三方库的索引，内部每个文件夹下，都包含```ios```文件夹，这个文件夹下都是一个pod库。

- **App.framework**

  Flutter项目的Dart代码编译而成的Framework。

- **engine**

  Flutter的引擎Framework，一个pod库。

- **FlutterPluginRegistrant**

  Flutter三方库的注册入口，一个pod库。

- **Generated.xcconfig**

  Flutter的路径信息，配置信息等。

- **podhelper.rb**

  pod执行的时候用到的脚本。后续会对这个文件做具体的分析

- **其他的文件基本都没什么用处可以不关注**

#### 官方方案分析

##### 官方执行步骤：

1. 在```Podfile```中加入如下代码：

   ```shell
    flutter_application_path = 'path/to/my_flutter/'
    load File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')
   ```

2. 对每个Xcode target添加方法```install_all_flutter_pods(flutter_application_path)```

   ```shell
   target 'MyApp' do
       install_all_flutter_pods(flutter_application_path)
    end
    target 'MyAppTests' do
       install_all_flutter_pods(flutter_application_path)
     
   end
   ```

**分析：**

第一步作用是设置flutter工程路径并添加脚本podhelper.rb。

第二步作用就是执行podhelper.rb中的方法install_all_flutter_pods，并将flutter工程路径变量传入。

那么下面我们来分析一下这个podhelper.rb中到底做了什么。

##### 分析podhelper.rb

脚本代码如下：

```ruby
# Install pods needed to embed Flutter application, Flutter engine, and plugins
# from the host application Podfile.
#
# @example
#   target 'MyApp' do
#     install_all_flutter_pods 'my_flutter'
#   end
# @param [String] flutter_application_path Path of the root directory of the Flutter module.
#                                          Optional, defaults to two levels up from the directory of this script.
#                                          MyApp/my_flutter/.ios/Flutter/../..
def install_all_flutter_pods(flutter_application_path = nil)
  flutter_application_path ||= File.join('..', '..')
  install_flutter_engine_pod
#  install_flutter_plugin_pods(flutter_application_path)
  install_flutter_application_pod(flutter_application_path)
end

# Install Flutter engine pod.
#
# @example
#   target 'MyApp' do
#     install_flutter_engine_pod
#   end
def install_flutter_engine_pod
  engine_dir = File.join(__dir__, 'engine')
  if !File.exist?(engine_dir)
    # Copy the debug engine to have something to link against if the xcode backend script has not run yet.
    # CocoaPods will not embed the framework on pod install (before any build phases can generate) if the dylib does not exist.
    debug_framework_dir = File.join(flutter_root, 'bin', 'cache', 'artifacts', 'engine', 'ios')
    FileUtils.mkdir_p(engine_dir)
    FileUtils.cp_r(File.join(debug_framework_dir, 'Flutter.framework'), engine_dir)
    FileUtils.cp(File.join(debug_framework_dir, 'Flutter.podspec'), engine_dir)
  end

  # Keep pod path relative so it can be checked into Podfile.lock.
  # Process will be run from project directory.
  engine_pathname = Pathname.new engine_dir
  project_directory_pathname = Pathname.new Dir.pwd
  relative = engine_pathname.relative_path_from project_directory_pathname

  pod 'Flutter', :path => relative.to_s, :inhibit_warnings => true
end

# Install Flutter plugin pods.
#
# @example
#   target 'MyApp' do
#     install_flutter_plugin_pods 'my_flutter'
#   end
# @param [String] flutter_application_path Path of the root directory of the Flutter module.
#                                          Optional, defaults to two levels up from the directory of this script.
#                                          MyApp/my_flutter/.ios/Flutter/../..
#def install_flutter_plugin_pods(flutter_application_path)
#  flutter_application_path ||= File.join('..', '..')
#
#  # Keep pod path relative so it can be checked into Podfile.lock.
#  # Process will be run from project directory.
#  current_directory_pathname = Pathname.new __dir__
#  project_directory_pathname = Pathname.new Dir.pwd
#  relative = current_directory_pathname.relative_path_from project_directory_pathname
#  pod 'FlutterPluginRegistrant', :path => File.join(relative, 'FlutterPluginRegistrant'), :inhibit_warnings => true
#
#  symlinks_dir = File.join(relative, '.symlinks')
#  FileUtils.mkdir_p(symlinks_dir)
#  plugin_pods = parse_KV_file(File.join(flutter_application_path, '.flutter-plugins'))
#  plugin_pods.map do |r|
#    symlink = File.join(symlinks_dir, r[:name])
#    FileUtils.rm_f(symlink)
#    File.symlink(r[:path], symlink)
#    pod r[:name], :path => File.join(symlink, 'ios'), :inhibit_warnings => true
#  end
#end

# Install Flutter application pod.
#
# @example
#   target 'MyApp' do
#     install_flutter_application_pod '../flutter_settings_repository'
#   end
# @param [String] flutter_application_path Path of the root directory of the Flutter module.
#                                          Optional, defaults to two levels up from the directory of this script.
#                                          MyApp/my_flutter/.ios/Flutter/../..
def install_flutter_application_pod(flutter_application_path)
  app_framework_dir = File.join(__dir__, 'App.framework')
  app_framework_dylib = File.join(app_framework_dir, 'App')
  if !File.exist?(app_framework_dylib)
    # Fake an App.framework to have something to link against if the xcode backend script has not run yet.
    # CocoaPods will not embed the framework on pod install (before any build phases can run) if the dylib does not exist.
    # Create a dummy dylib.
    FileUtils.mkdir_p(app_framework_dir)
    `echo "static const int Moo = 88;" | xcrun clang -x c -dynamiclib -o "#{app_framework_dylib}" -`
  end

  # Keep pod and script phase paths relative so they can be checked into source control.
  # Process will be run from project directory.
  current_directory_pathname = Pathname.new __dir__
  project_directory_pathname = Pathname.new Dir.pwd
  relative = current_directory_pathname.relative_path_from project_directory_pathname
  pod 'flutter_module', :path => relative.to_s, :inhibit_warnings => true

#  flutter_export_environment_path = File.join('${SRCROOT}', relative, 'flutter_export_environment.sh');
#  script_phase :name => 'Run Flutter Build Script',
#  :script => "set -e\nset -u\nsource \"#{flutter_export_environment_path}\"\n../xcode_backend.sh build",
##  :script => "set -e\nset -u\nsource \"#{flutter_export_environment_path}\"\n\"$FLUTTER_ROOT\"/packages/flutter_tools/bin/xcode_backend.sh build",
#
#  :input_files => [
#      File.join('${SRCROOT}', flutter_application_path, '.metadata'),
#      File.join('${SRCROOT}', relative, 'App.framework', 'App'),
#      File.join('${SRCROOT}', relative, 'engine', 'Flutter.framework', 'Flutter'),
#      flutter_export_environment_path
#    ],
#    :execution_position => :before_compile
end

#def parse_KV_file(file, separator='=')
#  file_abs_path = File.expand_path(file)
#  if !File.exists? file_abs_path
#    return [];
#  end
#  pods_array = []
#  skip_line_start_symbols = ["#", "/"]
#  File.foreach(file_abs_path) { |line|
#    next if skip_line_start_symbols.any? { |symbol| line =~ /^\s*#{symbol}/ }
#    plugin = line.split(pattern=separator)
#    if plugin.length == 2
#      podname = plugin[0].strip()
#      path = plugin[1].strip()
#      podpath = File.expand_path("#{path}", file_abs_path)
#      pods_array.push({:name => podname, :path => podpath});
#     else
#      puts "Invalid plugin specification: #{line}"
#    end
#  }
#  return pods_array
#end

#def flutter_root
#  generated_xcode_build_settings = parse_KV_file(File.join(__dir__, 'Generated.xcconfig'))
#  if generated_xcode_build_settings.empty?
#    puts "Generated.xcconfig must exist. Make sure `flutter pub get` is executed in the Flutter module."
#    exit
#  end
#  generated_xcode_build_settings.map { |p|
#    if p[:name] == 'FLUTTER_ROOT'
#      return p[:path]
#    end
#  }
#end

```

解释一下，这个脚本未注释的部分做了两件事：

- 引入Flutter引擎```install_flutter_engine_pod```
- 引入Flutter工程编译后的Framework，```install_flutter_application_pod```

> 如果是执行```flutter build ios --debug```会把这个脚本里的已注释代码部分取消注释。原因是release模式会把第三方库的东西都统一编译到App.framework内，debug不会，所以在debug时会把第三方库分别引入。

### 方案

综上，我们的方案步骤如下：

1. 在flutter_module工程里执行```flutter build ios --release```方法，在工程目录里找到```.ios```文件夹

2. 复制出来Flutter文件夹下的engine目录，App.framework及fluttermodule的podspec文件。

3. 将app.framework及podspec放入一个文件夹，engine放入另一个文件夹。

4. podfile里面添加

   ```ruby
   pod 'Flutter', :path => 'path to engine'
   pod 'flutter module 的name', :path => 'path to app.framework'
   ```

执行pod install就好了

此外，也可以把这两个文件放入git仓库进行管理。