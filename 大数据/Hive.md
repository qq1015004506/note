# Hive

Hive 分成四个部分：

![](D:\book\note\imgs\大数据\Hive架构.png)

* Meta Store（黄）：

  这部分保存在数据库中，数据库可以是MySQL。数据存放在HDFS，表结构存放在MetaStore中。每次进行Hive查询时根据Meta store中保存的结构信息去HDFS上解析数据。

* Client（红）:

  是终端，一般是Linux命令行，负责与Hive 核心部分进行交互。

* Hive Core（绿）:

  将HQL（SQL）转换成MapReduce，去HDFS上执行命令。对HDFS进行修改和查询。

* Hadoop（蓝）:

  就是HDFS，保存数据，为HDFS提供数据源。