---
title: Shell脚本基础
date: 2018-04-25 14:40:57
categories: Linux
tags: Linux
---

# Shell脚本的用处

经常会在网上看到一些一键XXX脚本。例如一键LAMP,LNMP脚本。只需要执行简单的命令，然后跟着脚本提示操作就能够完成搭建一个简单的网站，带数据库后台和各种环境。Shell脚本可以帮助我们完成很多冗杂的工作，也可以通过编写shell脚本达到快速部署环境的目的。能够大大提高工作效率。类似windows里的.BAT文件。



# 编写简单的Shell脚本

编写Shell脚本的方式非常简单。只需要使用常用的文本编辑器如VIM创建一个文件，然后在文件中输入命令即可。例如，如果想查看当前所在的工作路径并列出当前目录下所有文件及属性信息，实现这个功能的脚本就可以这么编写

```shell
[root@remilia ~]# vi example.sh
#!/bin/bash
#this is a example for shell script 
pwd
ls -al
```



那么一个个来解释这其中的含义吧

-   第一行#!/bin/bash 代表脚本声明，即告诉系统使用哪种命令解释器执行这个脚本。
-   第二行#为注释信息。如果有学过C语言或者其他编程语言应该能很快了解。注释信息**不会被执行**。是对这个脚本功能的解释。方便他人查看脚本的时候知道这个脚本的功能。
-   后面的就是Linux的命令，就不再赘述了。另外。Linux**不以后缀名分辨文件类型**。也可以说Linux中没有后缀名的概念。所以脚本名字的.sh是一个约定俗成的规矩 表示这是一个可执行的脚本。



一个写好的脚本文件可以通过bash去执行。结果如下

```shell
[root@remilia ~]# bash example.sh 
/root
total 556
dr-xr-x---.  3 root root   4096 May  2 03:09 .
drwxr-xr-x. 19 root root   4096 May  2 03:05 ..
-rw-------.  1 root root    871 Mar  1 19:49 anaconda-ks.cfg
-rw-------.  1 root root   7075 Apr 18 04:17 .bash_history
-rw-r--r--.  1 root root     18 Dec 28  2013 .bash_logout
-rw-r--r--.  1 root root    176 Dec 28  2013 .bash_profile
-rw-r--r--.  1 root root    176 Dec 28  2013 .bashrc
-rw-r--r--.  1 root root    100 Dec 28  2013 .cshrc
-rw-r--r--.  1 root root     60 May  2 03:09 example.sh
-rw-r--r--.  1 root root 513076 Feb 28 04:36 GitPython-1.0.1-5.el7.noarch.rpm
-rw-------.  1 root root     46 Mar  1 21:07 .lesshst
drwxr-xr-x.  3 root root     36 Apr 17 06:01 mirrors.163.com
-rw-r--r--.  1 root root    129 Dec 28  2013 .tcshrc
-rw-------.  1 root root    808 Mar  1 21:24 .viminfo
```



除了用bash这种方式执行之外，还可以使用**“./”**去执行。但是可能会报错提示权限不足。这边先使用chmod命令给脚本执行的权限。权限部分的知识在这暂且不提。在此只需要知道脚本的另一种执行方法就行。

```shell
[root@remilia ~]# ./example.sh
-bash: ./example.sh: Permission denied

[root@remilia ~]# chmod u+x example.sh 
[root@remilia ~]# ./example.sh 
/root
total 556
dr-xr-x---.  3 root root   4096 May  2 03:09 .
drwxr-xr-x. 19 root root   4096 May  2 03:05 ..
-rw-------.  1 root root    871 Mar  1 19:49 anaconda-ks.cfg
-rw-------.  1 root root   7075 Apr 18 04:17 .bash_history
-rw-r--r--.  1 root root     18 Dec 28  2013 .bash_logout
-rw-r--r--.  1 root root    176 Dec 28  2013 .bash_profile
-rw-r--r--.  1 root root    176 Dec 28  2013 .bashrc
-rw-r--r--.  1 root root    100 Dec 28  2013 .cshrc
-rwxr--r--.  1 root root     60 May  2 03:09 example.sh
-rw-r--r--.  1 root root 513076 Feb 28 04:36 GitPython-1.0.1-5.el7.noarch.rpm
-rw-------.  1 root root     46 Mar  1 21:07 .lesshst
drwxr-xr-x.  3 root root     36 Apr 17 06:01 mirrors.163.com
-rw-r--r--.  1 root root    129 Dec 28  2013 .tcshrc
-rw-------.  1 root root    808 Mar  1 21:24 .viminfo
```



## 接收用户的参数

上面这种简单的shell脚本只能执行一些预先定义好的功能。有时候需要接收用户的输入，才能更好的满足需求。



在Shell语言中，内设了用于接收参数的变量。变量之间可以用空格间隔。例如\$0代表当前shell脚本程序的名称，\$#对应的是总共有几个参数。、\$*对应的是所有位置的参数值,$?对应的是上一个命令执行的返回值。\$1 \$2 \$3 ... \$N代表第一个，第二个，第三个....第N个参数的值