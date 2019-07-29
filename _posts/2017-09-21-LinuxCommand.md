---
layout:     post
title:      "Linux Command -- Part I"
subtitle:   "第一篇"
date:       2017-09-21 12:00:00
author:     "Dingding" header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - linux command
---

## uname
Print certain system information

## readlink
print resolved symbolic links or canonical file names
对于查找可执行文件的实际位置,一般可以先用whereis命令找到软连接,再用readlink找出实际位置

* -f  
   canonicalize by following every symlink in every component of the given name recursively; all but the last component must exist
   
## export
显示、设置、删除环境变量，其作用效力仅限于该次登录操作。

*  -n  删除指定变量  
实际并未删除，只是该变量不会在后续执行环境中生效。
*  -p  
列出所有的shell赋予程序的环境变量

## mkdir
* -p : 递归创建目录

## source
在当前bash环境下读取并执行FileName中的命令。该命令通常用命令“.”来替代。

* source 命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell,脚本里所有新建、改变变量的语句都会保存在当前shell里面。
* sh filename与./filename 命令重新建立一个子shell，在子shell中执行脚本里的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。


## date
* date :当前时间
* date +%s :当前时间戳
* date -d @1436781720:时间戳转字符串
* date -j -f "%Y-%m-%d %H:%M:%S" "2015-07-13 18:02:00" "+%s"
* date -d '2013-2-22 22:14' +%s

## 开启启动管理
* $ sudo update-rc.d nginx defaults      #增加服务
* $ sudo update-rc.d -f nginx remove    #移除服务


## 查看日志
* tail -f log | grep xxx

## 用户切换
* sudo su root

## scp
secure copy (remote file copy program)

*  scp  [parameters] [[user@]host1:]file1 ... [[user@]host2:]file2
*  scp [参数] [原路径] [目标路径]
*  -r  递归copy,用于目录

## uniq
report or omit repeated lines,由于只能检测出相邻的向同行，一般与sort命令结合使用。

* -c  prefix lines by the number of occurrences
* -d  only print duplicate lines, one for each group
* -i  ignore differences in case when comparing
* -u  only print unique lines

## cat
concatenate files and print on the standard output

* 显示文件 cat filename
* 创建文件 cat filename
* 合并文件 cat file1 file2 > file3
* 追加文件 cat file1 >> file2

## wc
* -c  print the byte counts  
      不可见字符也会算在内
* -l  print the newline counts  
      统计文件行数，文件结束符也算作了一个newline character
      
## !!  --  ~
* -- :无论如何，将其之后的 argument 视为一个文件名  
    rm --f -f :删除一个名为"-f"的文件
* !! :代表上一次输入shell的内容
* ~  :代表用户的主目录

## 重定向
```
* >  :如果文件不存在，就创建文件；如果文件存在，就将其清空
* >> :如果文件不存在，就创建文件；如果文件存在，则将新的内容追加到文件的末尾
```

## ln
* ln source dest  
创建硬链接
* ln -s source dest  
创建软连接,常用语将某个执行命令加入到PATH内的某个目录中


## xargs
build and execute command lines from standard input

* 通常用于一些不支持不支持管道操作符的命令


## grep 
* -E :指定正则表达式
* -v: 过滤匹配项


## which
* which  查看可执行文件的位置。
* whereis 查看文件的位置。 
* locate   配合数据库查看文件位置。
* find   实际搜寻硬盘查询文件名称。

whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。
和find相比，whereis查找的速度非常快，这是因为linux系统会将 系统内的所有文件都记录在一个数据库文件中，当使用whereis和下面即将介绍的locate时，会从数据库中查找数据，而不是像find命令那样，通 过遍历硬盘来查找，效率自然会很高。 
但是该数据库文件并不是实时更新，默认情况下时一星期更新一次，因此，我们在用whereis和locate 查找文件时，有时会找到已经被删除的数据，或者刚刚建立文件，却无法查找到，原因就是因为数据库文件没有被更新。 

which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。 



## traceroute
traceroute ip(域名): 路由跟踪命令 
* -n 使用ip，速度更快 
* -q 每次发送的数据包数量，默认是3 
* -m 设置跳数，默认是30

## ifdown ifup
* ifdown 网卡设备名 ： 关闭网卡
* ifup 网卡设备名 ： 启用网卡


## netstat
网络状态查询
-t 列出TCP协议端口
-u 列出UDP协议端口
-n 不适用域名与服务名，而是用ip地址和端口号
-l 仅列出在监听端口
-a 所有的连接
-r 路由表


## pidof fcitx | xargs kill
xargs:換行和空白被空格代替


## 环境变量 source
* source: 在当前bash环境下读取并执行FileName中的命令。只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell,脚本里所有新建、改变变量的语句都会保存在当前shell里面。
* sh filename与./filename :重新建立一个子shell，在子shell中执行脚本里的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。

