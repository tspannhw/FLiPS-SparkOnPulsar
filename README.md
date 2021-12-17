# FLiPS-SparkOnPulsar
SOP

### Run with Pyspark

````

bin/pyspark  --packages io.streamnative.connectors:pulsar-spark-connector_2.12:3.1.1.3

````

### Spark Shell (Python/Pyspark)

````
dfPulsar = spark \
  .readStream \
  .format("pulsar") \
  .option("service.url", "pulsar://localhost:6650") \
  .option("admin.url", "http://localhost:8080") \ 
  .option("topic", "persistent://public/default/chatresult2") \
  .option("subscribe", "persistent://public/default/chatresult2") \
  .load()
dfPulsar.printSchema()
pQuery = dfPulsar.selectExpr("CAST(__key AS STRING)", "CAST(value AS STRING)").as[(String, String)].writeStream.format("console").option("truncate", "false").start()
pQuery.explain()
pQuery.awaitTermination()
pQuery.stop()
````


### Run with Spark Shell (Scala)

````
bin/spark-shell  --packages io.streamnative.connectors:pulsar-spark-connector_2.12:3.1.1.3

# <or>

bin/spark-shell --master spark://pulsar1:7077 --packages io.streamnative.connectors:pulsar-spark-connector_2.12:3.1.1.3

````

### Spark Shell (Scala)

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


Python 3.6.9 (default, Dec  8 2021, 21:08:43) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
21/12/17 12:44:41 WARN Utils: Your hostname, pulsar1 resolves to a loopback address: 127.0.1.1; using 192.168.1.230 instead (on interface enp3s0f0)
21/12/17 12:44:41 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by org.apache.spark.unsafe.Platform (file:/opt/spark/jars/spark-unsafe_2.12-3.2.0.jar) to constructor java.nio.DirectByteBuffer(long,int)
WARNING: Please consider reporting this to the maintainers of org.apache.spark.unsafe.Platform
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
:: loading settings :: url = jar:file:/opt/spark/jars/ivy-2.5.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
Ivy Default Cache set to: /root/.ivy2/cache
The jars for the packages stored in: /root/.ivy2/jars
io.streamnative.connectors#pulsar-spark-connector_2.12 added as a dependency
:: resolving dependencies :: org.apache.spark#spark-submit-parent-749999d2-b1c9-4877-b1a0-a73316f6656b;1.0
        confs: [default]
        found io.streamnative.connectors#pulsar-spark-connector_2.12;3.1.1.3 in central
        found io.swagger#swagger-annotations;1.5.21 in central
        found org.slf4j#slf4j-api;1.7.25 in central
        found com.sun.activation#javax.activation;1.2.0 in central
        found org.slf4j#jul-to-slf4j;1.7.25 in central
        found org.checkerframework#checker-qual;2.0.0 in central
        found org.codehaus.mojo#animal-sniffer-annotations;1.14 in central
:: resolution report :: resolve 381ms :: artifacts dl 14ms
        :: modules in use:
        com.sun.activation#javax.activation;1.2.0 from central in [default]
        io.streamnative.connectors#pulsar-spark-connector_2.12;3.1.1.3 from central in [default]
        io.swagger#swagger-annotations;1.5.21 from central in [default]
        org.checkerframework#checker-qual;2.0.0 from central in [default]
        org.codehaus.mojo#animal-sniffer-annotations;1.14 from central in [default]
        org.slf4j#jul-to-slf4j;1.7.25 from central in [default]
        org.slf4j#slf4j-api;1.7.25 from central in [default]
        ---------------------------------------------------------------------
        |                  |            modules            ||   artifacts   |
        |       conf       | number| search|dwnlded|evicted|| number|dwnlded|
        ---------------------------------------------------------------------
        |      default     |   7   |   0   |   0   |   0   ||   7   |   0   |
        ---------------------------------------------------------------------
:: retrieving :: org.apache.spark#spark-submit-parent-749999d2-b1c9-4877-b1a0-a73316f6656b
        confs: [default]
        0 artifacts copied, 7 already retrieved (0kB/10ms)
21/12/17 12:44:42 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
/opt/spark/python/pyspark/context.py:238: FutureWarning: Python 3.6 support is deprecated in Spark 3.2.
  FutureWarning
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 3.2.0
      /_/

Using Python version 3.6.9 (default, Dec  8 2021 21:08:43)
Spark context Web UI available at http://pulsar1.fios-router.home:4040
Spark context available as 'sc' (master = local[*], app id = local-1639763084346).
SparkSession available as 'spark'.

