---
title: "Kafka Connect"
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

date: 2022-10-02
last_modified_at: 2022-10-02
---

<br><br>

카프카는 프로듀서와 컨슈머를 쉽게 접속할 수 있도록 'Connect API'를 제공한다. 각각 이 API를 이용하여 각종 외부 시스템과 접속한다. 또한 API를 기반으로 카프카에 접속하기 위한 프레임워크로 Kafka Connect 도 제공한다.

<br><br>

# Kafka Connect & Kafka Streams

<br>

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/193440916-d4dd8159-9722-467f-b167-85e47ff3ac22.png' width='70%' />
</div>

<br>

Kafka connect 와 접속하기 위한 플러그인으로 커넥터가 개발되어 있으며 데이터베이스, 키 밸류 스토어, 검색 인덱스, 파일시스템 등의 외부 시스템과 접속할 수 있다. 커넥터에는 몇 가지 종류가 있다. 현재 컨플루언트가 제공하는 커넥터로는 ActiveMQ, IBM MQ, JMS HDFS 나 JDBC 를 이용하영 접속하는 것, 기타 개발사에서 개발해 컨플루언트가 인정하고 있는 것, 커뮤니티에 의해 개발된 것 등이 있다. 시스템 간 또는 제품 간의 접속을 개별 개발자가 만드는 것은 비효율적이기 때문에 재사용성과 품질의 향상을 위해서도 이러한 커넥터들의 존재는 큰 도움이 된다.

또한 카프카에 존재하고 있는 데이터를 스트림 처리하는 Streams API 를 라이브러리화한 Kafka Streams 라는 클라이언트 라이브러리가 준비되어 있다. 사용자는 Kafka Stream 라이브러리를 이용해서 자바 어플리케이션을 만들고, 작동시킬 수 있기 때문에 카프카 입출력에 사용하는 스트림 처리 어플리케이션을 비교적 쉽게 구현할 수 있다.

<br><br>

# 소스 & 싱크

<br>

Kafka Connect 에서는 카프카에 데이터를 넣는 프로듀서 쪽 커넥터를 `소스` 라고 부르고, 카프카에서 데이터를 출력하는 컨슈머 쪽의 커넥터를 `싱크` 라고 한다.

<br><br>

# 환경 구성

<br>

카프카 클러스터와 카프카 커넥터가 필요다.

Kafka Connect는 카프카 클러스터의 브로커와 함께 설치해 실행한다.
