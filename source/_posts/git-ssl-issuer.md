title: 'git服务器使用自签证书出现SSL certificate problem: Unable to get local issuer certificate错误'
date: 2017-02-23 11:00:00
tags:
---
搭建了本地[gogs](https://gogs.io)，看看使用情况，gogs自签证书只需要一个指令，就顺便加上了https，结果
```
git config --global http.sslVerify false
```
