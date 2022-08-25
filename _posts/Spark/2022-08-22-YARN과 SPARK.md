---
title: "YARN과 SPARK"
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

date: 2022-08-22
last_modified_at: 2022-08-22
---

<br><br>

하둡은 매우 인기 있고 일반적인 스파크 배포 플랫폼이다. 일부 전문가들은 스파크가 하둡 응용 프로그램의 주요 처리 플랫폼인 맵리듀스를 곧 대체할 것이라고 믿는다. YARN의 스파크 응용 프로그램은 동일한 런타임 아키텍처를 공유하지만, 구현에는 약간의 차이가 있다.

<br><br>

# 1. 클러스터 매니저로서의 리소스 매니저

<br>

독립실행형 스케줄러와 달리, YARN 클러스터의 클러스터 매니저는 YARN 리소스 매니저 라고 한다. 리소스 매니저는 클러스터의 모든 노드에서 리소스 사용 및 가용성을 모니터링한다. 클라이언트는 스파크 응용 프로그램을 YARN 리소스 매니저에 제출하고, 리소스 매니저는 애플리케이션 마스터라는 특수 컨테이너인 응용 프로그램의 첫 번째 컨테이너를 할당한다.

<br><br>

# 2. 스파크 마스터로서의 애플리케이션 마스터

<br>

애플리케이션 마스터는 스파크 마스터 프로세스다. 마스터 프로세스가 다른 클러스터 배포에서 수행하는 것처럼, 애플리케이션 마스터는 응용 프로그램 드라이버와 클러스터 매니저(이 경우에는 리소스 매니저) 사이에서 리소스를 조정한다. 그런 다음 이러한 리소스(컨테이너)를 드라이버에서 사용할 수 있게 만든다. 이는 응용 프로그램에 대한 작업을 실행하고 데이터를 저장하기 위한 실행자로 사용할 수 있게 한다. 애플리케이션 마스터는 응용 프로그램의 수명 동안 유지된다.

<br><br>

# 3. YARN 에서 스파크 배포

<br>

1. 클라이언트 모드

클라이언트 모드에서 드라이버 프로세스는 응용 프로그램을 제출하는 클라이언트에서 실행된다. 기본적으로 관리되지 않으므로 드라이버 호스트가 실패하면 그 응용 프로그램도 실패한다. 클라이언트 모드는 대화식 셸 세션(pyspark, spark-shell 등)과 비대화식 응용 프로그램 제출 모두에 지원된다.

```sh
# 클라이언트 배포 모드를 사용해 pyspark 세션을 시작하는 방법

$SPARK_HOME/bin/pyspakr \
--master yarn-client \
--num-executors 1 \
--drvier-memory 512m \
--executor-memory 512m \
--executor-cores 1

# OR

$SPARK_HOME/bin/pyspark \
--master yarn \
--deploy-mode client \
--num-executors 1 \
--driver-memory 512m \
--executor-memory 512m \
--executor-cores 1
```

<br>

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/185921241-922ba540-9521-4be6-b7b9-f62431158899.png' width='70%'/>
</div>

<br>

1. 클라이언트는 스파크 응용 프로그램을 클러스터 매니저(YARN 리소스 매니저)에 제출한다. 드라이버 프로세스, 스파크세션 및 스파크 콘텍스트가 만들어지고, 클라이언트에서 실행된다.
2. 리소스 매니저 응용 프로그램에 대해 애플리케이션 마스터(스파크 마스터)를 할당한다.
3. 애플리케이션 마스터는 리소스 매니저에게 실행자로 사용될 컨테이너를 요청한다. 할당된 컨테이너로 실행자가 생성된다.
4. 클라이언트에 속한 드라이버는 스파크 프로그램의 작업 및 단계 프로세스를 감시하기 위해 실행자와 통신한다. 드라이버는 진행률, 결과 및 상태를 클라이언트에 보고한다.

<br>

2. 클러스터 모드

<br>

클라이언트 배포 모드와 달리, YARN 클러스터 모드에서 실행되는 스파크 응용 프로그램과 드라이버는 애플리케이션 마스터의 하위 프로세스로 클러스터에서 실행된다.
