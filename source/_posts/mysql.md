title: mysql问题记录
date: 2017-09-05 17:36:13
tags: [mysql]
---
1.sql语句`select * from db_table where field != 12;`，当`field`值为`null`时，查不到该结果，sql语句需要修改为`select * from db_table where field != 12 or field is null;`