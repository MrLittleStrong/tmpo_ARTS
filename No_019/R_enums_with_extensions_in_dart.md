# Enums with Extensions in Dart

### Links

[Enums with Extensions in Dart](https://medium.com/flutter/enums-with-extensions-dart-460c42ea51f7)

### Notes

1. Dart has recently released support for extension methods. When we make an enumeration and our text is dependent on this enum. Previously we need to use a switch statement. And we may need to copy the entire code snippet if we need to use the same text.

Basic Usage is below：

 ```dart
enum SelectedColor {
  primaryColor,
  secondaryColor,
}
extension SelectedColorExtension on SelectedColor {
  String get name => describeEnum(this);
  String get displayTitle {
    switch (this) {
      case SelectedColor.PrimaryColor:
        return 'This is the Primary Color';
      case SelectedColor.SecondaryColor:
        return 'This is the Secondary Color';
      default:
        return 'SelectedScheme Title is null';
    }
  }
}
 ```

`describeEnum()` function is defined in flutter foundation library, strips off the enum class name from `enumEntry.toString()`. Code as below:

```dart
String describeEnum(Object enumEntry) {
  final String description = enumEntry.toString();
  final int indexOfDot = description.indexOf('.');
  assert(indexOfDot != -1 && indexOfDot < description.length - 1);
  return description.substring(indexOfDot + 1);
}
```

Here provieds a full example.

Full Example Code as below:

```dart
import 'package:flutter/material.dart';
import 'package:flutter/foundation.dart' show describeEnum;

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> {
  bool isPrimaryColor = true;
  @override
  Widget build(BuildContext context) {
    SelectedColor selectedColor = getRandomSelectedColor(isPrimaryColor);
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(
          actions: [
            FlatButton(
              onPressed: () {
                setState(() {
                  isPrimaryColor = !isPrimaryColor;
                });
              },
              child: Text(selectedColor.displayColorChangeText),
            )
          ],
        ),
        body: Column(
          children: [
            Text(() {
              switch (selectedColor) {
                case SelectedColor.PrimaryColor:
                  return 'This is the Primary Color';
                case SelectedColor.SecondaryColor:
                  return 'This is the Secondary Color';
                default:
                  return 'SelectedScheme Title is null';
              }
            }()),
            Text(() {
              switch (selectedColor) {
                case SelectedColor.PrimaryColor:
                  return 'This is the Primary Color';
                case SelectedColor.SecondaryColor:
                  return 'This is the Secondary Color';
                default:
                  return 'SelectedScheme Title is null';
              }
            }()),
            Text(selectedColor.displayTitle),
            Text(selectedColor.name),
            Text(selectedColor.displayTitle),
          ],
        ),
      ),
    );
  }
}

enum SelectedColor {
  PrimaryColor,
  SecondaryColor,
}

extension SelectedColorExtension on SelectedColor {
  String get name => describeEnum(this);

  String get displayTitle {
    switch (this) {
      case SelectedColor.PrimaryColor:
        return 'This is the Primary Color';
      case SelectedColor.SecondaryColor:
        return 'This is the Secondary Color';
      default:
        return 'SelectedScheme Title is null';
    }
  }

  String get displayColorChangeText {
    switch (this) {
      case SelectedColor.PrimaryColor:
        return 'Change to Secondary Color';
      case SelectedColor.SecondaryColor:
        return 'Change to Primary Color';
      default:
        return 'SelectedScheme is null';
    }
  }

  Color color() {
    switch (this) {
      case SelectedColor.PrimaryColor:
        return Colors.red;
      case SelectedColor.SecondaryColor:
        return Colors.blue;
      default:
        return Colors.transparent;
    }
  }
}

SelectedColor getRandomSelectedColor(bool isPrimary) {
  switch (isPrimary) {
    case true:
      return SelectedColor.PrimaryColor;
    case false:
      return SelectedColor.SecondaryColor;
    default:
      return null;
  }
}
```

