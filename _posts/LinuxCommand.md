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
*  -n  删除指定变量,实际并未删除，只是该变量不会在后续执行环境中生效。
*  -p  列出所有的shell赋予程序的环境变量
* export GOPATH=/home/worke/gowork :为环境变量赋值

## mkdir
* -p  递归创建目录

## 环境变量
* 在当前bash环境下读取并执行FileName中的命令。该命令通常用命令“.”来替代。
* source: 在当前bash环境下读取并执行FileName中的命令。只是简单地读取脚本里面的语句依次在当前shell里面执行，没有建立新的子shell,脚本里所有新建、改变变量的语句都会保存在当前shell里面。
* sh filename与./filename :重新建立一个子shell，在子shell中执行脚本里的语句，该子shell继承父shell的环境变量，但子shell新建的、改变的变量不会被带回父shell，除非使用export。

## date
* date :当前时间
* date +%s :当前时间戳
* date -d @1436781720:时间戳转字符串
* date -d '2013-2-22 22:14' +%s
* date -j -f "%Y-%m-%d %H:%M:%S" "2015-07-13 18:02:00" "+%s"



# 远程安全拷贝
*  scp  [parameters] [[user@]host1:]file1 ... [[user@]host2:]file2
*  scp [参数] [原路径] [目标路径]
*  -r  递归copy,用于目录



# 去除重复 uniq
* uinq 只能检测出相邻的向同行，一般与sort命令结合使用。
* -c  prefix lines by the number of occurrences
* -d  only print duplicate lines, one for each group
* -i  ignore differences in case when comparing
* -u  only print unique lines


# 显示、创建文件 cat
* 显示文件 cat filename
* 创建文件 cat filename
* 合并文件 cat file1 file2 > file3
* 追加文件 cat file1 >> file2


# 文件统计 wc
* -l  :统计文件行数，文件结束符也算作了一个newline character
      
# !!  --  ~
* -- :无论如何，将其之后的 argument 视为一个文件名  
    rm --f -f :删除一个名为"-f"的文件
* !! :代表上一次输入shell的内容
* ~  :代表用户的主目录

# 创建链接 ln
* ln -s source dest :创建软连接,常用语将某个执行命令加入到PATH内的某个目录中
* ln source dest:创建硬链接


## grep 
* -E :指定正则表达式
* -v: 过滤匹配项
* -o: 仅仅输出匹配部分
* -P: 使用Perl风格的正则表达式


##  分割文件 split
* -l :按行数进行拆分
* -a :指定拆分文件的后缀长度
* -d :用数字作为拆分文件的后缀

## 查找文件
* which  查看可执行文件的位置,可以用于确定所使用命令的位置。
* whereis 查看文件的位置。 
* locate  配合数据库查看文件位置。
* find   实际搜寻硬盘查询文件名称,应该优先考虑使用其他搜索命令。

## 获取文件绝对路径
* basepath=$(cd `dirname $0`;pwd);

## 查看系统限制
* ulimit -a
* ulimit -n 4096:修改可以打开的文件数上限

## sort
-u :仅输出不重复行
-d:仅输出重复行
-c:输出统计信息

# tail
* 获取文件最后五行:tail -5。
* 监控文件:tail -f。

## 查看端口占用
* lsof -i:端口号 用于查看某一端口的占用情况，比如查看8000端口使用情况，lsof -i:8000
* netstat -tlnp  

## 压缩解压
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

## xars与管道
* xargs :将前一项的输出当做下一项的命令行参数,通常用于不支持管道的命令
* 管道：将前一项的输出当做下一项的标准输入。管道符默认只能通过标准输出,错误输出会被过滤、丢弃。