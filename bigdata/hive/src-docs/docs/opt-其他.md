[TOC]

### Map阶段的优化

​	控制hive任务中的map数，确定合适的map数，以及每个map处理合适的数据量



- map个数影响因子
  - input目录中文件总个数；
  - input目录中每个文件大小；
  - 集群设置的文件块大小(默认为128M, 可在hive中通过set dfs.block.size;命令查看，不能在hive中自定义修改)；

```c
举例：
input目录中有1个文件（300M），会产生3个块（2个128M，1个44M）即3个Map数。
input目录中有3个文件（5M,10M,200M），会产生4个块（5M,10M,128M,72M）即4个Map数。
```



##### 减少Map数：

​	当一个任务有很多小文件（远远小于块大小128m）,会产生很多Map，

​	而一个**Map任务启动和初始化的时间**远远大于逻辑处理的时间，就会造成很大的资源浪费，而且同时可执行的map数是受限的。



```mysql
set mapred.max.split.size=100000000; -- （100M）
set mapred.min.split.size.per.node=100000000;
set mapred.min.split.size.per.rack=100000000;
set hive.input.format=org.apache.hadoop.hive.ql.io.CombineHiveInputFormat;//表示执行前进行小文件合并。
//大于128:按照128M分割；100~128按照100分；小于100的进行合并。
```



##### 增加Map数

​	当有一个小于128M的文件(其中有上千万条的数据，字段少并且数据单位小)

​	如果map处理的逻辑比较复杂，用**一个map**任务去做，耗时比较大。

```mysql
set mapred.reduce.tasks=10;
create table a_1 as 
select * from a distribute by rand();
-- 表示通过设置Map任务数来中加Map，把a表中的数据均匀的放到a_1目录下10个文件中。
```



#### Reduce阶段的优化

- reduce数量:　min(reducers.max，  总输入数据量/ bytes per reducer))

```
param1:hive.exec.reducers.bytes.per.reducer（默认为1000^3）
param2:hive.exec.reducers.max（默认为999）
```



​	



### 作业名

#### mr引擎: set mapred.job.name=job100001

```

set hive.execution.engine=mr;
set mapred.job.name=job100001;
create table dw_tmp_db.tmp_dm_mid_mbl_mdr_agg_daily_051 as select * from dw_tmp_db.tmp_dm_mid_mbl_mdr_agg_daily_042 limit 111;

set mapred.job.name=job100002;
create table  dw_tmp_db.tmp_dm_mid_mbl_mdr_agg_daily_052 as select * from dw_tmp_db.tmp_dm_mid_mbl_mdr_agg_daily_042 limit 111;

set mapred.job.name=job100003;
drop table if exists dw_tmp_db.tmp_dm_mid_mbl_mdr_agg_daily_051;
drop table if exists dw_tmp_db.tmp_dm_mid_mbl_mdr_agg_daily_052;
```



#### Tez设置appName

- Tez是通过设置session.id来设置appName的
- 在YARN 的Web UI界面查看到的appName和使用yarn application -list查看到的appName一样
- 必须在hive启动的时候设置，如果进入hive里面设置 `hive.session.id`，YARN的Web UI界面和yarn application -list查看的结果是**不生效**的。

```
hive --hiveconf hive.session.id=appName.id
```

