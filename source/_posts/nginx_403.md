title: nginx 403难题
date: 2015-09-29 23:46:07 +08:00
paragraph: fd
tags: [nginx,403,SELinux]
---
今天遇到个怪事，nginx server_root目录下的文件有读权限，层层目录也都有执行权限，但就是无法访问。

各种google之后终于找到了[原因](http://stackoverflow.com/a/26228135)。关闭`SELinux`之后访问正常，暂时不清楚具体原因，做个记录先。