oot@pulsar1:/opt/spark# ./runspark.sh 
21/12/17 15:16:05 WARN Utils: Your hostname, pulsar1 resolves to a loopback address: 127.0.1.1; using 192.168.1.230 instead (on interface enp3s0f0)
21/12/17 15:16:05 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another address
:: loading settings :: url = jar:file:/opt/spark/jars/ivy-2.5.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
Ivy Default Cache set to: /root/.ivy2/cache
The jars for the packages stored in: /root/.ivy2/jars
io.streamnative.connectors#pulsar-spark-connector_2.12 added as a dependency
:: resolving dependencies :: org.apache.spark#spark-submit-parent-81ae099f-7551-4ae1-ac0d-161ad0f1b71d;1.0
        confs: [default]
        found io.streamnative.connectors#pulsar-spark-connector_2.12;3.1.1.3 in central
        found io.swagger#swagger-annotations;1.5.21 in central
        found org.slf4j#slf4j-api;1.7.25 in central
        found com.sun.activation#javax.activation;1.2.0 in central
        found org.slf4j#jul-to-slf4j;1.7.25 in central
        found org.checkerframework#checker-qual;2.0.0 in central
        found org.codehaus.mojo#animal-sniffer-annotations;1.14 in central
:: resolution report :: resolve 340ms :: artifacts dl 14ms
        :: modules in use:
        com.sun.activation#javax.activation;1.2.0 from central in [default]
        io.streamnative.connectors#pulsar-spark-connector_2.12;3.1.1.3 from central in [default]
        io.swagger#swagger-annotations;1.5.21 from central in [default]
        org.checkerframework#checker-qual;2.0.0 from central in [default]
        org.codehaus.mojo#animal-sniffer-annotations;1.14 from central in [default]
        org.slf4j#jul-to-slf4j;1.7.25 from central in [default]
        org.slf4j#slf4j-api;1.7.25 from central in [default]
        ---------------------------------------------------------------------
        |                  |            modules            ||   artifacts   |
        |       conf       | number| search|dwnlded|evicted|| number|dwnlded|
        ---------------------------------------------------------------------
        |      default     |   7   |   0   |   0   |   0   ||   7   |   0   |
        ---------------------------------------------------------------------
:: retrieving :: org.apache.spark#spark-submit-parent-81ae099f-7551-4ae1-ac0d-161ad0f1b71d
        confs: [default]
        0 artifacts copied, 7 already retrieved (0kB/11ms)
21/12/17 15:16:07 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://pulsar1.fios-router.home:4040
Spark context available as 'sc' (master = spark://pulsar1:7077, app id = app-20211217151616-0000).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 3.2.0
      /_/
         
Using Scala version 2.12.15 (OpenJDK 64-Bit Server VM, Java 1.8.0_312)
Type in expressions to have them evaluated.
Type :help for more information.

scala> val dfPulsar = spark.readStream.format("pulsar").option("service.url", "pulsar://localhost:6650").option("admin.url", "http://localhost:8080").option("topic", "persistent://public/default/chatresult2").load()
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


scala> val pQuery = dfPulsar.selectExpr("CAST(__key AS STRING)", "CAST(value AS STRING)").as[(String, String)].writeStream.format("console").option("truncate", "false").start()

scala> 

-------------------------------------------                                     
Batch: 0
-------------------------------------------
+-----+-----+
|__key|value|
+-----+-----+
+-----+-----+

-------------------------------------------                                     
Batch: 1
-------------------------------------------
+-----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|__key|value                                                                                                                                                                                                                                           |
+-----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|null |{"id": "(9752,20,-1,-1)", "sentiment": "Positive", "userInfo": "StreamNative Blog User 5000", "comment": "\tApache Pulsar is a great option for many uses and includes pulsar sql and spark sql and flink", "contactInfo": "sparkdeveloper.com"}|
+-----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

-------------------------------------------
Batch: 2
-------------------------------------------
+-----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|__key|value                                                                                                                                                                                                                                           |
+-----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|null |{"id": "(9752,21,-1,-1)", "sentiment": "Positive", "userInfo": "StreamNative Blog User 5000", "comment": "\tApache Pulsar is a great option for many uses and includes pulsar sql and spark sql and flink", "contactInfo": "sparkdeveloper.com"}|
|null |{"id": "(9752,22,-1,-1)", "sentiment": "Positive", "userInfo": "StreamNative Blog User 5000", "comment": "\tApache Pulsar is a great option for many uses and includes pulsar sql and spark sql and flink", "contactInfo": "sparkdeveloper.com"}|
|null |{"id": "(9752,23,-1,-1)", "sentiment": "Positive", "userInfo": "StreamNative Blog User 5000", "comment": "\tApache Pulsar is a great option for many uses and includes pulsar sql and spark sql and flink", "contactInfo": "sparkdeveloper.com"}|
+-----+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

````

### My Spark Cluster

#### 1 server, 1 worker
````
sbin/start-.sh 
sbin/start-.sh --memory 2G spark://pulsar1:7077

````

### Reference

* https://github.com/yjshen/spark-connector-test
* https://streamnative.io/blog/tech/2019-07-16-one-storage-system-for-both-real-time-and-historical-data-analysis-pulsar-story/
* https://github.com/streamnative/pulsar-spark
* https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html
* https://spark.apache.org/docs/latest/spark-standalone.html
