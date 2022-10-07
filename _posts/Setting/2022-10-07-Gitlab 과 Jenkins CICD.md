---
title: "Spark 독립실행형 클러스터 배포"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Spark
toc: true
toc_sticky: true

date: 2022-08-16
last_modified_at: 2022-08-16

---

<br>

# 1. 클러스터 토폴로지 계획 및 여러 시스템 스파크 설치

<br>

- 스파크 마스터
- 스파트 워커 1,2,3

<br><br>

# 2. 네트워크 구성

<br>

hosts 파일 활용

```
vi /etc/hosts

000.000.000 sparkworker1
...
```

이 후 ping 으로 확인

<br><br>

# 3. 각 호스트에 spark-default.conf 파일 생성/편집

<br>

스파크 마스터, 워커 호스트에서 다음 명령 실행

```
cd $SPARK_HOME/conf
sudo cp spark-defaults.conf.template spark-default.conf
sudo sed -i "\$aspark.master\tspark://sparkmaster:7077" spark-defaults.conf
```

<br><br>

# 4. 각 호스트에서 spark-env.sh 파일 생성/편집

<br>

```
cd $SPARK_HOME/conf
sudo cp spark-env.sh.template spark-env.conf
sudo sed -i "\$aSPARK_MASTER_IP=sparkmaster" spark-env.sh
```

<br><br>

# 5. 스파크 마스터/워커 시작하기

<br>

스파크 마스터 호스트에서 다음 명령 실행

```sh
sudo $SPARK_HOME/sbin/start-master.sh
```

스파크 워커에서 다음 명령 실행

```sh
sudo $SPARK_HOME/sbin/start-slave.sh spark://sparkmaster:7077
```

http://sparkslaveN:8081/ 에서 스파크 워커 UI를 확인한다.

<br><br>

# 6. 다중 노드 클러스터 테스트

<br>

Pi 추정 예제

```sh
spark-submit --class org.apache.spark.examples.SparkPi \
--master spark://sparkmaster:7077 \
--driver-memory 512m \
--executor-memory 512m \
--executor-cores 1\
$SPARK_HOME/examples/jars/spark-eamples*.jar 10000
```

<br><br>
