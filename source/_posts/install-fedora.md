---
title: fedora安装、使用记录记录
date: 2018-09-05 09:11:54
tags: [centos,fedora]
categories: fedora
---
笔记本新装了个SSD，总共有两个SSD和一个机械硬盘，第一块SSD是自带的win10，于是在新SSD上装个Fedora，用于各种折腾，跟Win10组双系统。此处记录Fedora安装和使用过程中遇到的问题。   
<!-- more -->
## 一：系统安装
系统安装遇到的问题主要集中在`Fedora`和nvidia显卡上。  
主要说出现问题的地方。  
1.安装完成后，不能使用nvidia官方驱动，也不能使用系统软件管理中带的驱动，要根据[这篇文章][how-to-nvidia]介绍的方法来装截取关键代码如下：  
```bash
dnf install xorg-x11-drv-nvidia akmod-nvidia
dnf install xorg-x11-drv-nvidia-cuda #optional for cuda/nvdec/nvenc support
dnf update -y
```
-------
## 软件安装记录  
### 1.docker  
docker安装借助了[清华大学开源镜像站][mirrors-tsinghua]  
```bash
curl -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/fedora/docker-ce.repo
```
把软件仓库地址替换为 TUNA:
```bash
sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```
最后安装:
```bash
sudo yum makecache fast
sudo yum install docker-ce
```

<div style="display:none">
### 2.4had0w40ck4  
直接使用pip命令安装
```bash
pip3 install shadowsocks
```
</div>  

### 3.proxychains4  
在系统配置中开启全局代理，其它软件中竟然无法使用。使用`proxychains4`就可以了。  
安装方式：
```bash
sudo dnf install -y proxychains-ng
```
安装后，在`/etc/proxychains.conf`文件结尾加上要使用的代理配置 `代理方式 ip 端口`
例如：
```bash
socks5 127.0.0.1 1080
```
需要使用代理的工具，直接在前面加上`proxychains4`就可以了，例如： 
```bash
proxychains curl -O http://www.abc.com/
```
### 4.BaiduPCS-Go  
一个百度网盘的命令行工具，下载release解压后即可使用[BaiduPCS-Go][BaiduPCS-Go]。
### 5.ariac2(计划中)  


[how-to-nvidia]:https://rpmfusion.org/Howto/NVIDIA
[mirrors-tsinghua]:https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/
[BaiduPCS-Go]:https://github.com/ybbxk/BaiduPCS-Go