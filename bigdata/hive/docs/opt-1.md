[TOC]



### 合并小文件

#### 问题

##### 哪里会产生小文件 ?

- 源数据本身有很多小文件
- 动态分区会产生大量小文件
- reduce个数越多, 小文件越多
- 按分区插入数据的时候会产生大量的小文件, 文件个数 = maptask个数 * 分区数

##### 小文件太多造成的影响 ?

从Hive的角度看，小文件会开很多map，一个map开一个JVM去执行，所以这些任务的初始化，启动，执行会浪费大量的资源，严重影响性能。

HDFS存储太多小文件, 会导致namenode元数据特别大, 占用太多内存, 制约了集群的扩展



#### 解决方法

##### 方法一: 通过调整参数进行合并：

```sql
#每个Map最大输入大小(这个值决定了合并后文件的数量)
set mapred.max.split.size=256000000;

#一个节点上split的至少的大小(这个值决定了多个DataNode上的文件是否需要合并)
set mapred.min.split.size.per.node=100000000;

#一个交换机下split的至少的大小(这个值决定了多个交换机上的文件是否需要合并)
set mapred.min.split.size.per.rack=100000000;

#执行Map前进行小文件合并
set hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;

#===设置map输出和reduce输出进行合并的相关参数：

#设置map端输出进行合并，默认为true
set hive.merge.mapfiles = true

#设置reduce端输出进行合并，默认为false
set hive.merge.mapredfiles = true

#设置合并文件的大小
set hive.merge.size.per.task = 256*1000*1000

#当输出文件的平均大小小于该值时，启动一个独立的MapReduce任务进行文件merge。
set hive.merge.smallfiles.avgsize=16000000
```



##### 方法二: DISTRIBUTE BY+每个Reduce处理大小（推荐方案）

针对按分区插入数据的时候产生大量的小文件的问题, 可以使用DISTRIBUTE BY rand() 将数据随机分配给Reduce，这样可以使得每个Reduce处理的数据大体一致

```sql
# 设置每个reducer处理的大小为1个G示例值(可设置为128M，256M)
set hive.exec.reducers.bytes.per.reducer=1073741824;

# 使用distribute by rand()将数据随机分配给reduce, 避免出现有的文件特别大, 有的文件特别小
insert overwrite table test partition(dt)
select * from iteblog_tmp
DISTRIBUTE BY rand();
```



##### **方法三: 使用Sequencefile作为表存储格式，不要用textfile，在一定程度上可以减少小文件**



##### **方法四: 使用hadoop的archive归档**(最后备选方案)

- 原理：把分区目录下的文件打包成1个har文件(或多个文件)

```
#用来控制归档是否可用
set hive.archive.enabled=true;
#通知Hive在创建归档时是否可以设置父目录
set hive.archive.har.parentdir.settable=true;
#控制需要归档文件的大小 1024G
set har.partfile.size=1099511627776;

#使用以下命令进行归档
ALTER TABLE srcpart ARCHIVE PARTITION(ds='2008-04-08', hr='12');

#对已归档的分区恢复为原文件
ALTER TABLE srcpart UNARCHIVE PARTITION(ds='2008-04-08', hr='12');

```



---


