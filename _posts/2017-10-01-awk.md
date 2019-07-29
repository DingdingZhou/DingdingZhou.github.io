---
layout:     post
title:      "Linux Command"
subtitle:   "awk"
date:       2017-09-25 12:00:00
author:     "Dingding"
header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog:    true
tags:
    - awk 
    - linux command
---

# awk
pattern scanning and text processing language

* 优秀博客

[三十分钟学会AWK](http://blog.jobbole.com/109089/)


* 工作流程

```flow
st=>start: Excute AWK commands from BEGIN block
e=>end: Excute AWK commands from END block

read=>operation: Read a line from input stream
execute=>operation: Execute AWK commands on a line
cond=>condition: Is it End of File

st->read->execute->cond
cond(yes)->e
cond(no,right)->read
```


* 程序框架

注意:BEGIN/End是awk的关键字,必须大写

```

BEGIN {awk-commands}

/pattern/ {awk-commands}

END {awk-commands}
```


# awk example
```sh
#!/bin/bash

awk '{
	for (i=1;i<NF;i++){
		printf $i "\t";
	}
	cmd=("date +%Y-%m-%d-%H -d @" $NF);
	system(cmd);
	printf("\n"); 
}' $1;
```


