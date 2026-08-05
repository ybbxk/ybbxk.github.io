title: hexo使用注意事项
date: 2015-10-10 15:30:37
paragraph: none
tags: [hexo,markdown]
---
1.新的段落，换行之后加一个回车。
2.数学公式中有\_的，都要写成`\_`，以免自动转换成斜体。
3.要写hexo语法词时使用 
<% raw %>
code 
<% endraw %>
不转义code内容
4.要手写单页css style时，要把css标签写在后面，不能写在开头。
5.文章结尾不能出现\`\`\`，几率导致生成index页时，该文章之后的文章无法加载到index。文章结尾还是加一个空行比较保险。-_-|||

