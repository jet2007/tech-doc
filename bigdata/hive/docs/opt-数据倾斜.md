[TOC]

 

### 数据倾斜

- 倾斜原因：

  map输出数据按key Hash的分配到reduce中，由于**key分布不均匀**、**业务数据本身的特性**、**建表时考虑不周**、**某些SQL语句本身就有数据倾斜**等原因造成的reduce上的数据量差异过大，所以如何将数据均匀的分配到各个reduce中，就是解决数据倾斜的根本所在。

#### 1. 空值数据倾斜

​	join的key值发生倾斜，key值包含很多空值或是异常值，这种情况可以对异常值赋一个随机值来分散key。
​	

​	**案例**：在日志中，常会有信息丢失的问题，比如日志中的 user_id，如果取其中的 user_id 和 用户表中的user_id 关联，会碰到数据倾斜的问题。

```mysql
select * from log l
left outer join user u on 
case when (l.user_id is null or I.user_id='-' or I.user_id='0') 
then concat(‘sql_hive’,rand() ) else l.user_id end = u.user_id;
```



#### 2. Join操作产生数据倾斜



##### 大表和小表Join

​	**产生原因**：Hive在进行join时，按照join的key进行分发，而在join左边的表的数据会首先读入内存，如果左边表的key相对分散，读入内存的数据会比较小，join任务执行会比较快；而如果左边的表key比较集中，而这张表的数据量很大，那么数据倾斜就会比较严重，而如果这张表是小表，则还是应该把这张表放在join左边。
 	**解决方式**：使用map join让小的维度表先进内存。在map端完成reduce。
 	需要在sql中使用  /*+ **MAPJOIN**(smallTable) */  ；

```mysql
SELECT /*+ MAPJOIN(b) */ a.key, a.value
FROM a
JOIN b ON a.key = b.key;
-- a大表，b小表
```



##### 大表和大表Join(一)：

**产生原因**：业务数据本身的特性，导致两个表都是大表。

**情况**：Map输出key数量极少，导致reduce端退化为单机作业。
**解决方式**：业务削减。
**案例**：user 表有 500w+ 的记录，把 user 分发到所有的 map 上也是个不小的开销，而且 map join 不支持这么大的小表。如果用普通的 join，又会碰到数据倾斜的问题。

```mysql
select * from log l left outer join user u
 on l.user_id = u.user_id;
```



**解决方法**：当天登陆的用户其实很少,先只查询当天登录的用户，log里user_id有上百万个，这就又回到原来map join问题。所幸，每日的会员uv不会太多，有交易的会员不会太多，有点击的会员不会太多，有佣金的会员不会太多等等。所以这个方法能解决很多场景下的数据倾斜问题。

```mysql
-- 先过滤被使用的唯一userid,再业务计算
select /*+mapjoin(u2)*/ * from log l2
left outer join
 (
select  /*+mapjoin(l1)*/u1.*
from ( select distinct user_id from log ) l1
join user u1 on l1.user_id = u1.user_id
) u2
on l2.user_id = u2.user_id;
```





##### 大表和大表Join(二)：长尾型

**产生原因**：长尾型数据分布不均。

**情况**：Map输出key分布不均，少量key对应大量value，导致reduce端单机瓶颈。
**案例**：一个实例是商品浏览日志分析，例如将商品信息表i中的信息填充到商品浏览日志表w中，使用商品id关联。但是某些热卖商品浏览量很大，造成数据偏斜。例如，以下语句实现了一个inner join逻辑，将商品信息表拆分成2个表

- 热点与非热点分开处理

```mysql
-- 
select * from(
　　select /*+mapjoin(i)*/ w.id, w.time, w.amount, i1.name, i1.loc, i1.cat
　   from w left join (select * from i where uid in ('1001','1002') ) as i -- 热点子表
         ON W.uid=i.uid 
      where w.uid in('1001','1002') )  -- 热点数据
union all(
　　select w.id, w.time, w.amount, i2.name, i2.loc, i2.cat
　　　　from w left join i ON W.uid=i.uid where w.id not in('1001','1002') ); -- 非热点
```



#### COUNT DISTINCT

##### 多个业务类似的DISTINCT

场景：统计多个维度的同一个度量的COUNT DISTINCT; 示例：根据月份和性别，统计卖家的1月份男顾客数,...,等24个统计。

```mysql
SELECT seller,
       count( DISTINCT CASE WHEN MONTH=1 AND SEX='M' THEN buyer END) M01_MALE_BUYER_CNT,
       count( DISTINCT CASE WHEN MONTH=1 AND SEX='F' THEN buyer END) M01_FEMALE_BUYER_CNT,
       count( DISTINCT CASE WHEN MONTH=2 AND SEX='M' THEN buyer END) M02_MALE_BUYER_CNT,
       count( DISTINCT CASE WHEN MONTH=2 AND SEX='F' THEN buyer END) M02_FEMALE_BUYER_CNT,
       -- 共12月 * 2个性别 = 24个统计
 FROM SHOP_ORDER WHERE BIZ_DATE='2018-03-12'
GROUP BY seller
```

改造方案:

- 把DISTINCT用到的buyer，也加到GROUP BY 统计中
- 上步基础上，再统计业务

```mysql
-- 第一步： 把DISTINCT用到的buyer，也加到GROUP BY 统计中，结果表记录TT
SELECT seller,
       buyer ,
       COUNT(buyer) , -- 值=1 
       count( CASE WHEN MONTH=1 AND SEX='M' THEN buyer END) S_M01_MALE_BUYER_CNT, 
       count( CASE WHEN MONTH=1 AND SEX='F' THEN buyer END) S_M01_FEMALE_BUYER_CNT,
       count( CASE WHEN MONTH=2 AND SEX='M' THEN buyer END) S_M02_MALE_BUYER_CNT,
       count( CASE WHEN MONTH=2 AND SEX='F' THEN buyer END) S_M02_FEMALE_BUYER_CNT,
       -- 共12月 * 2个性别 = 24个统计
 FROM SHOP_ORDER WHERE BIZ_DATE='2018-03-12'
GROUP BY seller,buyer

-- 第2步：表TT只是为了说明方便，实际上为子查询
SELECT seller,
       SUM(CASE WHEN S_M01_MALE_BUYER_CNT>0 THEN 1 ELSE 0 END ) AS M01_MALE_BUYER_CNT
      ,SUM(CASE WHEN S_M01_FEMALE_BUYER_CNT>0 THEN 1 ELSE 0 END ) AS M01_FEMALE_BUYER_CNT
      -- 24个统计
  FROM TT
  GROUP BY seller

```
