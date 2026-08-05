title: Magento的block type解释
date: 2016-02-23 22:46:38
categories: [magento,php]
tags: magento
---
Magento在xml文件中block的type解释。结果也是在网上搜到的，按照 `type="A/B"`的格式来解释。  
答案转自[链接](http://stackoverflow.com/questions/6633307/understanding-magento-block-and-block-type)。 
```html
<block type="page/html" name="root" output="toHtml" template="example/view.phtml"> 
```
A是一个模块的别名。在这个例子里，'page'是Mage_Page_block(在文件app/code/core/Mage/Page/etc/config.xml中定义)。
B是A模块中的一个class名，每个词的首字母大写。在这个例子中，html转换为Html，是Mage_Page_block_Html的别名。这个文件大概在app/code/core/Mage/Page/Block/Html.php ，因为在Magento中，类名可以直接转换为路径。  
如果用model别名替换block别名结果则是Mage_Page_model。换成resource models和helpers也是一样的。自己写的模块如果包含block、models、helpers也应该在配置文件里(config/)定义。
[链接](http://stackoverflow.com/questions/6633307/understanding-magento-block-and-block-type)

