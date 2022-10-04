




---
## FAQ 问题
#### SSH 登陆服务器locale告警（-bash: warning:setlocale:）的处理方法
> 原因分析：
根据上面登录警告提示可知，系统已经设置了默认地区_语言字符集为en_US.UTF-8,但是在系统中没有定义对应的local文件，
所以只需要手动生成这个locale文件即可！

* 解决办法：
```shell {.line-numbers}
 ~# vim /etc/environment  #添加下面两行内容
 LANG="en_US.UTF-8"
 LC_ALL=
 ~# localedef -v -c -i en_US -f UTF-8 en_US.UTF-8
```
