# 使用SparkFE发行版

## 简介

SparkFE发行版是基于LLVM优化的高性能原生Spark版本，和标准Spark发行版一样提供Scala、Java、Python和R编程接口，用户使用SparkFE发行版方法与标准版一致。

## 下载SparkFE发行版

在Github的[Releases页面](https://github.com/4paradigm/SparkFE/releases)提供了SparkFE发行版的下载地址，用户可以直接下载到本地使用。

注意，预编译的SparkFE发行版为allinone版本，可以支持Linux和MacOS操作系统，如有特殊需求也可以下载源码重新编译SparkFE发行版。

## 使用Example Jars

下载解压后，设置`SPARK_HOME`环境变量，可以直接执行Example Jars中的例子。

```
export SPARK_HOME=`pwd`/spark-3.0.0-bin-sparkfe/

$SPARK_HOME/bin/spark-submit \
  --master local \
  --class org.apache.spark.examples.sql.SparkSQLExample \
  $SPARK_HOME/examples/jars/spark-examples*.jar
```

注意，SparkSQLExample为标准Spark源码自带的例子，部分SQL例子使用了SparkFE优化进行加速，部分DataFrame例子不支持SparkFE优化。

## 使用PySpark

下载SparkFE发行版后，也可以使用标准的PySpark编写应用，示例代码如下。

```
from pyspark.sql import SparkSession
from pyspark.sql import Row
from pyspark.sql.types import *
 
spark = SparkSession.builder.appName("demo").getOrCreate()
print(spark.version)

schema = StructType([
    StructField("name", StringType(), nullable=True),
    StructField("age", IntegerType(), nullable=True),
])

rows = [
    Row("Andy", 20),
    Row("Berta", 30),
    Row("Joe", 40)
]

spark.createDataFrame(spark.sparkContext.parallelize(rows), schema).createOrReplaceTempView("t1")
spark.sql("SELECT name, age + 1 FROM t1").show()

```

保存源码文件为`sparkfe_demo.py`后，使用下面命令提交本地运行。

```
${SPARK_HOME}/bin/spark-submit \
    --master=local \
    ./sparkfe_demo.py
```

观察Spark任务日志，底层生成了优化的SQL逻辑计划，并且根据不同体系架构进行JIT编译优化。

```
PROJECT(type=TableProject)
    DATA_PROVIDER(table=t1)
```