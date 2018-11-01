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








