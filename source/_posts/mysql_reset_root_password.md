title: 重置mysql root密码
date: 2015-10-26 17:04:19
tags: [mysql]
---
新的项目要用mysql5.6.x，centos默认的yum库只有mariadb，兼容mysql5.5。
看msyql文档装了社区版的5.7，却无法进入mysql，看到网上介绍的mysqld_safe指令重置密码，却发现5.7没有mysqld_safe指令。郁闷ing，再没找到其它方法，只能卸载5.7，安装5.6。  

Mysql[官网](https://dev.mysql.com/doc/refman/5.0/en/resetting-permissions.html)有详细的介绍：
1.使用运行mysql服务的账号登陆系统。
2.停止mysql服务，我使用命令：
```sh
systemctl stop mysql
```

3.新建一个文件，写入如下内容：
```sh
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('MyNewPass');
```
`MyNewPass` 字段使用新的密码
4.妥善保管上一步得到的文件，确保mysql命令有权限读取该文件。
5.执行命令
```sh
mysqld_safe --init-file=/home/me/mysql-init &
```
这里`/home/me/mysql-init`就是第三步得到的文件
6.mysql服务启动后，删除`/home/me/mysql-init`文件。  

之后就可以用新密码登陆mysql了，记录一下。
