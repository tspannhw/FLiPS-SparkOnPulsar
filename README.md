# FLiPS-SparkOnPulsar
SOP


### run

````
bin/spark-shell  --packages io.streamnative.connectors:pulsar-spark-connector_2.12:3.1.1.3
````

### sql
````
val dfPulsar = spark.readStream.format("pulsar").option("service.url", "pulsar://localhost:6650").option("admin.url", "http://localhost:8080").option("topic", "persistent://public/default/chatresult2").load()
dfPulsar.printSchema()
val pQuery = dfPulsar.selectExpr("CAST(__key AS STRING)", "CAST(value AS STRING)").as[(String, String)].writeStream.format("console").option("truncate", "false").start()
pQuery.explain()
pQuery.awaitTermination()
pQuery.stop()


````

### output

````

Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 3.2.0
      /_/
         
Using Scala version 2.12.15 (OpenJDK 64-Bit Server VM, Java 11.0.11)
Type in expressions to have them evaluated.
Type :help for more information.

val dfPulsar = spark.readStream.format("pulsar").option("service.url", "pulsar://localhost:6650").option("admin.url", "http://localhost:8080").option("topic", "persistent://public/default/chatresult2").load()
dfPulsar: org.apache.spark.sql.DataFrame = [value: binary, __key: binary ... 5 more fields]

scala> dfPulsar.printSchema()
root
 |-- value: binary (nullable = false)
 |-- __key: binary (nullable = true)
 |-- __topic: string (nullable = true)
 |-- __messageId: binary (nullable = true)
 |-- __publishTime: timestamp (nullable = true)
 |-- __eventTime: timestamp (nullable = true)
 |-- __messageProperties: map (nullable = true)
 |    |-- key: string
 |    |-- value: string (valueContainsNull = true)


````

### Reference

* https://github.com/yjshen/spark-connector-test
* https://streamnative.io/blog/tech/2019-07-16-one-storage-system-for-both-real-time-and-historical-data-analysis-pulsar-story/
* https://github.com/streamnative/pulsar-spark
* https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html
