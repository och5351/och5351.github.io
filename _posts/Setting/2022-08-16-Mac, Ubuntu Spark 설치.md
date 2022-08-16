---
title: "Mac, Ubuntu Spark 설치"
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

# 1. Java 설치

<br>

```sh
# ubuntu
sudo apt-get install openjdk-8-jdk-headless
```

```sh
brew cask install java
```

<br>

# 2. Spark 가져오기

<br>

```sh
wget https://d3kbcqa49mib13.cloudfront.net/spark-2.2.0-bin-hadoop2.7.tgz
```

<br><br>

# 3. Spark 릴리즈 풀기

<br>

```sh
tar -xzf spark-2.2.0-bin-hadoop2.7.tgz
sudo mv spark-2.2.0-bin-hadoop2.7 /opt/spark
```

<br><br>

# 4. 필요한 환경 변수 설정

<br>

```
export SPARK_HOME=/opt/spark
export PATH=$SPARK_HOME/bin:$PATH
```

<br><br>

# 5. 설치 테스트

<br>

```
spark-submit --class org.apache.spark.examples.SparkPi \
--master local \
$SPARK_HOME/examples/jars/spark-examples*.jar 1000
```

<br><br>

# 6. 스파크 설치 탐색

<br>

<table>
  <thead>
    <tr>
      <th colspan='1'>디렉토리</th>
      <th colspan='1'>설명</th>
    <tr>
  </thead>
  <tbody>
    <tr>
      <td>bin/</td>
      <td>pyspark, spark-shell, spark-sql 및 sparkR 과 같은 셸 프로그램을 통해 또는 spark-submit을 사용하는 일괄 처리 모드에서 대화식으로 스파크 응용 프로그램을 실행하는 모든 명령/스크립트를 포함한다.</td>
    </tr>
    <tr>
      <td>conf/</td>
      <td>스파크 구성 파일(spark-defaults.conf.template)을 설정할 대 사용할 수 있는 템플릿과 스파크 프로세스(spark-env.sh.template)에 필요한 환경 변수를 설정하는 데 사용되는 셸 스크립트가 들어 있다. 또한 로깅을 제어하는 구성 템플릿(log4j.properties.template), 메트릭 컬렉션(metrics.properties.template) 및 슬레이브 파일(slaves.template) 템플릿(독립실행형 모드로 실행되는 스파크 클러스터에 참여할 수 있는 슬레이브 노드를 제어한다)도 있다.</td>
    </tr>
    <tr>
      <td>data/</td>
      <td>스파크 프로젝트 내에서 mllib, graphx 및 스트리밍 라이브러리를 테스트하는 데 사용되는 샘플 데이터세트가 들어 있다.</td>
    </tr>
    <tr>
      <td>examples/</td>
      <td>스파크 릴리스와 함께 제공되는 모든 예제의 소스코드와 컴파일된 어셈블리(jar 파일)가 들어 있다. 샘플 프로그램은 자바, 파이썬, R 및 스칼라에 포함돼 있다. 포함된 예제의 최신 코드는 <a href='https://github.com/apache/spark/tree/master/examples'>https://github.com/apache/spark/tree/master/examples</a>에서 찾을 수 있다.</td>
    </tr>
    <tr>
      <td>jars/</td>
      <td>스파크의 주요 어셈블리 및 snapy, py4j, parquet 등 스파크에서 사용하는 서비스를 지원하는 어셈블리가 들어 있다. 디렉토리는 기본적으로 스파크의 CLASSPATH에 포함돼 있다.</td>
    </tr>
    <tr>
      <td>licenses/</td>
      <td>스칼라와 제이쿼리(JQuery)와 같이 따로 포함된 프로젝트를 다루는 라이선스 파일을 포함한다.이 파일은 법적 준수만을 목적으로 하며, 스파크를 실행하는 데는 필요하지 않다. </td>
    </tr>
    <tr>
      <td>python/</td>
      <td>pyspark를 실행하는 데 필요한 모든 파이썬 라이브러리를 포함한다. 일반적으로 이러한 파일에 직접 액세스할 필요는 없다.</td>
    </tr>
    <tr>
      <td>R/</td>
      <td>SparkR 패키지와 관련 라이브러리 및 문서가 들어 있다.</td>
    </tr>
    <tr>
      <td>sbin/</td>
      <td>로컬 또는 원격으로 독립실행혀 모드로 실행되는 스파크 클러스터의 마스터 및 슬레이블 서비스를 시작 및 중지하고, YARN 및 메소스와 관련된 프로세스를 시작하는 관리 스크립트가 포함돼 있다. 독립실행형 모드에서 다중 노드 클러스터를 배포할 때 다음 절에서 이러한 스크립트 중 일부를 사용한다.</td>
    </tr>
    <tr>
      <td>yarn/</td>
      <td>YARN에서 실행되는 스파크 응용 프로그램에 대한 지원 라이브러리가 들어 있다. 스파크가 YARN 클러스터의 프로세스 간에 데이터를 이동하는 데 사용하는 셔플 서비스가 여기에 포함된다.</td>
    </tr>
  </tbody>
</table>

<br><br>
