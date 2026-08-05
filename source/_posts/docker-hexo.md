title: docker部署hexo
date: 2017-07-21 10:26:56
tags: [docker,hexo]
---
很早想在docker上部署hexo，但是一直没(tai)时(lan)间(le)。终于抽空部署上来，问题多多，记录一下。

1.手动拉取 nodejs 镜像 `docker pull node:latest`，个人喜好，后面创建镜像的时候可以节省时间。顺便提一下，使用[daocloud](https://www.daocloud.io/)，拉取速度提升了1到2个数量级。

2.安装hexo，`npm install -g hexo-cli`，这一句就直接给我搞了个没有权限的报错。貌似是npm的一个bug，google到了解决方法如下。
```
npm config set user 0
npm config set unsafe-perm true
npm install hexo-cli -g
```
3.因为已经有现成的hexo工程，并没有把工程安装写在Dockerfile中，而是进入container完成工程的重新部署。Dockerfile只是安装环境和部分软件。就不传了。  

4.全自动部署，当然是需要docker-compose了。但是使用compose运行时，提示`nodejs exited with code 0`，而直接用Dockerfile却可以正常运行，参考这个[stackoverflow](https://stackoverflow.com/questions/37100358/docker-composer-exited-with-code-0)得到了解决方法。在docker-compose.yml文件增加一行`tty: true`，可以实现后台运行。  

5.由于桌面端没装nodejs(本来就是不想装，才用的docker)，无法运行hexo命令。在虚拟机里运行的话，每次要敲一大堆的命令。但是桌面上装上的话，又背离了使用docker的初衷。所以，还是用脚本来解决吧。在/usr/bin 下面增加一个文件，内容如下。
```
#!/bin/bash
all=$@
docker exec -i hexo bash -c "cd /path/to/hexo;hexo $all"
```
hexo使用的container 名字是 `hexo`, 通过一个`all`来传递所有参数，是因为直接在 -c后面用 `$@`时，多参数只能收到第一个参数，采用了个变通的方法。使用时，直接在桌面运行 `hexo param`就可以了。  
大功告成，用此docker完成本文。