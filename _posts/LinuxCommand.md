---
layout:     post
title:      "Linux Command随记"
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
* -p  
递归创建目录

## source
在当前bash环境下读取并执行FileName中的命令。该命令通常用命令“.”来替代。

* source 命令其实只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell,脚本里所有新建、改变变量的语句都会保存在当前shell里面。
* sh filename与./filename 命令重新建立一个子shell，在子shell中执行脚本里的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。


## date
* date :当前时间
* date +%s :当前时间戳
* date -d @1436781720:时间戳转字符串
* date -d '2013-2-22 22:14' +%s
* date -j -f "%Y-%m-%d %H:%M:%S" "2015-07-13 18:02:00" "+%s"



# scp
secure copy (remote file copy program)

*  scp  [parameters] [[user@]host1:]file1 ... [[user@]host2:]file2
*  scp [参数] [原路径] [目标路径]
*  -r  递归copy,用于目录



# uniq
report or omit repeated lines,由于只能检测出相邻的向同行，一般与sort命令结合使用。

* -c  prefix lines by the number of occurrences
* -d  only print duplicate lines, one for each group
* -i  ignore differences in case when comparing
* -u  only print unique lines


# cat
concatenate files and print on the standard output

* 显示文件 cat filename
* 创建文件 cat filename
* 合并文件 cat file1 file2 > file3
* 追加文件 cat file1 >> file2


# wc
* -c  print the byte counts  
      不可见字符也会算在内
* -l  print the newline counts  
      统计文件行数，文件结束符也算作了一个newline character
      
# !!  --  ~
* -- :无论如何，将其之后的 argument 视为一个文件名  
    rm --f -f :删除一个名为"-f"的文件
* !! :代表上一次输入shell的内容
* ~  :代表用户的主目录

# 重定向
```
* >  :如果文件不存在，就创建文件；如果文件存在，就将其清空
* >> :如果文件不存在，就创建文件；如果文件存在，则将新的内容追加到文件的末尾
```

# ln
创建链接,包括软连接和硬链接

* -s : ln -s source dest  
创建软连接,常用语将某个执行命令加入到PATH内的某个目录中



## xargs
build and execute command lines from standard input

* 通常用于一些不支持不支持管道操作符的命令



## grep 
* -E :指定正则表达式
* -v: 过滤匹配项


## split
* -l :按行数进行拆分
* -a :指定拆分文件的后缀长度
* -d :用数字作为拆分文件的后缀


## which
* which  查看可执行文件的位置。
* whereis 查看文件的位置。 
* locate   配合数据库查看文件位置。
* find   实际搜寻硬盘查询文件名称。

whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。
和find相比，whereis查找的速度非常快，这是因为linux系统会将 系统内的所有文件都记录在一个数据库文件中，当使用whereis和下面即将介绍的locate时，会从数据库中查找数据，而不是像find命令那样，通 过遍历硬盘来查找，效率自然会很高。 
但是该数据库文件并不是实时更新，默认情况下时一星期更新一次，因此，我们在用whereis和locate 查找文件时，有时会找到已经被删除的数据，或者刚刚建立文件，却无法查找到，原因就是因为数据库文件没有被更新。 


which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。 




## |
管道符默认只能通过标准输出



















