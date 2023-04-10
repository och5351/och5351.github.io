---
title: "Spark Streaming 예제"
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

Spark Streaming 예제이다. readStream 메소드를 활용해 Streaming을 구현한다.

<br><br>

# 예제

<br>


```python
# Schema 정의
schema = StructType([
    StructField("eqp_id", StringType()),
    StructField("single_no", StringType()),
    StructField("img_nm", StringType()),
    StructField("img_path", StringType()),
    StructField("upload_dttm", StringType())
])

# Kafka 데이터 수신을 위한 DataFrame 생성
kafka_df = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", BOOTSTRAP_SERVERS) \
    .option("subscribe", TOPIC_NAME) \
    .option("startingOffsets", "earliest") \
    .load()

# json 데이터 파싱
parsed_df = kafka_df.selectExpr("CAST(value AS STRING)") \
    .select(from_json("value", schema).alias("data")) \
    .select("data.*")
```

<br><br>