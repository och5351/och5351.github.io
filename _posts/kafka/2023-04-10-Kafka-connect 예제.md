---
title: "Kafka-connect 예제"
# header:
#   image: /assets/images/hadoop/hadoop_logo.svg
#   caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
layout: posts
categories:
  - Kafka
tags:
  - Kafka
  - Ingetion
toc: true
toc_sticky: true

date: 2023-04-10
last_modified_at: 2023-04-10
---

<br><br>

카프카 커넥트를 등록하기 위한 예제를 작성한다.

<br><br>

# 1. Kafka connect config

<br>

```java
package org.dbsink.connect;

import org.apache.kafka.common.config.AbstractConfig;
import org.apache.kafka.common.config.ConfigDef;
import org.apache.kafka.common.config.ConfigDef.Importance;
import org.apache.kafka.common.config.ConfigDef.Type;

import java.util.Map;

public class DIPSinkDBInsertConnectorConfig extends AbstractConfig{

    private static final String CONFIG_CONNECTION_GROUP = "config group";
    public static final String CONFIG_NAME_SINK_TOPIC_NAME = "json 속성명";
    public static final String CONFIG_DOCUMENTATION_SINK_TOPIC_NAME = "해당 내용 설명";
    public static final String CONFIG_DISPLAY_SINK_TOPIC_NAME = "";

    public static ConfigDef config() {
        ConfigDef config = new ConfigDef();

        config.define(
                CONFIG_NAME_SINK_TOPIC_NAME,
                Type.STRING,
                ConfigDef.NO_DEFAULT_VALUE,
                Importance.HIGH,
                CONFIG_DOCUMENTATION_SINK_TOPIC_NAME,
                CONFIG_CONNECTION_GROUP,
                1,
                ConfigDef.Width.LONG,
                CONFIG_DISPLAY_SINK_TOPIC_NAME
        );

        return config;
    }

    public DIPSinkDBInsertConnectorConfig(Map<String, String> props) {
        super(config(), props);
    }

}
```

<br><br>

# 2. Kafka connect Connector

<br>

```java
package org.dbsink.connect;

import org.apache.kafka.common.config.ConfigDef;
import org.apache.kafka.common.config.ConfigException;
import org.apache.kafka.connect.connector.Task;
import org.apache.kafka.connect.errors.ConnectException;
import org.apache.kafka.connect.sink.SinkConnector;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class DIPSinkDBInsertConnector extends SinkConnector{

    private Map<String, String> configProperties;

    @Override
    public String version() {
        return "0.1";
    }

    @Override
    public void start(Map<String, String> props) {
        this.configProperties = props;
        try {
            new DIPSinkDBInsertConnectorConfig(props);
        }catch (ConfigException e){
            throw new ConnectException(e.getMessage(), e);
        }
    }

    @Override
    public Class<? extends Task> taskClass() {
        return DIPSinkDBInsertTask.class;
    }

    @Override
    public List<Map<String, String>> taskConfigs(int maxTasks) {
        List<Map<String, String>> taskConfigs = new ArrayList<>();
        Map<String, String> taskProps = new HashMap<>(configProperties);
        for (int i = 0; i < maxTasks; i++){
            taskConfigs.add(taskProps);
        }
        return taskConfigs;
    }

    @Override
    public ConfigDef config() {
        return DIPSinkDBInsertConnectorConfig.config();
    }

    @Override
    public void stop() {

    }

}
```

<br><br>

# 3. Kafka connect Task

<br>

```java
package org.dbsink.connect;

import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.StringSerializer;

import org.apache.kafka.connect.sink.SinkRecord;
import org.apache.kafka.connect.sink.SinkTask;

import java.util.*;
import org.json.JSONObject;


import org.dbsink.connect.util.Util;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

/**
 * DIP 이미지 Sink Task 클래스
 * @author chanhae oh
 */
public class DIPSinkDBInsertTask extends SinkTask{

    /**
     * kafka config 변수
     */
    Properties pub_config = new Properties();

    /**
     * kafka connect config 변수
     */
    public DIPSinkDBInsertConnectorConfig config;

    @Override
    public String version() {
        return "0.1.98";
    }


    @Override
    public void start(Map<String, String> props) {
        this.config = new DIPSinkDBInsertConnectorConfig(props);
        SINK_TOPIC_NAME = config.getString(DIPSinkDBInsertConnectorConfig.CONFIG_NAME_SINK_TOPIC_NAME);
    }

    @Override
    public void put(Collection<SinkRecord> records) {
        if (records.isEmpty()) {
            return;
        }

        for (SinkRecord record : records) {

            String keyData = record.key().toString();
            String value = record.value().toString();
        }       
    }

    @Override
    public void stop() {
    }
}
```

<br><br>

# 4. kafka connect script

<br>

```json
{
        "name":"sink name",
        "config":{
                "connector.class" : "java 패키지 경로",
                "topics" : "읽을 카프카 토픽",
                "topic.sink.name" : "카프카 config 에서 정의한 커스텀 속성",
                "key.converter" : "String",
                "value.converter" : "String"
        }

}
```

<br><br>

# 5. kafka connect plugin 저장소 업데이트

<br>

```bash
netstat -anlp | grep 8083
kill -9 pid
/kafka/bin/connect-distributed.sh -daemon /kafka/conf/connect-distributed.properties
```

<br><br>

# 6. Kafka connect 실행

<br>

```bash
curl -X POST -H "ContentType: application/json" localhost:8083/connectors -d @json
```

<br><br>

# 7. Kafka connect 상태 확인

<br>

```bash
curl -X GET localhost:8083/connectors | python -m json.tool
curl -X GET localhost:8083/connectors/"커넥터이름"/status | python -m json.tool
```