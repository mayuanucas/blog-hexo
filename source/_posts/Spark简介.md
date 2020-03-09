---
title: Spark简介
date: 2020-03-09 17:54:03
tags: 
- scala
- spark
categories: 
- 大数据
---

Apache Spark 是一个快速的，多用途的集群计算系统。它提供了 Java，Scala，Python 和 R 的高级 API，以及一个支持通用的执行图计算的优化过的引擎。它还支持一组丰富的高级工具，包括使用 SQL 处理结构化数据处理的 [Spark SQL](https://spark.apache.org/docs/latest/sql-programming-guide.html)，用于机器学习的 [MLlib](https://spark.apache.org/docs/latest/ml-guide.html)，用于图计算的 [GraphX](https://spark.apache.org/docs/latest/graphx-programming-guide.html)，以及 [Spark Streaming](https://spark.apache.org/docs/latest/streaming-programming-guide.html)。

<!-- more -->

# 简介

Spark 于 2009 年诞生于加州大学伯克利分校 AMPLab，2013 年被捐赠给 Apache 软件基金会，2014 年 2 月成为 Apache 的顶级项目。相对于 MapReduce 的批处理计算，Spark 可以带来上百倍的性能提升，因此它成为继 MapReduce 之后，最为广泛使用的分布式计算框架。

# 特点

Apache Spark 具有以下特点：

- 使用先进的 DAG 调度程序，查询优化器和物理执行引擎，以实现性能上的保证；
- 多语言支持，目前支持的有 Java，Scala，Python 和 R；
- 提供高级 API，可以轻松地构建应用程序；
- 支持批处理，流处理和复杂的业务分析；
- 丰富的类库支持：包括 SQL，MLlib，GraphX 和 Spark Streaming 等库，并且可以将它们无缝地进行组合；
- 丰富的部署模式：支持本地模式和自带的集群模式，也支持在 Hadoop，Mesos，Kubernetes 上运行；
- 多数据源支持：支持访问 HDFS，Alluxio，Cassandra，HBase，Hive 以及数百个其他数据源中的数据。

{% asset_img future-of-spark.png %}

# 集群架构

| Term（术语）    | Meaning（含义）                                              |
| --------------- | ------------------------------------------------------------ |
| Application     | Spark 应用程序，由集群上的一个 Driver 节点和多个 Executor 节点组成。 |
| Driver program  | 主运用程序，该进程运行应用的 main() 方法并且创建 SparkContext |
| Cluster manager | 集群资源管理器（例如，Standlone Manager，Mesos，YARN）       |
| Worker node     | 执行计算任务的工作节点                                       |
| Executor        | 位于工作节点上的应用进程，负责执行计算任务并且将输出数据保存到内存或者磁盘中 |
| Task            | 被发送到 Executor 中的工作单元                               |

{% asset_img spark-集群模式.png 集群模式 %}

**执行过程**：

1. 用户程序创建 SparkContext 后，它会连接到集群资源管理器，集群资源管理器会为用户程序分配计算资源，并启动 Executor；
2. Dirver 将计算程序划分为不同的执行阶段和多个 Task，之后将 Task 发送给 Executor；
3. Executor 负责执行 Task，并将执行状态汇报给 Driver，同时也会将当前节点资源的使用情况汇报给集群资源管理器。

# 核心组件

Spark 基于 Spark Core 扩展了四个核心组件，分别用于满足不同领域的计算需求。

{% asset_img spark-stack.png 核心组件 %}

## Spark SQL

Spark SQL 主要用于结构化数据的处理。其具有以下特点：

- 能够将 SQL 查询与 Spark 程序无缝混合，允许您使用 SQL 或 DataFrame API 对结构化数据进行查询；
- 支持多种数据源，包括 Hive，Avro，Parquet，ORC，JSON 和 JDBC；
- 支持 HiveQL 语法以及用户自定义函数 (UDF)，允许你访问现有的 Hive 仓库；
- 支持标准的 JDBC 和 ODBC 连接；
- 支持优化器，列式存储和代码生成等特性，以提高查询效率。

## Spark Streaming

Spark Streaming 主要用于快速构建可扩展，高吞吐量，高容错的流处理程序。支持从 HDFS，Flume，Kafka，Twitter 和 ZeroMQ 读取数据，并进行处理。

{% asset_img  spark-streaming-arch.png %}

Spark Streaming 的本质是微批处理，它将数据流进行极小粒度的拆分，拆分为多个批处理，从而达到接近于流处理的效果。

{% asset_img  spark-streaming-flow.png %}

## MLlib

MLlib 是 Spark 的机器学习库。其设计目标是使得机器学习变得简单且可扩展。它提供了以下工具：

- **常见的机器学习算法**：如分类，回归，聚类和协同过滤；
- **特征化**：特征提取，转换，降维和选择；
- **管道**：用于构建，评估和调整 ML 管道的工具；
- **持久性**：保存和加载算法，模型，管道数据；
- **实用工具**：线性代数，统计，数据处理等。

## Graphx

GraphX 是 Spark 中用于图形计算和图形并行计算的新组件。在高层次上，GraphX 通过引入一个新的图形抽象来扩展 RDD(一种具有附加到每个顶点和边缘的属性的定向多重图形)。为了支持图计算，GraphX 提供了一组基本运算符（如：subgraph，joinVertices 和 aggregateMessages）以及优化后的 Pregel API。此外，GraphX 还包括越来越多的图形算法和构建器，以简化图形分析任务。

# 弹性式数据集RDDs

`RDD` 全称为 Resilient Distributed Datasets，是 Spark 最基本的数据抽象，它是只读的、分区记录的集合，支持并行操作，可以由外部数据集或其他 RDD 转换而来，它具有以下特性：

- 一个 RDD 由一个或者多个分区（Partitions）组成。对于 RDD 来说，每个分区会被一个计算任务所处理，用户可以在创建 RDD 时指定其分区个数，如果没有指定，则默认采用程序所分配到的 CPU 的核心数；
- RDD 拥有一个用于计算分区的函数 compute；
- RDD 会保存彼此间的依赖关系，RDD 的每次转换都会生成一个新的依赖关系，这种 RDD 之间的依赖关系就像流水线一样。在部分分区数据丢失后，可以通过这种依赖关系重新计算丢失的分区数据，而不是对 RDD 的所有分区进行重新计算；
- Key-Value 型的 RDD 还拥有 Partitioner(分区器)，用于决定数据被存储在哪个分区中，目前 Spark 中支持 HashPartitioner(按照哈希分区) 和 RangeParationer(按照范围进行分区)；
- 一个优先位置列表 (可选)，用于存储每个分区的优先位置 (prefered location)。对于一个 HDFS 文件来说，这个列表保存的就是每个分区所在的块的位置，按照“移动数据不如移动计算“的理念，Spark 在进行任务调度的时候，会尽可能的将计算任务分配到其所要处理数据块的存储位置。

`RDD[T]` 抽象类的部分相关代码如下：

```scala
// 由子类实现以计算给定分区
def compute(split: Partition, context: TaskContext): Iterator[T]

// 获取所有分区
protected def getPartitions: Array[Partition]

// 获取所有依赖关系
protected def getDependencies: Seq[Dependency[_]] = deps

// 获取优先位置列表
protected def getPreferredLocations(split: Partition): Seq[String] = Nil

// 分区器 由子类重写以指定它们的分区方式
@transient val partitioner: Option[Partitioner] = None
```

# Spark SQL

## 简介

Spark SQL 是 Spark 中的一个子模块，主要用于操作结构化数据。它具有以下特点：

- 能够将 SQL 查询与 Spark 程序无缝混合，允许您使用 SQL 或 DataFrame API 对结构化数据进行查询；
- 支持多种开发语言；
- 支持多达上百种的外部数据源，包括 Hive，Avro，Parquet，ORC，JSON 和 JDBC 等；
- 支持 HiveQL 语法以及 Hive SerDes 和 UDF，允许你访问现有的 Hive 仓库；
- 支持标准的 JDBC 和 ODBC 连接；
- 支持优化器，列式存储和代码生成等特性；
- 支持扩展并能保证容错。

{% asset_img sql-hive-arch.png %}

## DataFrame & DataSet

### DataFrame

为了支持结构化数据的处理，Spark SQL 提供了新的数据结构 DataFrame。DataFrame 是一个由具名列组成的数据集。它在概念上等同于关系数据库中的表或 R/Python 语言中的 dataframe。 由于 Spark SQL 支持多种语言的开发，所以每种语言都定义了 DataFrame 的抽象，主要如下：

| 语言   | 主要抽象                                     |
| ------ | -------------------------------------------- |
| Scala  | Dataset[T] & DataFrame (Dataset[Row] 的别名) |
| Java   | Dataset[T]                                   |
| Python | DataFrame                                    |
| R      | DataFrame                                    |

### DataFrame 对比 RDDs

DataFrame 和 RDDs 最主要的区别在于一个面向的是结构化数据，一个面向的是非结构化数据，它们内部的数据结构如下：

{% asset_img spark-dataFrame+RDDs.png %}

DataFrame 内部的有明确 Scheme 结构，即列名、列字段类型都是已知的，这带来的好处是可以减少数据读取以及更好地优化执行计划，从而保证查询效率。

**DataFrame 和 RDDs 应该如何选择？**

- 如果你想使用函数式编程而不是 DataFrame API，则使用 RDDs；
- 如果你的数据是非结构化的 (比如流媒体或者字符流)，则使用 RDDs，
- 如果你的数据是结构化的 (如 RDBMS 中的数据) 或者半结构化的 (如日志)，出于性能上的考虑，应优先使用 DataFrame。

### DataSet

Dataset 也是分布式的数据集合，在 Spark 1.6 版本被引入，它集成了 RDD 和 DataFrame 的优点，具备强类型的特点，同时支持 Lambda 函数，但只能在 Scala 和 Java 语言中使用。在 Spark 2.0 后，为了方便开发者，Spark 将 DataFrame 和 Dataset 的 API 融合到一起，提供了结构化的 API(Structured API)，即用户可以通过一套标准的 API 就能完成对两者的操作。

### 静态类型与运行时类型安全

静态类型 (Static-typing) 与运行时类型安全 (runtime type-safety) 主要表现是：在实际使用中，如果你用的是 Spark SQL 的查询语句，则直到运行时你才会发现有语法错误，而如果你用的是 DataFrame 和 Dataset，则在编译时就可以发现错误 (这节省了开发时间和整体代价)。DataFrame 和 Dataset 主要区别在于：

在 DataFrame 中，当你调用了 API 之外的函数，编译器就会报错，但如果你使用了一个不存在的字段名字，编译器依然无法发现。而 Dataset 的 API 都是用 Lambda 函数和 JVM 类型对象表示的，所有不匹配的类型参数在编译时就会被发现。

以上这些最终都被解释成关于类型安全图谱，对应开发中的语法和分析错误。在图谱中，Dataset 最严格，但对于开发者来说效率最高。

{% asset_img spark-运行安全.png %}

### DataFrame & DataSet & RDDs 总结

这里对三者做一下简单的总结：

- RDDs 适合非结构化数据的处理，而 DataFrame & DataSet 更适合结构化数据和半结构化的处理；
- DataFrame & DataSet 可以通过统一的 Structured API 进行访问，而 RDDs 则更适合函数式编程的场景；
- 相比于 DataFrame 而言，DataSet 是强类型的 (Typed)，有着更为严格的静态类型检查；
- DataSets、DataFrames、SQL 的底层都依赖了 RDDs API，并对外提供结构化的访问接口。

{% asset_img spark-structure-api.png %}

### Spark SQL的运行原理

DataFrame、DataSet 和 Spark SQL 的实际执行流程都是相同的：

1. 进行 DataFrame/Dataset/SQL 编程；
2. 如果是有效的代码，即代码没有编译错误，Spark 会将其转换为一个逻辑计划；
3. Spark 将此逻辑计划转换为物理计划，同时进行代码优化；
4. Spark 然后在集群上执行这个物理计划 (基于 RDD 操作) 。

#### 逻辑计划(Logical Plan)

执行的第一个阶段是将用户代码转换成一个逻辑计划。它首先将用户代码转换成 `unresolved logical plan`(未解决的逻辑计划)，之所以这个计划是未解决的，是因为尽管您的代码在语法上是正确的，但是它引用的表或列可能不存在。 Spark 使用 `analyzer`(分析器) 基于 `catalog`(存储的所有表和 `DataFrames` 的信息) 进行解析。解析失败则拒绝执行，解析成功则将结果传给 `Catalyst` 优化器 (`Catalyst Optimizer`)，优化器是一组规则的集合，用于优化逻辑计划，通过谓词下推等方式进行优化，最终输出优化后的逻辑执行计划。

{% asset_img spark-Logical-Planning.png %}

#### 物理计划(Physical Plan) 

得到优化后的逻辑计划后，Spark 就开始了物理计划过程。 它通过生成不同的物理执行策略，并通过成本模型来比较它们，从而选择一个最优的物理计划在集群上面执行的。物理规划的输出结果是一系列的 RDDs 和转换关系 (transformations)。

{% asset_img spark-Physical-Planning.png %}

#### 执行

在选择一个物理计划后，Spark 运行其 RDDs 代码，并在运行时执行进一步的优化，生成本地 Java 字节码，最后将运行结果返回给用户。 

### Structured API基本使用

#### 创建DataFrame和Dataset

##### 创建DataFrame

Spark 中所有功能的入口点是 `SparkSession`，可以使用 `SparkSession.builder()` 创建。创建后应用程序就可以从现有 RDD，Hive 表或 Spark 数据源创建 DataFrame。示例如下：

```scala
val spark = SparkSession.builder().appName("Spark-SQL").master("local[2]").getOrCreate()
val df = spark.read.json("/usr/file/json/emp.json")
df.show()

// 建议在进行 spark SQL 编程前导入下面的隐式转换，因为 DataFrames 和 dataSets 中很多操作都依赖了隐式转换
import spark.implicits._
```

可以使用 `spark-shell` 进行测试，需要注意的是 `spark-shell` 启动后会自动创建一个名为 `spark` 的 `SparkSession`，在命令行中可以直接引用即可：

{% asset_img spark-sql-shell.png %}

##### 创建Dataset

Spark 支持由内部数据集和外部数据集来创建 DataSet，其创建方式分别如下：

######  由外部数据集创建

```scala
// 1.需要导入隐式转换
import spark.implicits._

// 2.创建 case class,等价于 Java Bean
case class Emp(ename: String, comm: Double, deptno: Long, empno: Long, 
               hiredate: String, job: String, mgr: Long, sal: Double)

// 3.由外部数据集创建 Datasets
val ds = spark.read.json("/usr/file/emp.json").as[Emp]
ds.show()
```

###### 由内部数据集创建

```scala
// 1.需要导入隐式转换
import spark.implicits._

// 2.创建 case class,等价于 Java Bean
case class Emp(ename: String, comm: Double, deptno: Long, empno: Long, 
               hiredate: String, job: String, mgr: Long, sal: Double)

// 3.由内部数据集创建 Datasets
val caseClassDS = Seq(Emp("ALLEN", 300.0, 30, 7499, "1981-02-20 00:00:00", "SALESMAN", 7698, 1600.0),
                      Emp("JONES", 300.0, 30, 7499, "1981-02-20 00:00:00", "SALESMAN", 7698, 1600.0))
                    .toDS()
caseClassDS.show()
```

##### 由RDD创建DataFrame

Spark 支持两种方式把 RDD 转换为 DataFrame，分别是使用反射推断和指定 Schema 转换：

###### 使用反射推断

```scala
// 1.导入隐式转换
import spark.implicits._

// 2.创建部门类
case class Dept(deptno: Long, dname: String, loc: String)

// 3.创建 RDD 并转换为 dataSet
val rddToDS = spark.sparkContext
  .textFile("/usr/file/dept.txt")
  .map(_.split("\t"))
  .map(line => Dept(line(0).trim.toLong, line(1), line(2)))
  .toDS()  // 如果调用 toDF() 则转换为 dataFrame 
```

###### 以编程方式指定Schema

```scala
import org.apache.spark.sql.Row
import org.apache.spark.sql.types._


// 1.定义每个列的列类型
val fields = Array(StructField("deptno", LongType, nullable = true),
                   StructField("dname", StringType, nullable = true),
                   StructField("loc", StringType, nullable = true))

// 2.创建 schema
val schema = StructType(fields)

// 3.创建 RDD
val deptRDD = spark.sparkContext.textFile("/usr/file/dept.txt")
val rowRDD = deptRDD.map(_.split("\t")).map(line => Row(line(0).toLong, line(1), line(2)))


// 4.将 RDD 转换为 dataFrame
val deptDF = spark.createDataFrame(rowRDD, schema)
deptDF.show()
```

##### DataFrames与Datasets互相转换

Spark 提供了非常简单的转换方法用于 DataFrame 与 Dataset 间的互相转换，示例如下：

```shell
# DataFrames转Datasets
scala> df.as[Emp]
res1: org.apache.spark.sql.Dataset[Emp] = [COMM: double, DEPTNO: bigint ... 6 more fields]

# Datasets转DataFrames
scala> ds.toDF()
res2: org.apache.spark.sql.DataFrame = [COMM: double, DEPTNO: bigint ... 6 more fields]
```

## Columns列操作

### 引用列

Spark 支持多种方法来构造和引用列，最简单的是使用 `col() ` 或 `column() ` 函数。

```scala
col("colName")
column("colName")

// 对于 Scala 语言而言，还可以使用$"myColumn"和'myColumn 这两种语法糖进行引用。
df.select($"ename", $"job").show()
df.select('ename, 'job).show()
```

### 新增列

```scala
// 基于已有列值新增列
df.withColumn("upSal",$"sal"+1000)
// 基于固定值新增列
df.withColumn("intCol",lit(1000))
```

### 删除列

```scala
// 支持删除多个列
df.drop("comm","job").show()
```

### 重命名列

```scala
df.withColumnRenamed("comm", "common").show()
```

需要说明的是新增，删除，重命名列都会产生新的 DataFrame，原来的 DataFrame 不会被改变。

## 使用Structured API进行基本查询

```scala
// 1.查询员工姓名及工作
df.select($"ename", $"job").show()

// 2.filter 查询工资大于 2000 的员工信息
df.filter($"sal" > 2000).show()

// 3.orderBy 按照部门编号降序，工资升序进行查询
df.orderBy(desc("deptno"), asc("sal")).show()

// 4.limit 查询工资最高的 3 名员工的信息
df.orderBy(desc("sal")).limit(3).show()

// 5.distinct 查询所有部门编号
df.select("deptno").distinct().show()

// 6.groupBy 分组统计部门人数
df.groupBy("deptno").count().show()
```

## 使用Spark SQL进行基本查询

### Spark  SQL基本使用

```scala
// 1.首先需要将 DataFrame 注册为临时视图
df.createOrReplaceTempView("emp")

// 2.查询员工姓名及工作
spark.sql("SELECT ename,job FROM emp").show()

// 3.查询工资大于 2000 的员工信息
spark.sql("SELECT * FROM emp where sal > 2000").show()

// 4.orderBy 按照部门编号降序，工资升序进行查询
spark.sql("SELECT * FROM emp ORDER BY deptno DESC,sal ASC").show()

// 5.limit  查询工资最高的 3 名员工的信息
spark.sql("SELECT * FROM emp ORDER BY sal DESC LIMIT 3").show()

// 6.distinct 查询所有部门编号
spark.sql("SELECT DISTINCT(deptno) FROM emp").show()

// 7.分组统计部门人数
spark.sql("SELECT deptno,count(ename) FROM emp group by deptno").show()
```

### 全局临时视图

上面使用 `createOrReplaceTempView` 创建的是会话临时视图，它的生命周期仅限于会话范围，会随会话的结束而结束。

你也可以使用 `createGlobalTempView` 创建全局临时视图，全局临时视图可以在所有会话之间共享，并直到整个 Spark 应用程序终止后才会消失。全局临时视图被定义在内置的 `global_temp` 数据库下，需要使用限定名称进行引用，如 `SELECT * FROM global_temp.view1`。

```scala
// 注册为全局临时视图
df.createGlobalTempView("gemp")

// 使用限定名称进行引用
spark.sql("SELECT ename,job FROM global_temp.gemp").show()
```

# 参考文献

https://spark.apache.org/docs/latest/index.html

https://spark.apache.org/docs/latest/sql-programming-guide.html

