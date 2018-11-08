
## 参数文件
mysql 启动时会按照一定的顺序在指定位置读取配置文件,查看方式为:mysql --help | grep "my.cnf";关于数据目录,日志文件等应通过配置文件查找.

## 日志文件
* 错误日志
遇到问题,应该第一时间查看错误日志.
错误日志的位置可以从配置文件中查找,也可以通过 show variables like 'log_error'\G 命令定位.
* 慢查询日志
默认慢查询日志并未开启.对于未经过索引的查找也可以记录的慢查询日志中.也可以将慢查询日志记录到数据库slow_log中
log_slow_queries:慢查询日志位置
long_query_time :慢查询日志时间
log-queries-not-using-indexes :是否记录未经索引查询

## 查询日志
查询日志由general_log参数控制,查看方式为 show variables like '%general_log%'\G
该日志对性能有一定影响,默认文件名为:主机名.log

## 二进制日志
对数据库的更改操作都会记录到该文件中


## 套接字文件
show variables like '%socket%'\G


## PID文件
show variables like '%pid_file%'\G
可以用于mysql服务的pid

## InnoDB存储引擎文件
### 表空间文件
默认为ibdata1
### 重做日志文件
默认为 ib_logfile0,ib_logfile1




## 事务
ACID:原子性（Atomicity）一致性（Consistency）隔离性（Isolation）持久性（Durability）
并发一致性问题:脏读、不可重复读、丢失更新


## 索引
B+树索引、全文索引、哈希索引
B+树索引只能找到一个给定键值所在的具体行
聚集索引上的扫描属于表扫描，辅助索引上的扫描属于索引扫描
### B+树索引
* 聚集索引
聚集索引并非物理上连续的，而是逻辑上连续的。页与页之间通过双向链表链接，单页内的记录间也通过双向链表链接
聚集索引上对主键的排序查找和范围查找非常快
* 辅助索引
辅助索引的叶子节点存储的是刚行的主键（聚集索引键）
B+树应建立在高选择性上（cardinality)
* 应用
数据处理大致可以分成两大类：联机事务处理OLTP（on-line transaction processing）、联机分析处理OLAP（On-Line Analytical Processing）
OLTP 系统强调数据库内存效率，强调内存各种指标的命令率，强调绑定变量，强调并发操作；
OLAP 系统则强调数据分析，强调SQL执行市场，强调磁盘I/O，强调分区等。 
联合索引
sql索引选择上，优先选择索引字段小的值,如字符串的辅助索引和整形的辅助索引，应优先选择整形索引，因为一个磁盘页上可以存放更多整形数，从而减少读取磁盘的次数(所以有可能不应该用字符串当做主键)
覆盖索引，
统计,ICP优化(index condition pushdown)
### 哈希索引
dba无法干预哈希索引的建立
### 倒排索引
通常利用关联数组实现


## 主键选择
非分布式架构直接套用自增id做主键
小规模分布式架构用uuid或者自增id+步长做主键
大规模分布式架构用自建的id生成器做主键，参考twitter的snowflake算法


## 
牢记，默认情况下，InnoDB存储引擎不会回滚超时引发的错误异常
