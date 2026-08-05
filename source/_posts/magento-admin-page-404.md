title: magento后台管理页面404错误
date: 2015-12-29 15:05:00
categories: magento
tags: [magento,404]
---
做magento开发，为了方便数据同步，直接把线上数据库导入本地，结果后台管理界面直接返回404。在[stackoverflow](http://stackoverflow.com/questions/5178066/error-404-not-found-in-magento-admin-login-page)找到了解决方法，执行如下sql语句，即可解决。  
```sql
SET FOREIGN_KEY_CHECKS=0;
UPDATE `core_store` SET store_id = 0 WHERE code='admin';
UPDATE `core_store_group` SET group_id = 0 WHERE name='Default';
UPDATE `core_website` SET website_id = 0 WHERE code='admin';
UPDATE `customer_group` SET customer_group_id = 0 WHERE customer_group_code='NOT LOGGED IN';
SET FOREIGN_KEY_CHECKS=1;
```  
按照作者的意思，store_id,group_id,website_id,customer_group_id这些id的值应该是0，但在数据导入新服务器时丢失了。从sql语句中推测大概是因为外键检查导致的数据丢失。所以重新设置数据库的这些字段可以解决问题。
