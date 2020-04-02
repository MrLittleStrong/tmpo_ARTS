Xcode build configuration files

### Links

[Xcode Build Configuration Files](https://nshipster.com/xcconfig/)

### Notes

This article introduced How to use xcconfig file to seperate configurations in easy ways compared to use Configurations directly in project.

Usually, we use xcconfig file to manage frameworks, files,  as to seperate some framework import and some not in specific environment. This article introduced how to use it to manage code like api_base_url, which is to add to info.plist and read it from bundle.

However, xcconfig treat // as a comment delimiter, so we cant add ```http://``` in this file. 

And don't use xcconfig to store secrets