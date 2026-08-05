title: Centos时区设置
date: 2015-10-26 16:47:11
tags: [centos,timezone]
---
新装了centos系统，发现时区设置不对，网上搜索后发现centos的时区设置稍微有点特殊，做个记录。  

在/etc目录下执行 `ll localtime` 结果发现 `localtime -> ../usr/share/zoneinfo/America/New_York` ，"本地时间"实际上是纽约时间。centos系统的时区文件在/usr/share/zoneinfo/目录下，很容已能找到 `Asia/Shanghai` 文件，删除原来的 `/etc/localtime` ，然后执行如下命令
```sh
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
现在的系统时间就是“北京时间”了。
