# Hive 函数

- Help

> show functions like '\*date\*' 
>
> desc function max



## 复杂类型

| Func   | Code                                  | Result                                                   | Desc   |
| ------ | ------------------------------------- | -------------------------------------------------------- | ------ |
| array  | select array(1,2,3,4)                 | [1,2,3,4]                                                | 数组   |
| map    | SELECT map('k1','val1','key2','val2') | {"k1":"val1","key2":"val2"}                              | map    |
| struct | SELECT  struct(1,'val2',3,'val4',5)   | {"col1":1,"col2":"val2","col3":3,"col4":"val4","col5":5} | struct |



## 条件函数

| Func     | Code                               | Result     | Desc             |
| -------- | ---------------------------------- | ---------- | ---------------- |
| if       | select if(1=1,'a','b')             | a          | 二元条件判断     |
| nvl      | select nvl(null,'a')               | a          | 空值二元条件判断 |
| coalesce | select coalesce(null,null,'a','b') | a          | 空值多元条件判断 |
| isnull   | select isnull(1),isnotnull(1)      | true,false | 是否为空         |



## 字符串函数

| Func           | Code                                                         | Result                  | Desc                                                         |
| -------------- | ------------------------------------------------------------ | ----------------------- | ------------------------------------------------------------ |
| concat         |                                                              |                         | 字符串按次序进行拼接                                         |
| concat_ws      | select concat_ws('_','1','2','3')                            | 1_2_3                   | 与concat类似,指定的分隔符喜进行分隔                          |
| find_in_set    | select find_in_set('ab', 'abc,b,ab,c,def,ab')<br />select find_in_set('ab', 'abc,b,a,c,def,ab') | 3<br />6                | 返回以逗号分隔的字符串中str出现的位置，如果参数str为逗号或查找失败将返回0 |
| regexp_extract | select regexp_extract('foothebar', 'foo(.*?)(bar)', 2)       | bar<br />若取第1个为the | 抽取字符串subject中符合正则表达式pattern的第index个部分的子字符串 |
| regexp_replace | select regexp_replace('foobar','oo\|ar','#')                 | f#b#                    | 与上类似，替换                                               |
| split          | select split('1,2,3,4',',')                                  | ["1","2","3","4"]       | 拆分成数组                                                   |
| str_to_map     | select str_to_map('k1=v1,k2=v2',',','=')                     | {"k1":"v1","k2":"v2"}   |                                                              |



## 日期函数

| Func      | Code                                                         | Result     | Desc                                                         |
| --------- | ------------------------------------------------------------ | ---------- | ------------------------------------------------------------ |
| DateDelta | SELECT DateDelta('2018-09-06','year=+1,month=1,day=1,day=-1') | 2018-12-31 | end day of this year<br />https://github.com/jet2007/HiveDateUDF |



## 表生成函数

| Func    | Code                                                         | Result            | Desc                     |
| ------- | ------------------------------------------------------------ | ----------------- | ------------------------ |
| explode | select explode(t.a) as ex_a from (
<br/>select array(1,2,3) as a union all select array(4,3) as a ) t | 1,2,3,4,3五行记录 | 从原表的2行，生成5行数据 |

- lateral view用于和split、explode等UDTF一起使用的，能将一行数据拆分成多行数据，在此基础上可以对拆分的数据进行聚合
- select A,B,C from table_1 LATERAL VIEW explode(B) table1 as B

```
A                         B                                C
190     [1030,1031,1032,1033,1190]      select id
191     [1030,1031,1032,1033,1190]      select id

190    1030  select id
190    1031  select id
190    1032  select id
190    1033  select id
```

