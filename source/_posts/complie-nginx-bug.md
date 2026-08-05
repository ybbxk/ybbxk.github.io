title: nginx编译bug
date: 2017-08-04 13:48:57
tags: [nginx,openssl]
---
自定义编译nginx时，出现了如下报错
```
relocation R_X86_64_32 against `.rodata' can not be used when making a shared object; recompile with -fPIC
/path/to/openssl/.openssl/lib/libssl.a: error adding symbols: Bad value
collect2: error: ld returned 1 exit status
```
打算直接修改Makefile文件时，找到了[askubuntu](https://askubuntu.com/questions/804416/building-nginx-deb-package-with-custom-openssl/804417#804417)，新的解决方法，尝试之后，编译通过，在这里记录解决方法。
在nginx目录下的 auto/options 文件中找到这么一行
```
        --with-openssl-opt=*)            OPENSSL_OPT="$value"       ;; 
```
修改为如下样式
```
        --with-openssl-opt=*)            OPENSSL_OPT="$value -fPIC"       ;; 
```
可能因为版本不同，我找到的跟链接例子中的不大一样，但是可以解决编译错误。