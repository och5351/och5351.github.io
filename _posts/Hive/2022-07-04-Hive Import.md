---
title: "Hive Import"
# header:
#   image: /assets/images/hadoop/hadoop_logo.svg
#   caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
layout: posts
categories:
  - Hive
tags:
  - Hadoop
  - Hive
toc: true
toc_sticky: true

date: 2022-07-04
last_modified_at: 2022-07-04
---

하이브 테이블에 데이터를 임포트하기 위해 LOAD DATA 조작을 사용하는 방법이 있다. 또한 INSERT 구문이나 테이블 생성 시점에 CTAS(CREATE TABLE ... AS SELECT) 구문을 사용하여 다른 테이블의 데이터를 불러와서 특정 테이블에 데이터를 추가할 수도 있다.

<br><br>

# INSERT

```sql
INSERT OVERWRITE TABLE target
SELECT col1, col2
FROM source;
```

<br>

파티션된 테이블은 PARTITION 절을 통해 데이터를 삽입할 파티션을 지정할 수 있다.

```SQL
INSERT OVERWRITE TABLE target
PARTITION (dt='yyyy-mm-dd')
SELECT col1, col2
FROM source;
```

<br>

OVERWRITE 키워드는 TARGET 테이블이나 파티션의 기존 내용이 SELECT 구문의 실행결과로 새로 갱신된다는 것을 의미한다. 기존 데이터가 있는 테이블이나 파티션에 레코드를 추가하려면 INSERT INTO TABLE을 사용하면 된다.

<br>

```SQL
INSERT OVERWRITE TABLE target
PARTITION (dt)
SELECT col1, col2, dt
FROM source;
```

`동적 파티션 삽입`

> 하이브는 0.14.0 부터는 INSERT INTO TABLE ... VALUES 구문으로 작은 레코드를 삽입할 수 있다.

<br><br>

# 다중테이블 INSERT

HiveQL에서 INSERT 구문은 from 절 앞 뒤 모두에 올 수 있다.

```SQL
FROM source
INSERT OVERWRITE TABLE target
SELECT col1, col2 
```

<br>

FROM 절을 앞에 사용한느 방식은 단일 쿼리에서 다수의 INSERT 절을 사용할 수 있기 때문에 매우 유용하다. 다중테이블 삽입은 INSERT 삽입 구문보다 훨씬 효율적인데, 다수의 분리된 출력을 생상할 때 입력 테이블을 한 번만 읽으면 되기 때문이다.

<br>

```sql
FROM records2
INSERT OVERWRITE TABLE stations_by_year
  SELECT year, COUNT(DISTINCT station)
  GROUP BY year
INSERT OVERWRITE TABLE records_by_year
  SELECT year, COUNT(1)
  GROUPT BY year
INSERT OVERWRITE TABLE good_records_by_year
  SELECT year, COUNT(1)
  WHERE temperature != 9999 AND quality IN (0, 1, 4, 5, 9)
  GROUP BY year;
```

여기서 원본 테이블(records2)은 한 개만 있지만, 세 개의 다른 쿼리의 결과를 담는 세 개의 테이블을 볼 수 있다.

<br><br>

# CREATE TABLE...AS SELECT

<br>

하이브 쿼리의 결과를 새로운 테이블에 저장해두면 유용할 때가 있다. 이는 쿼리의 결과가 너무 커서