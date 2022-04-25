---
title: "Sqoop 과 Hive"
toc: true
header:
  image: /assets/images/sqoop/sqoop_logo.svg
  caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/b/b4/Apache_Sqoop_logo.svg)"
layout: posts
categories:
  - Sqoop
tags:
  - [Sqoop, Hive]
toc: true
toc_sticky: true

date: 2022-04-25
last_modified_at: 2020-04-25
---

<br><br>
<a id="home1"></a>

데이터를 분석할 때 관계형 연산을 지원하는 하이브와 같은 시스템을 활용하면 분석 파이프라인의 개발이 쉬워진다. 관계형 데이터 저장소에서 데이터를 가져온 경우 하이브를 사용하면 편리하다.

## 목차

- [1. 로그 데이터 예 - Import](#1)
- [2. 로그 데이터 예 - Sqoop에서 Hive로 바로 import](#2)

<br>

<a id="1"></a>

## 1. 로그 데이터 예

웹 기반의 위젯 구매 시스템에서 들어오는 다른 로그 데이터가 시스템에 있다고 가정.

```text
1,15,120 Any St.,LosAngeles,CA,90210,2010-08-01
3,4,120 Any St.,Los Angeles,CA,90210,2010-08-01
2,5,400 Some PI.,Cupertino,CA,95014,2010-07-30
...
```

영업팀이 가장 집중해야 하는 지역을 알려주기 위해 우편번호를 기준으로 가장 매출이 높은 지역을 찾을 경우.
판매 로그와 widgets 테이블이 필요.

```SQL
-- 데이터 하이브로 로드
-- 테이블 생성
CREATE TABLE sales(
  widget_id INT,
  qty INT,
  street STRING,
  city STRING,
  state STRING,
  zip INT,
  sale_data STRING,
  ) ROW FORMAT DELIMITED FILEDS TERMINATED BY ',';

-- LOG 데이터 저장
LOAD DATA LOCAL TINPATH "sales.log" INTO TABLE sales;
```

스쿱은 기존 관계형 데이터 저장소의 테이블을 기반으로 하이브 테이블을 자동으로 생성할 수 있다.

```bash
# HDFS에 저장된 데이터를 하이브 테이블을 생성하고 로드
# 스쿱과 하이브의 기본 구분자는 서로 다르므로 구분자 지정 필수
sqoop create-hive-table\
--connect jdbc:mysql://localhost/hadoop \
--table widgets\
--fields-terminated-by ','
```

> 하이브에서 제공하는 자료형은 일반적인 SQL 시스템에 비해 매우 적다. SQL 타입과 완전히 대응되는 하이브 자료형은 많지 않고, import 를 위해 하이브 테이블을 정의할 때 Sqoop은 컬럼의 값을 저장할 수 있는 최선의 하이브 타입을 선택한다. 따라서 정확도는 다소 떨어질 수 있으며, 아래 로그는 해당 타입 문제 경고 이다.

```
yy/mm/dd h:m:s WARN hive.TableDefWriter:
Column 'column_name' had to be
cast to a less precise type in Hive
```


<br>

<div align="right">

[목차로](#home1)

</div><br><br>
<a id="2"></a>

## 2. 로그 데이터 예 - Sqoop에서 Hive로 바로 import
<br>

HDFS에 저장된 데이터를 Hive로 로드하는 세 단계의 작업을 DB에서 바로 Hive로 Import 한 번의 작업으로 단계를 줄일 수 있다. Import를 수행할 때 Sqoop은 Hive 테이블을 생성한 후 데이터를 로드한다. Import를 처음 수행하는 것이라면 MySQL의 테이블 스키마를 기반으로 Hive에 widgets 테이블을 생성하는 명령을 실행할 수 있다.

```bash
sqoop import \
--connect jdbc:mysql://localhost/hadoop \
--table widgtes \
-m 1 \
--hive-import # 하이브로 데이터를 바로 로드하고, 원천 데이터베이스에 있는 해당 테이블의 스키마를 기반으로 하이브 스키마를 자동으로 추론하는 명령어
```

어떤 데이터 import 방식을 선택했는지에 상관없이 우편번호를 기준으로 가장 매출이 높은 지역을 찾는 데 widgets 데이터셋과 sales 데이터셋을 함께 사용할 수 잇다.

```SQL
-- 나중을 위해 쿼리 결과 저장
CREATE TABLE zip_profits
AS
SELECT SUM(w.price * s.qty) AS sales_vol, S.ZIP FROM SALES s
JOIN widgets w ON (s.widget_id = w.id) GROUP BY s.zip;

...
Moving data to : hdfs://localhost/user/hive/warehouse/zip_profits
...
```



<br>

<div align="right">

[목차로](#home1)

</div><br><br>