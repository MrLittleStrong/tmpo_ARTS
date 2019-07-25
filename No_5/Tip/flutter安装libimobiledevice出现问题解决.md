###起因

由于公司人手短缺，在新的业务页面优化迭代中，我们需要使用flutter来拆分并重新构建旧有的业务。

时间紧任务重，赶紧就把flutter装起来了。

前面基本按照[入门: 在macOS上搭建Flutter开发环境](https://flutterchina.club/setup-macos/)步骤进行。

### 问题一

然而在安装```libimobiledevice```过程中报了以下错误：

```shell
configure: error: Package requirements (libusbmuxd >= 1.1.0) were not met:

Requested 'libusbmuxd >= 1.1.0' but version of libusbmuxd is 1.0.10

Consider adjusting the PKG_CONFIG_PATH environment variable if you
installed software in a non-standard prefix.

Alternatively, you may set the environment variables libusbmuxd_CFLAGS
and libusbmuxd_LIBS to avoid the need to call pkg-config.
See the pkg-config man page for more details.

READ THIS: https://docs.brew.sh/Troubleshooting
```

#### 解决

查阅网站，依次执行以下指令解决。

```shell
brew update
brew uninstall --ignore-dependencies libimobiledevice
brew uninstall --ignore-dependencies usbmuxd
brew install --HEAD usbmuxd
brew unlink usbmuxd
brew link usbmuxd
brew install --HEAD libimobiledevice
```

### 问题二

安装完所有依赖执行`flutter doctor`检查的时候可能报以下错误：

```shell
Unhandled exception:
Exception: idevice_id returned an error:

#0      IMobileDevice.getInfoForDevice (package:flutter_tools/src/ios/mac.dart:122:9)
<asynchronous suspension>
#1      IOSDevice.getAttachedDevices (package:flutter_tools/src/ios/devices.dart:152:53)
<asynchronous suspension>
#2      IOSDevices.pollingGetDevices (package:flutter_tools/src/ios/devices.dart:112:57)
#3      PollingDeviceDiscovery.devices (package:flutter_tools/src/device.dart:163:52)
<asynchronous suspension>
#4      DeviceManager.getAllConnectedDevices (package:flutter_tools/src/device.dart:91:46)
<asynchronous suspension>
#5      DeviceValidator.validate (package:flutter_tools/src/doctor.dart:677:54)
<asynchronous suspension>
#6      Doctor.startValidatorTasks (package:flutter_tools/src/doctor.dart:107:52)
#7      Doctor.diagnose (package:flutter_tools/src/doctor.dart:174:41)
#8      _AsyncAwaitCompleter.start (dart:async/runtime/libasync_patch.dart:49:6)
#9      Doctor.diagnose (package:flutter_tools/src/doctor.dart:164:24)
#10     DoctorCommand.runCommand (package:flutter_tools/src/commands/doctor.dart:29:39)
#11     _AsyncAwaitCompleter.start (dart:async/runtime/libasync_patch.dart:49:6)
#12     DoctorCommand.runCommand (package:flutter_tools/src/commands/doctor.dart:28:42)
#13     FlutterCommand.verifyThenRunCommand (package:flutter_tools/src/runner/flutter_command.dart:383:18)
#14     _asyncThenWrapperHelper.<anonymous closure> (dart:async/runtime/libasync_patch.dart:77:64)
#15     _rootRunUnary (dart:async/zone.dart:1132:38)
#16     _CustomZone.runUnary (dart:async/zone.dart:1029:19)
#17     _FutureListener.handleValue (dart:async/future_impl.dart:129:18)
#18     Future._propagateToListeners.handleValueCallback (dart:async/future_impl.dart:642:45)
#19     Future._propagateToListeners (dart:async/future_impl.dart:671:32)
#20     Future._complete (dart:async/future_impl.dart:476:7)
#21     _SyncCompleter.complete (dart:async/future_impl.dart:51:12)
#22     _AsyncAwaitCompleter.complete.<anonymous closure> (dart:async/runtime/libasync_patch.dart:33:20)
#23     _rootRun (dart:async/zone.dart:1124:13)
#24     _CustomZone.run (dart:async/zone.dart:1021:19)
#25     _CustomZone.bindCallback.<anonymous closure> (dart:async/zone.dart:947:23)
#26     _microtaskLoop (dart:async/schedule_microtask.dart:41:21)
#27     _startMicrotaskLoop (dart:async/schedule_microtask.dart:50:5)
#28     _runPendingImmediateCallback (dart:isolate/runtime/libisolate_patch.dart:115:13)
#29     _RawReceivePortImpl._handleMessage (dart:isolate/runtime/libisolate_patch.dart:172:5)
```

#### 解决

执行以下命令解决：

```shell
brew uninstall ideviceinstaller
brew uninstall libimobiledevice
brew install --HEAD libimobiledevice
brew link --overwrite libimobiledevice
brew install --HEAD ideviceinstaller
brew link --overwrite ideviceinstaller
sudo rm -rf /var/db/lockdown/*
```

之后连接iOS设备选择“信任”之后，执行以下命令：

```shell
sudo chmod -R 777 /var/db/lockdown/
```