## date
* date :当前时间
* date +%s :当前时间戳
* date -d @1436781720:时间戳转字符串
* date -d '2013-2-22 22:14' +%s
* date -j -f "%Y-%m-%d %H:%M:%S" "2015-07-13 18:02:00" "+%s"


### 远程安全拷贝 scp
*  scp  [parameters] [[user@]host1:]file1 ... [[user@]host2:]file2
*  scp [参数] [原路径] [目标路径]
*  -r  递归copy,用于目录



### 去除重复 uniq
* uinq 只能检测出相邻的向同行，一般与sort命令结合使用。
* -c  prefix lines by the number of occurrences
* -d  only print duplicate lines, one for each group
* -i  ignore differences in case when comparing
* -u  only print unique lines


### 显示、创建文件 cat
* 显示文件 cat filename
* 创建文件 cat filename
* 合并文件 cat file1 file2 > file3
* 追加文件 cat file1 >> file2


### 文件统计 wc
* -l  :统计文件行数，文件结束符也算作了一个newline character
      
### 特殊字符 !!  --  ~
* -- :无论如何，将其之后的 argument 视为一个文件名  
    rm --f -f :删除一个名为"-f"的文件
* !! :代表上一次输入shell的内容
* ~  :代表用户的主目录

### 创建链接 ln
* ln -s source dest :创建软连接,常用语将某个执行命令加入到PATH内的某个目录中
* ln source dest:创建硬链接


### grep 
* -E :指定正则表达式
* -v: 过滤匹配项
* -o: 仅仅输出匹配部分
* -P: 使用Perl风格的正则表达式


###  分割文件 split
* -l :按行数进行拆分
* -a :指定拆分文件的后缀长度
* -d :用数字作为拆分文件的后缀

### 查找文件 
* which  查看可执行文件的位置,可以用于确定所使用命令的位置。
* whereis 查看文件的位置。 
* locate  配合数据库查看文件位置。
* find   实际搜寻硬盘查询文件名称,应该优先考虑使用其他搜索命令。

### 获取文件绝对路径
* basepath=$(cd `dirname $0`;pwd);

### 查看系统限制
* ulimit -a
* ulimit -n 4096:修改可以打开的文件数上限

### sort
-u :仅输出不重复行
-d:仅输出重复行
-c:输出统计信息

### tail
* 获取文件最后五行:tail -5。
* 监控文件:tail -f。

### 查看端口占用
* lsof -i:端口号 用于查看某一端口的占用情况，比如查看8000端口使用情况，lsof -i:8000
* netstat -tlnp  

### 压缩解压
* gzip:解压 .gz 程序
  * -d  解压
  * -c  输出至标准输出
* bzip2 : 解压 .bz2 程序
  * -d  解压
  * -c  输出至标准输出
* tar 
  * -x  解压
  * -c  压缩
  * -f  指定文件名称

### xars与管道
* xargs :将前一项的输出当做下一项的命令行参数,通常用于不支持管道的命令
* 管道：将前一项的输出当做下一项的标准输入。管道符默认只能通过标准输出,错误输出会被过滤、丢弃。



## du命令用来查看目录或文件所占用磁盘空间的大小。常用选项组合为：du -sh
du常用的选项：
　　-h：以人类可读的方式显示
　　-a：显示目录占用的磁盘空间大小，还要显示其下目录和文件占用磁盘空间的大小
　　-s：显示目录占用的磁盘空间大小，不要显示其下子目录和文件占用的磁盘空间大小
　　-c：显示几个目录或文件占用的磁盘空间大小，还要统计它们的总和
　　--apparent-size：显示目录或文件自身的大小
　　-l ：统计硬链接占用磁盘空间的大小
　　-L：统计符号链接所指向的文件占用的磁盘空间大小　　

du -sh : 查看当前目录总共占的容量。而不单独列出各子项占用的容量 
du -lh --max-depth=1 : 查看当前目录下一级子文件和子目录占用的磁盘容量。
du -sh * | sort -n 统计当前文件夹(目录)大小，并按文件大小排序
du -sk filename 查看指定文件大小


## kill 
1. kill -9 id：一般不加参数kill是使用15来杀，这相当于正常停止进程，停止进程的时候会释放进程所占用的资源；他们的区别就好比电脑关机中的软关机（通过“开始”菜单选择“关机”）与硬关机（直接切断电源），虽然都能关机，但是程序所作的处理是不一样的。
2. kill - 9 表示强制杀死该进程；而 kill 则有局限性，例如后台进程，守护进程等；
3. 执行kill命令，系统会发送一个SIGTERM信号给对应的程序。SIGTERM多半是会被阻塞的。kill -9命令，系统给对应程序发送的信号是SIGKILL，即exit。exit信号不会被系统阻塞，所以kill -9能顺利杀掉进程。


