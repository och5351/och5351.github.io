---
title: "Spark from_json 함수"
toc: true
# header:
#   image: /assets/images/spark/spark_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/f3/Apache_Spark_logo.svg)"
layout: posts
categories:
  - Spark
tags:
  - Spark
toc: true
toc_sticky: true

date: 2023-04-10
last_modified_at: 2023-04-10
---

<br><br>

from_json() 은 spark sql 에서 지원하는 함수로 json을 받아왔을 때 각 속성들을 column화 시켜주는 함수이다. 아래는 <a href="https://sparkbyexamples.com/spark/spark-from_json-convert-json-column-to-struct-map-or-multiple-columns-2/">sparkbyexamples</a> 에서의 설명이다.


> In Spark/PySpark from_json() SQL function is used to convert JSON string from DataFrame column into struct column, Map type, and multiple columns.

<br><br>

# 예제

<br>


```python
# Schema 정의
schema = StructType([
    StructField("id1", StringType()),
    StructField("id2", StringType()),
    StructField("value1", StringType()),
    StructField("value2", StringType())
])

# json 데이터 파싱
# kafka 에서 오는 데이터는 value
df.selectExpr("CAST(value AS STRING)") \
    .select(from_json("value", schema).alias("data")) \
    .select("data.*") \
    .show()
```

<br><br>