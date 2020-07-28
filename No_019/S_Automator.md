# 利用Automator自动化你的工作

### Notes

1、有些常用的比较长的命令，可以存在**Dash**里。Dash提供了文档查找工具，同时还有代码片段功能，方便保存和查询。

2、然而使用还是不方便，需要切到命令行复制并输入这些命令，所以考虑用Automator。

3、可以在Application里找到Automator。

- 新建一个Quick Action。

- workflow中选择只在音频文件的右键菜单里出现，也只在Finder中生效。

- 拖一个`Run Shell Script`模块

- 将`Pass input`设置成`as arguments`

- 在shell里写入你的脚本，例如作者写的

  ```shell
  # 输入文件
  fileName=$1
  # 输出文件名
  targetName=${fileName:0:-4}"-compressed.mp3"
  
  /usr/local/bin/ffmpeg -i $fileName -map 0:a:0 -b:a 96k $targetName
  ```

- 编辑完成后保存。Finder的右键Services里面就有这个菜单了。

