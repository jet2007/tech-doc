[TOC]

# UDF使用

  介绍第三方开发的udf函数



## 日期加减计算

- ## 介绍：仿照python relativedelta的日期加减计算，使用一个函数实现多次日期加减计算(如月份，日期相加)
- 地址：<https://github.com/jet2007/HiveDateUDF>   [jar包](https://github.com/jet2007/HiveDateUDF/releases/download/rc-0.2/jet-hive-date-udf-0.2.jar)
- [详细介绍](https://github.com/jet2007/HiveDateUDF/blob/master/README.md)
- DEMO


| DESC                          | CODE                                                         | RESULT              |
| ----------------------------- | ------------------------------------------------------------ | ------------------- |
| day +3                        | SELECT DateDelta('20180102','+3')                            | 20180105            |
| day +3                        | SELECT DateDelta('20180102','day=+3')                        | 20180105            |
| day +3                        | SELECT DateDelta('2018-01-02','day=+3')                      | 2018-01-05          |
| datetime +3day                | SELECT DateDelta('20180102030405','day=+3')                  | 20180105030405      |
| datetime +3day                | SELECT DateDelta('2018-01-02 03:04:05','day=+3')             | 2018-01-05 03:04:05 |
| end day of this year          | SELECT DateDelta('2018-09-06','year=+1,month=1,day=1,day=-1') | 2018-12-31          |
| end day of last month         | SELECT DateDelta('20180906','day=1,day=-1')                  | 20180831            |
| end day of this year          | SELECT DateDelta('2018-09-06','y=+1,m=1,d=1,d=-1')           | 2018-12-31          |
| end day of last month(Format) | SELECT DateDelta('2018-09-06','day=1,day=-1','<u>**yyyyMMdd**</u>') | 20180831            |



## hive-third-functions

- 介绍：hive-third-functions 包含了一些很有用的hive udf函数，特别是数组和json函数.
- 地址：<https://github.com/aaronshan/hive-third-functions>； [jar包](https://github.com/aaronshan/hive-third-functions/releases/download/2.2.0/hive-third-functions-2.2.0-shaded.zip)
- 详细：<https://github.com/aaronshan/hive-third-functions/blob/master/README-zh.md>
- DEMO

```sql
select array_contains(array(16,12,18,9), 12) => true
select array_equals(array(16,12,18,9), array(16,12,18,9)) => true
select array_intersect(array(16,12,18,9,null), array(14,9,6,18,null)) => [null,9,18]
select array_max(array(16,13,12,13,18,16,9,18)) => 18
select array_min(array(16,12,18,9)) => 9
select array_join(array(16,12,18,9,null), '#','=') => 16#12#18#9#=
select array_distinct(array(16,13,12,13,18,16,9,18)) => [9,12,13,16,18]
select array_position(array(16,13,12,13,18,16,9,18), 13) => 2
select array_remove(array(16,13,12,13,18,16,9,18), 13) => [16,12,18,16,9,18]
select array_reverse(array(16,12,18,9)) => [9,18,12,16]
select array_sort(array(16,13,12,13,18,16,9,18)) => [9,12,13,13,16,16,18,18]
select array_concat(array(16,12,18,9,null), array(14,9,6,18,null)) => [16,12,18,9,null,14,9,6,18,null]
select array_value_count(array(16,13,12,13,18,16,9,18), 13) => 2
select array_slice(array(16,13,12,13,18,16,9,18), -2, 3) => [9,18]
select array_element_at(array(16,13,12,13,18,16,9,18), -1) => 18
select array_shuffle(array(16,12,18,9))
select sequence(1, 5) => [1, 2, 3, 4, 5]
select sequence(5, 1) => [5, 4, 3, 2, 1]
select sequence(1, 9, 4) => [1, 5, 9]
select sequence('2016-04-12 00:00:00', '2016-04-14 00:00:00', 24*3600*1000) => ['2016-04-12 00:00:00', '2016-04-13 00:00:00', '2016-04-14 00:00:00']
```

```sql
select json_array_get("[{\"a\":{\"b\":\"13\"}}, {\"a\":{\"b\":\"18\"}}, {\"a\":{\"b\":\"12\"}}]", 1); => {"a":{"b":"18"}}
select json_array_get('["a", "b", "c"]', 0); => a
select json_array_get('["a", "b", "c"]', 1); => b
select json_array_get('["c", "b", "a"]', -1); => a
select json_array_get('["c", "b", "a"]', -2); => b
select json_array_get('[]', 0); => null
select json_array_get('["a", "b", "c"]', 10); => null
select json_array_get('["c", "b", "a"]', -10); => null
select json_array_length("[{\"a\":{\"b\":\"13\"}}, {\"a\":{\"b\":\"18\"}}, {\"a\":{\"b\":\"12\"}}]"); => 3
select json_array_extract("[{\"a\":{\"b\":\"13\"}}, {\"a\":{\"b\":\"18\"}}, {\"a\":{\"b\":\"12\"}}]", "$.a.b"); => ["\"13\"","\"18\"","\"12\""]
select json_array_extract_scalar("[{\"a\":{\"b\":\"13\"}}, {\"a\":{\"b\":\"18\"}}, {\"a\":{\"b\":\"12\"}}]", "$.a.b") => ["13","18","12"]
select json_extract("{\"a\":{\"b\":\"12\"}}", "$.a.b"); => "12"
select json_extract_scalar("{\"a\":{\"b\":\"12\"}}", "$.a.b") => 12
select json_extract_scalar('[1, 2, 3]', '$[2]');
select json_extract_scalar(json, '$.store.book[0].author');
select json_size('{"x": {"a": 1, "b": 2}}', '$.x'); => 2
select json_size('{"x": [1, 2, 3]}', '$.x'); => 3
select json_size('{"x": {"a": 1, "b": 2}}', '$.x.a'); => 0
```



## IP地址解析：IP-->地址位置

- 含义：解析IP地址
- 返回值：返回Map类型的值；示例：{"cityName":"上海市","ispName":"联通","regionName":null,"continentName":"中国","provinceName":"上海"}
- 参数1：必选，IP地址
- 参数2：可选；add file hdfs路径的文件名，如下例的"ip2region.db"; 默认值=ip2region.db

- 地址及创建函数：

  - [hive-udf-0.2.jar](https://github.com/jet2007/HiveDateUDF/releases/download/rc-0.2/hive-udf-0.2.jar) 
  - ip2region相关：[ip2region.db](https://github.com/jet2007/HiveDateUDF/releases/download/rc-0.2/ip2region.db), [ip2region-1.7.jar](https://github.com/jet2007/HiveDateUDF/releases/download/rc-0.2/ip2region-1.7.jar)

  ```sql
  -- 说明：第1行为ip2region的数据文件，第2行为ip2region的jar包，第3行为udf的jar包
  add file hdfs://quickstart.cloudera:8020/user/hive/warehouse/func.db/ip2region.db
  ADD jar /home/cloudera/tmp/chm/udf/ip2region-1.7.jar
  ADD jar /home/cloudera/tmp/chm/udf/hive-udf-0.2.jar
  CREATE TEMPORARY FUNCTION ip2geo AS com.ct.dw.udf.geoip.UDFIp2RegionByHDFS
  ```

- 使用示例

  ```sql
  SELECT ip2geo('221.226.1.30','ip2region.db')
  SELECT ip2geo('221.226.1.30') -- 与上例等价
  ```

- ip2region.db说明

  - 来源：https://github.com/lionsoul2014/ip2region
  - ip2region的数据聚合自以下服务商的开放API或者数据(升级程序每秒请求次数2到4次): 
    01, >80%, 淘宝IP地址库, <http://ip.taobao.com/> 
    02, ≈10%, GeoIP, <https://geoip.com/> 
    03, ≈2%, 纯真IP库, <http://www.cz88.net/> 

- 备注UDF取HDFS文件的主要CODE

  ```java
  	protected File getLookupFile(String lookupFile) {
  		/* distributed cache */
  		File f = new File(lookupFile); // file available locally
  		if (!f.exists()) {
  			/* local resources (non-MR mode) */
  			File resourceDir = new File(
  					SessionState.get().getConf().getVar(HiveConf.ConfVars.DOWNLOADED_RESOURCES_DIR));
  			f = new File(resourceDir, lookupFile);
  		}
  		return f;
  	}
  ```



#####  