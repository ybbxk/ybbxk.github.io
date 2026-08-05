title: gitlab升级后http方式502错误
date: 2016-05-13 14:11:32
tags: gitlab
---
gitlab从8.1升级到8.7.4，ssh方式已经可以提交获取代码了，但是通过http方式还是502。

查询nginx后台日志显示 ` connect() to unix://var/opt/gitlab/gitlab-git-http-server/socket failed (13: Permission denied) ` 把nginx用户加到文件所在的组后，再次尝试依然是502，但是错误日志内容有了变化 ` connect() to unix://var/opt/gitlab/gitlab-git-http-server/socket failed (111: Connection refused) ` 连接拒绝了。google搜结果，在[这里](https://gitlab.com/gitlab-org/omnibus-gitlab/issues/982)找到了答案。修改nginx配置，把 `/var/opt/gitlab/gitlab-git-http-server/socket` 替换为 `/var/opt/gitlab/gitlab-workhorse/socket` 就可以了。
