title: 便签
date: 2015-09-16 14:05:10
---
1.增加数据库用户权限  
`grant all privileges on dbname.* to username@'%' identified by 'password';`  

2.清除 DNS 缓存:  
chrome://net-internals/#dns  

3.[PHP程序员进阶学习书籍参考指南](http://blog.csdn.net/heiyeshuwu/article/details/50686878)  

4.git clone 后恢复文件修改时间:
```bash
git log --pretty=%at --name-status --reverse | perl -ane '($x,$f)=@F;next if !$x;$t=$x,next if !defined($f)||$s{$f};$s{$f}=utime($t,$t,$f),next if $x=~/[AM]/;'
```

5.php xdebug cli  
```bash
export PHP_IDE_CONFIG="serverName=serverName" && export XDEBUG_CONFIG="remote_enable=1 remote_mode=req remote_port=9000 remote_host=ip remote_connect_back=0"
```
`serverName`是IDE中配置的server name

6.获取docker容器ip
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id
```
7.xargs 处理filename带空格的文件
```bash
find . -name \*.mp3 -print0 | xargs -0 mplayer
```
8.xdebug 连不上时，可以试试把 remote_host 改为
```bash
xdebug.remote_host=host.docker.internal
```
9.Linux 查看上次开机时间
```
date -d "$(awk -F. '{print $1}' /proc/uptime) second ago" +"%Y-%m-%d %H:%M:%S"
```
