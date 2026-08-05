---
title: mysql the user specified as a definer issue
date: 2019-08-31 09:01:04
tags: mysql
---
好久没用家里电脑的环境了，今天通过php写入数据库时出现了这个错误`SQLSTATE[HY000]: General error: 1449 The user specified as a definer ('mysql.infoschema'@'localhost') does not exist`，一开始还以为是代码或者配置的问题，网上搜索后才知道是mysql版本升级或者降级后数据库表权限需要更新，执行下面的指令就可以恢复:
```bash
mysql -u root -p
mysql> SET GLOBAL innodb_fast_shutdown = 1;
mysql_upgrade -u root -p
```

