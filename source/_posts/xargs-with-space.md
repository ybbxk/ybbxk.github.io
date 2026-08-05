title: 带空格文件名使用xargs问题
date: 2017-07-21 11:51:35
tags: [xargs,shell,script]
---
多文件使用xargs操作时，经常会出现带空格的文件名，而有文件找不到的错误提示。在 [stackoverflow](https://stackoverflow.com/questions/16758525/how-can-i-make-xargs-handle-filenames-that-contain-spaces)上找到两种解决方法。在此贴上。

1.使用换行符(\n)作为分隔符
```
ls *mp3 | xargs -d '\n' mplayer
```
2.跟find连用时
```
 -0      Change xargs to expect NUL (``\0'') characters as separators,
         instead of spaces and newlines.  This is expected to be used in
         concert with the -print0 function in find(1).
```
```
find . -name "*.mp3" -print0 | xargs -0 mplayer
```