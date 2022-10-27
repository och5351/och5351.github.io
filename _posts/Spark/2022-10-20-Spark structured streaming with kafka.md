---
title: "Spark structured streaming with kafka"
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

date: 2022-10-20
last_modified_at: 2022-10-21
---

<br><br>

# 1. 카프카 소스에서 메시지 읽기

<br>

카프카에서 메시지를 읽을 때 아래 옵션 중 하나만 지정한다.

* assign : 토픽뿐만 아니라 읽으려는 파티션까지 세밀하게 지정하는 옵션(JSON 문자열({"topicA":[0,1],"topicB":[2,4]}))
* subscribe : 토픽 목록 구독
* subscribePattern : 패턴 지정 여러 토픽 구독

<br>

추가적인 옵션

* startingOffsets 및 endingOffsets : 쿼리를 시작할 때 읽을 지점. 옵션 값은 가장 작은 오프셋부터 읽는 `earliest`나 가장 큰 오프셋부터 읽는 `latest` 중 하나를 지정. 또는 각 `TopicPartition`에 대한 시작 오프셋을 명시한 JSON 문자열을 사용해 지정. 이 때 JSON에 오프셋을 -2로 지정하면 earliest로, -1로 지정하면 latest로 동작. 예를 들어 {"topicA":{"0":23,"1":-1},"topicB":{"0":-2}}와 같은 JSON 명세를 사용할 수 있다. 이 설정은 새로운 스트리밍 쿼리가 시작될 때만 적용되며, 다시 시작한 쿼리에서 쿼리가 남긴 오프셋을 사용. 쿼리 실행중에 새롭게 발견한 파티션은 earliest 방식으로 읽는다.

* failOnDataLoss : 데이터 유실(예: 토픽이 삭제되거나 오프셋이 범위를 벗어났을 때)이 일어났을 때 쿼리를 중단할 것인지 지정. 잘못된 정보를 줄 수 있으므로 원하는 대로 동작하지 않는 경우 비활성화할 수 있다. 기본값은 true

* maxOffsetsPerTrigger : 특정 트리거 시점에 읽을 오프셋의 전체 개수


<br>

하기와 같이 kafka 의 토픽을 읽어 올 수 있으며, 여러 토픽들도 읽어 올 수 있다.
`readStream`의 경우 show로 바로 읽을 수 없으며, offset 지정이 불가하다.


### Creating a Kafka Source for Streaming Queries

```python
# Subscribe to 1 topic
df = spark \
  .readStream \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2") \
  .option("subscribe", "topic1") \
  .load()
df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")

# Subscribe to multiple topics
df = spark \
  .readStream \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2") \
  .option("subscribe", "topic1,topic2") \
  .load()
df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")

# Subscribe to a pattern
df = spark \
  .readStream \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2") \
  .option("subscribePattern", "topic.*") \
  .load()
df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")
```

<br>

### Creating a Kafka Source for Batch Queries

```python
# Subscribe to 1 topic defaults to the earliest and latest offsets
df = spark \
  .read \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2") \
  .option("subscribe", "topic1") \
  .load()
df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")

# Subscribe to multiple topics, specifying explicit Kafka offsets
df = spark \
  .read \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2") \
  .option("subscribe", "topic1,topic2") \
  .option("startingOffsets", """{"topic1":{"0":23,"1":-2},"topic2":{"0":-2}}""") \
  .option("endingOffsets", """{"topic1":{"0":50,"1":-1},"topic2":{"0":-1}}""") \
  .load()
df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")

# Subscribe to a pattern, at the earliest and latest offsets
df = spark \
  .read \
  .format("kafka") \
  .option("kafka.bootstrap.servers", "host1:port1,host2:port2") \
  .option("subscribePattern", "topic.*") \
  .option("startingOffsets", "earliest") \
  .option("endingOffsets", "latest") \
  .load()
df.selectExpr("CAST(key AS STRING)", "CAST(value AS STRING)")
```

<br><br>

# 2. 카프카 토픽 메타 데이터

<br>


<table>
    <thead>
        <tr>
            <th colspan="1">Column</th>
            <th colspan="1">Type</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>key</td>
            <td>binary</td>
        </tr>
         <tr>
            <td>value</td>
            <td>binary</td>
        </tr>
        <tr>
            <td>partition</td>
            <td>int</td>
        </tr>
        <tr>
            <td>offset</td>
            <td>long</td>
        </tr>
        <tr>
            <td>timestamp</td>
            <td>long</td>
        </tr>
        <tr>
            <td>timestampType</td>
            <td>int</td>
        </tr>
    </tbody>
  <table>