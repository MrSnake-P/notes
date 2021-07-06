[TOC]

## neo4j 一些指令

* 查询

```python
match (a:address) where a.ExchangeName='Huobi.com' 
return a.address_hash skip 0 limit 10	# 查询哈希值
```

* 创建节点的同时创建关系

  ```
  merge (a:xxx{zz:'xx'}) with a
  merge (b:xxx{zz:'yy'}) with a b
  creata (a)-[r:yyy{zz:'zz'}]->(b)
  ```

## neo4j 字符串操作

| 函数    | 作用                         |
| ------- | ---------------------------- |
| Trim()  | 删除字符串左右两边多余的空格 |
| lTrim() | 删除字符串左边多余的空格     |
| rTrim() | 删除字符串右边多余的空格     |

## neo4j 批量插入数据

[官方文档](https://neo4j.com/developer/guide-import-csv/#batch-importer)

1.连接neo4j

```
import neo4j
neo4j_url = 'bolt://192.168.3.24:7687'
driver = neo4j.GraphDatabase.driver(neo4j_url, auth=('neo4j', 'jifengfeicui'), encrypted=False)

session=driver.session()

cypher = "with {0} as list  match (a:address)-[]->(t) where a.address_hash in list return t.txhash ".format(list(address_set))
transaction_results = session.run(cypher)
```

2.csv数据插入

## kafka

[官方文档](https://kafka.apache.org/quickstart)

* 取得 KAFKA

  ```bash
  $ tar -xzf kafka_2.13-2.7.0.tgz
  $ cd kafka_2.13-2.7.0
  ```

* 启动环境

  * 本地环境安装需求 Java 8+

  ```bash
  $ bin/zookeeper-server-start.sh config/zookeeper.properties
  ```
  * 另一个终端会话

    ```bash
    $ bin/kafka-server-start.sh config/server.properties
    ```

* 建立主题储存您的活动

  ```bash
  $ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
  ```

* 显示 [详细信息，例如](https://kafka.apache.org/documentation/#intro_topics) 新主题[的分区数](https://kafka.apache.org/documentation/#intro_topics)

  ```bash
  & bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
  
  Topic:quickstart-events  PartitionCount:1    ReplicationFactor:1 Configs:
      Topic: quickstart-events Partition: 0    Leader: 0   Replicas: 0 Isr: 0
  ```

  

* 启动生产者 

  ```bash
  $ bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
  ```

* 启动消费者

  ```bash
  $ bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
  ```

* 删除本地环境

  ```bash
  $ rm -rf /tmp/kafka-logs /tmp/zookeeper
  ```

## neo4j-admin import

```shell
../bin/neo4j-admin import --database orders
     --nodes=Customer=customers.csv
     --nodes=Order=orders_header.csv
     --relationships=ORDERED=
     "customer_orders_header.csv,orders1.csv,orders2.csv"
     --trim-strings=true
 在shell上为一行输入
```

插入原理：先插入节点，再根据关系表头中设置的参数，寻找节点建立关系。

**note：必要的类型有，**`:Id` `:START_ID` `:END_ID`

### 其他一些类型

* `:LABEL` # 节点的标签列

  ```shell
  example.csv/
  name,age,:LABEL
  snake,21,PERSON
  snake2,22,PERSON
  ```

* `:type` # 指定关系类型的列，即根据指定的名字来生成关系

* `:int` `:float` # 数值类型

### 插入文件设置

```shell
address_head.csv/
address:ID
------------------
address.csv/
3MbYQMMmSkC3AgWkj9FMo5LsPTW1zBTwXL
3MbYQMMmSkC3AgWkj9FMo5LsPTW1zBTwXL
132j6EiUWNamSSjiYEQBhsbufsnBh4a28U
1LrKcqA81updDyASYW6113yXFrDrJ1xizs
1CXE1tU9XKLRog1FTHXhQFkL3FrwWodumd
----------------------------------
relationship_head.csv/
txid,fee:float,sendingaddress:START_ID,referenceaddress:END_ID,version:int
--------------------------------------------------------------------
relationship.csv/
ce36efda15bc6cf99ba6a010e71b47b00a5ea2071a392839effe7ed392cf690f,0.00010000,3MbYQMMmSkC3AgWkj9FMo5LsPTW1zBTwXL,132j6EiUWNamSSjiYEQBhsbufsnBh4a28U,0
f82f2f5a557ff3ed65984d80fa6de31541c879e12de56fd9d489326c6ecf5079,0.00010000,132j6EiUWNamSSjiYEQBhsbufsnBh4a28U,1En8LrBs7FGekRX3zSsmEVuPz2KVUEtCy2,0
--------------------------------------------------------------------
# 在shell中执行
$ sudo neo4j-admin import --database omni-usdt --high-io=true --skip-bad-relationships=true --nodes=USDT_OMNI="head1.csv,address3.csv" --relationships="relationship_head.csv,relationship.csv" --skip-duplicate-nodes=true
--------------------------------------------------------------------
# 其中参数的意义
--high-io 	# 加快插入速度
--skip-bad-relationships 	# 跳过报错的关系
--skip-duplicate-nodes	# 跳过重复的节点
```

