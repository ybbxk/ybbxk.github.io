title: linux脚本
date: 2016-04-07 17:04:20
tags: linux
---
1.使用cp时强制覆盖，两种方案:([来源](http://stackoverflow.com/a/8488293))
a) ` /bin/cp -rf /zzz/zzz/* /xxx/xxx `
b) ` /bin/cp -rf /zzz/zzz/* /xxx/xxx `  
原来当使用root登陆时，环境变量中会有一条 ` alias cp = cp -i ` ，用于防止管理员误操作。

2.crontab环境变量
  crontab执行命令时，并不适用当前用户的环境变量，编写任务时最好使用全路径。确实需要环境变量时，可以在crontab文件的指令中直接执行 `source ~/.bash_profile` 或 `/etc/profile`等加载用户环境变量。
3.crontab中的run-parts
  运行指定目录中的所有脚本，注意脚本文件中不能含有'.'，因为不含有任何参数的run-parts将会忽略他们。例如backup.sh这个脚本是不会被执行的。
