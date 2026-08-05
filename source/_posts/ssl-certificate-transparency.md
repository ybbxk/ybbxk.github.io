title: 为 SSL 站点启用 Certificate Transparency 功能
date: 2016-02-24 14:36:18
tags: ssl
---
转载自：[为 SSL 站点启用 Certificate Transparency 功能](http://blog.eqoe.cn/posts/enable-certificate-transparency-for-nginx.html)
下载 nginx 源代码和 nginx-ct 的源代码：
```sh
wget http://nginx.org/download/nginx-1.9.6.tar.gz
wget -O nginx-ct.zip https://github.com/grahamedgecombe/nginx-ct/archive/master.zip
tar zxf nginx-1.9.6.tar.gz
unzip nginx-ct.zip
```
编译 nginx ( 你也可以带上你自己的参数，加上 ssl 和 ct module 就可以）
```sh
cd nginx-1.9.6/
./configure --with-http_v2_module --with-http_ssl_module --add-module=../nginx-ct-master
```
创建 SCT 的文件夹
```sh
mkdir /etc/ssl/scts/
```
下载证书提交工具
```sh
wget -O ct-submit.zip https://github.com/grahamedgecombe/ct-submit/archive/master.zip
unzip ct-submit.zip
cd ct-submit-master/
# 请确保已经安装 go 语言
go build
```
将证书提交到 Certificate Transparency Log 服务器，假设你的 SSL 证书在 /etc/ssl/server.crt （带证书链的）
```sh
sudo sh -c "./ct-submit-master ct.googleapis.com/aviator \
  </etc/ssl/server.crt \
  >/etc/ssl/scts/aviator.sct"
sudo sh -c "./ct-submit-master ct.googleapis.com/pilot \
  </etc/ssl/server.crt \
  >/etc/ssl/scts/pilot.sct"
sudo sh -c "./ct-submit-master ct.googleapis.com/rocketeer \
  </etc/ssl/server.crt \
  >/etc/ssl/scts/rocketeer.sct"
```
然后在 nginx 配置中添加
```sh
ssl_ct on;
ssl_ct_static_scts /etc/ssl/scts;
```
以上内容转载自：[为 SSL 站点启用 Certificate Transparency 功能](http://blog.eqoe.cn/posts/enable-certificate-transparency-for-nginx.html)
可能因为使用的linux发行版不一样，我用的是centos，在执行nginx的configure时需要指定openssl的源码路径(openssl 1.0.2+，我用的是 openssl-1.0.2f，奇怪的版本号)，否则会有未定义函数的报错。
```sh
--with-openssl=/path/to/openssl
```  

