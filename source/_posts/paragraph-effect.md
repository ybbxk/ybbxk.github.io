title: 段落效果
date: 2015-10-10 13:56:30
categories: 前端
paragraph: none
tags: [css,hexo,前端]
---
按照中文的习惯，每段都要空两个字符，结果用markdown时，发现手动输入的空格都被过滤了，无效。搜索一番后找到方法，两个`&emsp;`即可解决。  
<p class="ds">但是每次都要手动输入，比较麻烦，想到了用css解决，加入这么一段就OK
`p{text-indent: 2em;}`</p>
<p class="fd">脑洞大开，又想到了其它效果，**首字下沉**，也找到了相应的css  
`p:first-letter:first-child {font-size:2.5em; font-family:"楷体","楷体_GB2312"; font-weight:bold; line-height:1.2em; float:left; padding:5px 2px 0 0; color:#c00;}`</p>

这下两种方案都有了解决方法，然后又有了新的想法，要是两种方案能通过变量控制就更好了。

方法，在主题的`/layout/_partials/head.swig`文件中增加如下代码：
{% raw %}
{% if is_post() %}
{% endraw %}
<br>
{% raw %}
{% if page.paragraph === 'ds' or page.paragraph === "" %}
{% endraw %}
<br>
&lt;style type="text/css"&gt;
<br>
p{text-indent: 2em;}
<br>
&lt;/style&gt;
<br>
{% raw %}
{% elseif page.paragraph === 'fd' %}
{% endraw %}
<br>
&lt;style type="text/css"&gt;
<br>
p:first-letter {font-size:2.5em; font-family:"楷体","楷体_GB2312"; font-weight:bold; line-height:1.2em; float:left; padding:5px 2px 0 0; color:#c00;}
<br>
&lt;/style&gt;
<br>
{% raw %}
{% endif %}
{% endif %}
{% endraw %}

写文章时，在Front-matter中加上 `paragraph: ds/fd`字段，ds表示段落前增加两个空字符；`fd`表示首字下沉；不写时默认为`ds`；不想要段落格式转换时填上其它内容，我用了`none`。后期有其它想法时，可以再增加。
<style type="text/css">
p.ds{text-indent: 2em;}
p.fd:first-letter {font-size:2.5em; font-family:"楷体","楷体_GB2312"; font-weight:bold; line-height:1.2em; float:left; padding:5px 2px 0 0; color:#c00;}
</style>
