title: redis-incr
date: 2017-10-19 15:29:39
tags: [redis]
---
redis-incr在非事务情况下会返回修改后的新值，在事务中返回空数组。