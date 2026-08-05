title: grep正则表达式
date: 2015-10-29 16:45:36
tags: [grep,linux,regular]
---
写个脚本，需要用到grep的零宽断言。拿`grep -E`尝试了半天都没效果。换用`grep -P`后，问题解决。以后还是统一用`grep -P`进行正则匹配吧。
