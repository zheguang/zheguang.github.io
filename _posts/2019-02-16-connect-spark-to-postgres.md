---
layout: post
title: "Connect Spark to Postgres"
categories: [blog, systems]
tags: [spark, postgres]
---

In this post I will show an example of connecting Spark to Postgres, and pushing SparkSQL queries to run in the Postgres.

## Set up Postgres

First, install and start the Postgres server, e.g. on the `localhost` and port `7433`.  Create a database `test_db` and two tables `person` and `class`:
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

test_db=# create table class(name text, class text);
test_db=# inert into class values ('shu', 'nursing'), 
('shu', 'coffee'), 
('zhe', 'maths'), 
('zhe', 'climbing');

test_db=# select *from class;
 name | class
------+----------
 shu  | nursing
 shu  | coffee
 zhe  | maths
 zhe  | climbing
(4 rows)
```

## Set up JDBC in Spark

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
scala> Class.forName("org.postgresql.Driver") != null
```

Add JDBC information to be used by Spark
```scala
scala> val url = "jdbc:postgresql://localhost:7433/test_db"
import java.util.Properties
val connectionProperties = new Properties()
connectionProperties.setProperty("Driver", "org.postgresql.Driver")
```

## Execute SQL query as text
Textual SQL queries are pushed all the way down through JDBC to database, and only the results are loaded back to Spark.

Create a simple query, push it down to the Postgres to execute, and display its result in Spark:
```scala
scala> val person = "(select * from person) as person"
```
Alias is necessary for it to work.

Create a dataframe to represent the data loading operation.  Note that this operation is not yet executed until an "action" occurs.
```scala
scala> val personDf = spark.read.jdbc(url, query, connectionProperties)
```

Here is one exmaple action: show the query result:
```scala
scala> personDf.show()
+----+---+
|name|age|
+----+---+
| shu| 21|
| zhe| 20|
+----+---+
```
This is when the query actually gets executed within the database and reults are loaded into Spark's dataframe.

We can inspect the query plan to see that it indeed consists of only one operator that is to scan the table through JDBC:
```scala
scala> personDf.explain
== Physical Plan ==
*(1) Scan JDBCRelation((select * from person) as person)
  [numPartitions=1] [name#43,age#44] PushedFilters: [], ReadSchema:
  struct<name:string,age:int>
```

Same thing happens for complicated queries such as:
```scala
scala> spark.read.jdbc(url, "(select avg(age) from person join class on person.name = class.name where class != 'maths') as nonmaths_age", connectionProperties).explain
== Physical Plan ==
*(1) Scan JDBCRelation((select avg(age) from person join class on
  person.name = class.name where class != 'maths') as nonmaths_age)
  [numPartitions=1] [avg#51] PushedFilters: [], ReadSchema:
  struct<avg:decimal(38,18)>
```

## Pushdown operators using Dataset / DataFrame

What happens if the computation is structured using Dataset or DataFrame?  It turns out some simple filters are pushed down to the database as well, but not more sophisticated ones.  Joins are not pushed down either.

Simple filters:
```scala
scala> personDf.where("age != 23").explain
== Physical Plan ==
*(1) Scan JDBCRelation((select * from person) as person)
  [numPartitions=1] [name#43,age#44] PushedFilters: [*IsNotNull(age),
  *Not(EqualTo(age,23))], ReadSchema: struct<name:string,age:int>
```

But not other filters such as those involved arithmetics or user-defined behaviors or functions:
```scala
scala> personDf.where("age + 1 != 23").explain
== Physical Plan ==
*(1) Filter NOT ((age#44 + 1) = 23)
+- *(1) Scan JDBCRelation((select * from person) as person)
      [numPartitions=1] [name#43,age#44] PushedFilters: [*IsNotNull(age)],
      ReadSchema: struct<name:string,age:int>
```

Moroever, joins are not pushed down at all:
```scala
scala> val classDf = spark.read.jdbc(url, "(select name, class from
  class) as class", connectionProperties)
scala> val joinDf = personDf.join(classDf, "name")
joinDf: org.apache.spark.sql.DataFrame = [name: string, age: int ... 1
  more field]

scala> joinDf.explain
== Physical Plan ==
*(5) Project [name#0, age#1, class#14]
+- *(5) SortMergeJoin [name#0], [name#13], Inner
   :- *(2) Sort [name#0 ASC NULLS FIRST], false, 0
   :  +- Exchange hashpartitioning(name#0, 200)
   :     +- *(1) Scan JDBCRelation((select * from person) as p) 
                [numPartitions=1] [name#0,age#1] PushedFilters: 
                [*IsNotNull(name)], ReadSchema: struct<name:string,age:int>
   +- *(4) Sort [name#13 ASC NULLS FIRST], false, 0
      +- Exchange hashpartitioning(name#13, 200)
         +- *(3) Scan JDBCRelation((select * from class) as c) 
                [numPartitions=1] [name#13,class#14] PushedFilters: 
                [*IsNotNull(name)], ReadSchema: 
                [struct<name:string,class:string>
```
As shown above, the two tables are loaded in full from Postgres, and the join is done within Spark using sort-merge join.
