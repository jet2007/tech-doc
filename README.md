[TOC]

# Github项目

## 报表

| 项目                                                     | 星级 | 介绍                                                         | 说明 |
| -------------------------------------------------------- | ---- | ------------------------------------------------------------ | ---- |
| [CBoard](https://github.com/yzhang921/CBoard)            | ***  | 较完善，CTRIPER开发，The most easiest to use, self-service open BI reporting and BI dashboard platform |      |
| [Superset](https://github.com/apache/incubator-superset) | ***  | 14.6K星级 Apache Superset (incubating) is a modern, enterprise-ready business intelligence web application |      |
| [metabase](https://github.com/metabase/metabase)         | **   | 6K星级，Metabase is the easy, open source way for everyone in your company to ask questions and learn from data. |      |
| [EasyReport](https://github.com/metabase/metabase)       | *    | 一个简单易用的Web报表工具(支持Hadoop,HBase及各种关系型数据库) |      |
| [ART](https://github.com/jet2007/art)                    | **   | Ctrip改进后的ART报表系统                                     |      |



## 调度



| 项目                                                         | 星级 | 介绍                                                         | 说明                                                         |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Easy Scheduler](https://github.com/analysys/EasyScheduler)  | **** | Easy Scheduler是一个分布式工作流任务调度系统，主要解决"错综复杂的任务依赖关系，而不能直观监控任务健康状态等问题"。Easy Scheduler以DAG流式的方式将Task组装起来，并可实时监控任务的运行状态，同时支持重试、从指定节点恢复失败、暂停及Kill任务等操作。EasyScheduler由在工作流调度方面工作多年的多位小伙伴研发而成，致力于成为大数据平台的中流砥柱，使调度变得更加容易 |                                                              |
| [Hera](https://github.com/scxwhite/hera)                     | **** | hera 分布式任务调度系统 大数据任务调度系统 任务调度 （数据部门专用）;基于Ali Zeus系统；二次开发版本https://github.com/jet2007/hera的release版本hera-20190422(自定义作业类型及页面布局调整) |                                                              |
| [Airflow](https://github.com/azkaban/azkaban)                | ***  | Airflow is a platform to programmatically author, schedule and monitor workflows。 | [dag编辑工具](https://github.com/lattebank/airflow-dag-creation-manager-plugin) |
| [Azkaban](https://github.com/azkaban/azkaban)                | ***  | Azkaban workflow manager<br />[azkaban_assistant***](https://github.com/cocofree/azkaban_assistant) - azkaban小助手,增加任务web配置、远程脚本调用、报警扩展、跨项目依赖等功能。V2.5,单机SSH远程执行作业 |                                                              |
| [dataworks-zeus](https://github.com/ctripcorp/dataworks-zeus) - | **   | Ctrip Hadoop Job Scheduling System derived from https://github.com/alibaba/zeus |                                                              |
| [light-task-scheduler](https://github.com/ltsopensource/light-task-scheduler) | **   | LTS(light-task-scheduler)主要用于解决分布式任务调度问题，支持实时任务，定时任务和Cron任务。有较好的伸缩性，扩展性 |                                                              |
| [pinball](https://github.com/pinterest/pinball)              | **   | Pinball is a scalable workflow manager                       |                                                              |



## ETL

| 项目                                                         | 星级 | 介绍                                                         | 说明 |
| ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ---- |
| [DataX](https://github.com/alibaba/DataX)                    | **** | DataX 是阿里巴巴集团内被广泛使用的离线数据同步工具/平台，实现包括 MySQL、Oracle、HDFS、Hive、OceanBase、HBase、OTS、ODPS 等各种异构数据源之间高效的数据同步功能 |      |
| [Embulk](https://github.com/embulk/embulk)                   | **** | Embulk is a parallel bulk data loader that helps data transfer between various storages, databases, NoSQL and cloud services.有Input,Filter,output不同插件 |      |
| [HData](<https://github.com/jet2007/HData-ETL>)              | **** | 一个支持多数据源的ETL数据导入/导出工具，做了二次开发；[原代码](https://github.com/jet2007/hdata-Native) |      |
| [StreamSets Data Collector](https://github.com/streamsets/datacollector) | ***  | 流式数据处理导入导出工具，WEB IDE；支持源有JDBC,HADOOP,REDISTF;支持目标有JDBC,HADOOP,HBASE,REDIS,ELASITCSEARCH,SOLR等; |      |
| [awesome-etl](https://github.com/pawl/awesome-etl)           | ***  | A curated list of awesome ETL frameworks, libraries, and software. |      |
| [FirstBlood](https://github.com/hanson007/FirstBlood)        | **   | 精简版数据同步工具 ETL（DATAX WEB版本），[介绍](https://www.cnblogs.com/huangxiaoxue/p/9392817.html) |      |
| [flinkx](https://github.com/DTStack/flinkx)                  | ***  | 基于flink的分布式数据同步工具;FlinkX是在是袋鼠云内部广泛使用的基于flink的分布式离线数据同步框架，实现了多种异构数据源之间高效的数据迁移 |      |



数据库同步



| 项目                                              | 星级 | 介绍                                                         | 说明 |
| ------------------------------------------------- | ---- | ------------------------------------------------------------ | ---- |
| [otter](https://github.com/alibaba/otter)         | ***  | 阿里巴巴分布式数据库同步系统(解决中美异地机房)；定位： 基于数据库增量日志解析，准实时同步到本机房或异地机房的mysql/oracle数据库. 一个分布式数据库同步系统；otter已在阿里云推出商业化版本 [数据传输服务DTS] |      |
| [DataLink](https://github.com/ucarGroup/DataLink) | ***  | DataLink是一个满足各种异构数据源之间的实时增量同步，分布式、可扩展的数据交换平台。 |      |
| canal                                             |      |                                                              |      |
|                                                   |      |                                                              |      |
|                                                   |      |                                                              |      |



## 数据仓库辅助

| 项目                                                     | 星级 | 介绍                                                         | 说明 |
| -------------------------------------------------------- | ---- | ------------------------------------------------------------ | ---- |
| [CBoard](https://github.com/yzhang921/CBoard)            | ***  | 较完善，CTRIPER开发，The most easiest to use, self-service open BI reporting and BI dashboard platform |      |
| [Superset](https://github.com/apache/incubator-superset) | ***  | 14.6K星级 Apache Superset (incubating) is a modern, enterprise-ready business intelligence web application |      |
| [metabase](https://github.com/metabase/metabase)         | **   | 6K星级，Metabase is the easy, open source way for everyone in your company to ask questions and learn from data. |      |
| [EasyReport](https://github.com/metabase/metabase)       | *    | 一个简单易用的Web报表工具(支持Hadoop,HBase及各种关系型数据库) |      |
| [ART](https://github.com/jet2007/art)                    | **   | Ctrip改进后的ART报表系统                                     |      |



## Hive相关

| 项目                                                     | 星级 | 介绍                                                         | 说明 |
| -------------------------------------------------------- | ---- | ------------------------------------------------------------ | ---- |
| [CBoard](https://github.com/yzhang921/CBoard)            | ***  | 较完善，CTRIPER开发，The most easiest to use, self-service open BI reporting and BI dashboard platform |      |
| [Superset](https://github.com/apache/incubator-superset) | ***  | 14.6K星级 Apache Superset (incubating) is a modern, enterprise-ready business intelligence web application |      |
| [metabase](https://github.com/metabase/metabase)         | **   | 6K星级，Metabase is the easy, open source way for everyone in your company to ask questions and learn from data. |      |
| [EasyReport](https://github.com/metabase/metabase)       | *    | 一个简单易用的Web报表工具(支持Hadoop,HBase及各种关系型数据库) |      |
| [ART](https://github.com/jet2007/art)                    | **   | Ctrip改进后的ART报表系统                                     |      |



## 工具型

| 项目                                                     | 星级 | 介绍                                                         | 说明 |
| -------------------------------------------------------- | ---- | ------------------------------------------------------------ | ---- |
| [CBoard](https://github.com/yzhang921/CBoard)            | ***  | 较完善，CTRIPER开发，The most easiest to use, self-service open BI reporting and BI dashboard platform |      |
| [Superset](https://github.com/apache/incubator-superset) | ***  | 14.6K星级 Apache Superset (incubating) is a modern, enterprise-ready business intelligence web application |      |
| [metabase](https://github.com/metabase/metabase)         | **   | 6K星级，Metabase is the easy, open source way for everyone in your company to ask questions and learn from data. |      |
| [EasyReport](https://github.com/metabase/metabase)       | *    | 一个简单易用的Web报表工具(支持Hadoop,HBase及各种关系型数据库) |      |
| [ART](https://github.com/jet2007/art)                    | **   | Ctrip改进后的ART报表系统                                     |      |



## sql解析类

| 项目                                                     | 星级 | 介绍                                                         | 说明 |
| -------------------------------------------------------- | ---- | ------------------------------------------------------------ | ---- |
| [CBoard](https://github.com/yzhang921/CBoard)            | ***  | 较完善，CTRIPER开发，The most easiest to use, self-service open BI reporting and BI dashboard platform |      |
| [Superset](https://github.com/apache/incubator-superset) | ***  | 14.6K星级 Apache Superset (incubating) is a modern, enterprise-ready business intelligence web application |      |
| [metabase](https://github.com/metabase/metabase)         | **   | 6K星级，Metabase is the easy, open source way for everyone in your company to ask questions and learn from data. |      |
| [EasyReport](https://github.com/metabase/metabase)       | *    | 一个简单易用的Web报表工具(支持Hadoop,HBase及各种关系型数据库) |      |
| [ART](https://github.com/jet2007/art)                    | **   | Ctrip改进后的ART报表系统                                     |      |





# 技术文档：Gitbook+Github示例

- 以tech-doc\bigdata\hive目录为例
- 技术文档的源目录：src-docs
  - 在src-docs目录此下，gitbook serve或build后，生成_book目录
  - 将_book目录下的所有文件(夹)COPY到bigdata\hive目录下，访问jet2007.github.io/tech-doc/bigdata/hive展示出gitbook内容。


