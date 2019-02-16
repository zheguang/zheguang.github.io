---
layout: post
title: "Connect Spark to Postgres"
categories: [blog, systems]
tags: [spark, postgres]
---

In this post I will show an example of connecting Spark to Postgres, and pushing SparkSQL queries to run in the Postgres.

First, install and start the Postgres server, e.g. on the `localhost` and port `7433`.  Create a database `test_db` and a table `person`:
```repl
test_db=# create table person(name text, age int);
test_db=# insert into person values('shu', 21);
test_db=# insert into person values('zhe', 20);
test_db=# select * from person;
name | age
-----+-----
shu  | 21
zhe  | 20
(2 rows)
```

Download the PostgreSQL JDBC Driver jar.

Run the Spark Shell:
```bash
./bin/spark-shell
```

Within the Spark Shell, add the Postgresql JDBC driver jar to the classpath:
```repl
scala> :require <path_to_postgresql-42.0.0.jar>
```

Check if the JDBC driver is on the classpath:
```scala
scala> assert(Class.forName("org.postgresql.Driver") != null)
```

Create a simple query, push it down to the Postgres to execute, and display its result in Spark:
```scala
scala> val query = "select * from person"

val url = "jdbc:postgresql://localhost:7433/test_db"
import java.util.Properties
val connectionProperties = new Properties()
connectionProperties.setProperty("Driver", "org.postgresql.Driver")

val df = spark.read.jdbc(url, query, connectionProperties)

scala> df.show()
+----+---+
|name|age|
+----+---+
| shu| 21|
| zhe| 20|
+----+---+
```
