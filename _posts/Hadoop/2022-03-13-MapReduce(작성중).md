---
title: "MapReduce(작성중)"
header:
  image: /assets/images/hadoop/hadoop_logo.svg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
layout: posts
categories:
  - Hadoop
tags:
  - Hadoop
  - Cloudera
  - CDP
toc: true
toc_sticky: true

date: 2022-03-13
last_modified_at: 2022-07-12
---

<br><br>
<a id="home1"></a>

# 맵리듀스란?

<br>

데이터 처리를 위한 프로그래밍 모델. 단순하지만 유용한 프로그램을 작성할 수 있으며

<br><br>

# 맵과 리듀스

<br>

맵리듀스 작업은 크게 맵 단계와 리듀스 단계로 구분된다. 각 단계는 입력과 출력으로 키-값의 쌍을 가지며, 그 타입은 프로그래머가 선택한다. 또한 프로그래머는 맵 함수와 리듀스 함수를 작성해야 한다. <br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147897985-4ab97da6-a0ca-4772-906b-9fc3aae0bc20.png" width="70%" height="70%">
<p>맵 리듀스의 논리적 데이터 흐름</p>
</div>

<br>

<br>


# 맵리듀스 개발

맵리듀스 프로그램은 일련의 개발 절차를 따른다. 

1. 맵과 리듀스 함수를 구현하고 잘 동작하는지 확인하기 위한 단위 테스트를 작성
2. 잡을 실행하는 드라이버 프로그램을 작성
3. 정상적으로 작동하는지 검증하기 위해 데이터셋의 일부를 이용해서 IDE에서 실행(실패시 IDE 디버거를 통해 문제의 원인을 찾을 수 있다.)
4. 디버깅 정보를 활용해서 해당 문제를 검증하도록 단위 테스트를 확장하거나 매퍼와 리듀서가 입력을 정확히 처리하도록 개선

<br>

작은 데이터셋이 예상대로 동작 > 클러스터에서 본격적으로 실행

<br><br>

# 성능 개선

1. 맵리듀스 프로그램 표준검사
2. 태스크 프로파일링(성능 분석) - 분산 프로그램의 프로파일링은 쉬운 일이 아니므로 이를 지원하기 위해 하둡은 훅(hook, 가로채기)을 제공

<br><br>

# 환경 설정 API

하둡의 컴포넌트는 하둡 자체의 환경 설정 API를 이용하여 설정

`org.apache.hadoop.conf` 패키지에 있는 `Configuration` 클래스 인스턴스는 환경 설정 속성과 값의 집합이다.


```xml
<?xml version="1.0"?>
<configuration>
  <property>
    <name>color</name>
    <value>yellow</value>
    <description>Color</description>
  </property>
  <property>
    <name>size</name>
    <value>10</value>
    <description>Size</description>
  </property>
  <property>
    <name>weight</name>
    <value>heavy</value>
    <final>true</final>
    <description>Weight</description>
  </property>
  <property>
    <name>size-weight</name>
    <value>${size},${weight}</value>
    <description>Size and weight</description>
  </property>  
</configuration>
```

```java
Configuration conf = new Configuration();
conf.addResource("configuration-1.xml");
assertThat(conf.get("color"), is("yellow"));
assertThat(conf.getInt("size",0), is(10));
assertThat(conf.get("bredth", "wide")m is("wide"));
```


