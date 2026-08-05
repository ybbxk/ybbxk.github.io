title: docke使用记录
date: 2017-11-01 10:50:29
tags: [docker]
categories: docker
---
1.使用root用户登陆container，解决方法来自[stackoverflow](https://stackoverflow.com/a/35485346)。`docker exec -u 0 -it mycontainer bash`