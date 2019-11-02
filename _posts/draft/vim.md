1.:g/^/exe ":s/^/".line(".")."/"
2.:r !seq 1 100
3.插入模式下，按顺序输入 ctrl-r，=，range(1,100)，回车
4.:g/^文字 /s//\=printf("%s%d",submatch(0),line("."))/g

CTRL-A 命令在宏命令里很有用。例如: 使用以下的步骤构造一个数字编号的列表。

(1). 建立第一个列表项。确保它以数字开始。
(2). qa - 用寄存器 'a' 开始记录
(3). Y - 抽出这个列表项
(4). p - 把该项的一个副本放置在下一行上
(5). CTRL-A - 增加计数
(6). q - 停止记录
(7). <count>@a - 重复抽出、放置和增加计数操作 <count> 次

举例:
a
b
c
变为：
1. a
2. b
3. c


操作
step0.
a
b
c
step1. 添加一个0，单独起一行
0
a
b
c

step2. qa yw j P CTRL-A a . space ESC q
0
1. a
b
c

step3.删除、恢复
0
a
b
c

step4.选中行，执行 :normal @a
1. a
2. b
3. c





