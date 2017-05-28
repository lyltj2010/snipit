## 读取结构化数据

Spark可以从本地CSV，HDFS以及Hive读取结构化数据，直接解析为DataFrame，进行后续分析。


#### 读取本地CSV

需要指定一些选项，比如留header，比如指定delimiter值，用`，`或者`\t`或者其他。

```scala
import org.apache.spark.sql.{DataFrame, SparkSession}

object ReadCSV {
  val spark: SparkSession = SparkSession
    .builder()
    .appName("Spark Rocks")
    .master("local[*]")
    .getOrCreate()

  val path: String = "/path/to/file/data.csv"
  val df: DataFrame = spark.read
    .option("header","true")
    .option("inferSchema","true")
    .option("delimiter",",")
    .csv(path)
    .toDF()

  def main(args: Array[String]): Unit = {
    df.show()
    df.printSchema()
  }
}
```

#### 读取Hive数据

SparkSession可以直接调用`sql`方法，传入sql查询语句即可。返回的DataFrame可以做简单的变化，比如转换
数据类型，对重命名之类。

```scala
import org.apache.spark.sql.{DataFrame, SparkSession}
import org.apache.spark.sql.types.IntegerType

object ReadHive {
  val spark: SparkSession = SparkSession
    .builder()
    .appName("Spark Rocks")
    .master("local[*]")
    .enableHiveSupport() // 需要开启Hive支持
    .getOrCreate()
  import spark.implicits._ //隐式转换

  val sql: String = "SELECT col1, col2 FROM db.myTable LIMIT 1000"
  val df: DataFrame = spark.sql(sql)
    .withColumn("col1", $"col1".cast(IntegerType))
    .withColumnRenamed("col2","new_col2")

  def main(args: Array[String]): Unit = {
    df.show()
    df.printSchema()
  }
}
```

#### 读取HDFS数据

HDFS上没有数据无法获取表头，需要单独指定。可以参考databricks的[网页](https://github.com/databricks/spark-csv)。一般HDFS默认在9000端口访问。

```scala
import org.apache.spark.sql.{DataFrame, SparkSession}

object ReadHDFS {
  val spark: SparkSession = SparkSession
    .builder()
    .appName("Spark Rocks")
    .master("local[*]")
    .getOrCreate()

  val location: String = "hdfs://localhost:9000/user/zhangsan/test"
  val df: DataFrame = spark
    .read
    .format("com.databricks.spark.csv")
    .option("inferSchema","true")
    .option("delimiter","\001")
    .load(location)
    .toDF("col1","col2")

  def main(args: Array[String]): Unit = {
    df.show()
    df.printSchema()
  }
}
```
