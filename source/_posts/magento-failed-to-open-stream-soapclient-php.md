title: magento-failed-to-open-stream-soapclient.php
date: 2016-01-25 18:29:18
categories: [magento,php]
tags: [magento,soapclient]
---
```
Warning: include(SoapClient.php): failed to open stream: No such file or directory
```  
新部署magento时，出现上面一条错误，原因是未安装php-soap，安装php-soap就没有错误了，要根据已安装的php版本来选择。
