title: 虚拟机挂载主机共享文件夹
date: 2016-01-05 10:45:59
tags: [virtualbox,mount]
---
通过下面的指令可以挂载目录，并指定所属用户和用户组
```sh
sudo mount -t vboxsf -o uid=1000,gid=1000 host_shared_folder /path/to/local/
```  
  
