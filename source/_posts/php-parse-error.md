title: php parse error
date: 2016-02-29 09:36:21
categories: php
tags: php
---
PHP模版文件打开后出现"Parse error: syntax error, unexpected end of file in 'file path'",在[stackoverflow](http://stackoverflow.com/questions/11482527/parse-error-syntax-error-unexpected-end-of-file-in-my-php-code)找到了解决方法。  
>在代码的行尾要尽量避免这种
```php
{?>
```
>和这种代码
```php
<?php}
```
>要使用空格分开，像这样
```php
{ ?>
<?php {
```
>还要避免使用 ` <? ` 而应该使用 ` <?php ` 。  
