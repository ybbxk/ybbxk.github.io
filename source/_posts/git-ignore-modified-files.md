title: git_ignore_modified_files
date: 2015-12-14 23:15:29
tags: [git]
---
当文件已经更新，想要忽略本地的改动时，就不能使用.gitignore了，可以使用  
```git update-index --assume-unchanged /path/to/file```  
想要继续跟踪这些文件时，执行  
```git update-index --no-assume-unchanged /path/to/file```  
