---
layout:     post
title:      "shell"
subtitle:   "重定向"
date:       2017-09-18 12:00:00
author:     "Dingding"
header-img: "img/post/start-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - shell 
---

### 重定向
* command > file	将输出重定向到 file。
* command < file	将输入重定向到 file。
* command >> file	将输出以追加的方式重定向到 file。
* n > file	将文件描述符为 n 的文件重定向到 file。
* n >> file	将文件描述符为 n 的文件以追加的方式重定向到 file。
* n >& m	将输出文件 m 和 n 合并。
* n <& m	将输入文件 m 和 n 合并。
* << tag	将开始标记 tag 和结束标记 tag 之间的内容作为输入。
* HereDocument
```
command << delimiter
	document
delimiter
```

### 变量赋值

```
for file in `ls /etc`
do
	echo $file
done
```

```
for file in $(ls /etc)
do
	echo $file
done
```

