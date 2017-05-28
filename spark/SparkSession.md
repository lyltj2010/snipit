## 新建SparkSession

一个Spark项目只能建立一个SparkSession，所以建立的需要`.getOrCreate()`，如果没有，新建;如果有，直接用。

```scala
import org.apache.spark.sql.SparkSession

object HelloWorld {
  val spark: SparkSession = SparkSession
    .builder()
    .appName("Spark Rocks")
    .master("local[*]")
    .enableHiveSupport()
    .getOrCreate()

  def main(args: Array[String]): Unit = {
    println("HelloWorld!")
  }
}
```
