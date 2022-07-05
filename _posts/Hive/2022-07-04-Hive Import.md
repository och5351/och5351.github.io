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

하이브 쿼리의 결과를 새로운 테이블에 저장해두면 유용할 때가 있다. 이는 쿼리의 결과가 너무 커서 콘솔에서 보기 힘들거나 그 결과를 이용하여 추가적인 처리 작업이 필요한 경우에 적합하다.

새로운 테이블의 컬럼 정의는 SELECT 절의 결과로부터 유추할 수 있다. 다음 쿼리에서 대상 테이블은 col1과 col2로 명명된 두 개의 컬럼을 가지는데 각 컬럼의 자료형은 source 테이블과 동일하다.

```sql
CREATE TABLE target
AS
SELECT col1, col2
FROM source;
```

CTAS 연산은 원자적이기 때문에 SELECT 쿼리가 어떤 이유로 실패한다면 그 테이블은 생성되지 않는다.

<br><br>

# 테이블 변경하기

<br>

하이브는 읽기 시점 스키마를 적용하기 때문에 테이블을 생성한 후에도 테이블 정의를 변경할 수 있는 유연성이 있다. 하지만 새로운 구조를 반영하여 기존 데이터를 변경하는 책임은 사용자에게 있기 때문에 상당히 주의해야 한다.

```sql
ALTER TABLE source RENAME TO target;
```

ALTER TABLE 은 테이블의 메타데이터를 변경할 뿐만 아니라 새로운 이름을 반영하기 위해 기존 테이블 디렉터리를 변경한다. 이 예제에서 /user/hive/warehouse/source는 /user/hive/warehouse/target 으로 변경된다. 외부 테이블의 기존 디렉터리는 변경되지 않고 그 메타데이터만 갱신된다.

하이브는 컬럼에 대한 정의를 변경하고, 새로운 컬럼을 추가하고, 심지어는 테이블의 모든 기존 컬럼을 완전히 교체하는 것도 허용한다.

```sql
ALTER TABLE target ADD COLUMNS (col3 STRING);
```

새로운 컬럼인 col3는 기존 컬럼의 뒷부분에 추가된다. 데이터 파일은 갱신되지 않기 때문에 쿼리를 실행하면 col3의 모든 값에 대해 null을 반환할 것이다(물론 그 파일에 추가된 ㅍ필드가 존재하지 않는 경우에만 해당한다). 하이브는 기존 레코드의 갱신을 허락하지 않기 때문에 기존 파일을 다른 방식으로 갱신하도록 준비할 필요하기 있다. 이런 이유로 SELECT 구문을 사용하여 새로운 테이블을 생성할 때 새로운 컬럼을 정의하고 기존 레코드를 그대로 유지하는 방식이 주로 사용된다.

컬럼에 대한 이름이나 자료형과 같은 컬럼의 메타데이터를 변경하는 작업은 '기존 자료형을 새로운 자료형으로 해석할 수 있다'라고 가정하면 매우 단순해진다.

> http://bit.ly/data_def_lang

<br><br>

# 테이블 삭제하기

DROP TABLE 구문은 테이블의 데이터와 메타데이터를 모두 삭제한다. 외부 테이블은 메타데이터만 삭제된다. 데이터는 그대로 남는다.

테이블 정의를 그대로 유지하면서 테이블의 전체 데이터만 삭제하는 방법도 있다.

```sql
TRUNCATE TABLE my_table;
```

TRUNCATE TABLE 구문은 외부 테이블에는 적용되지 않는다. 대신 하이브 쉘에서 dfs -rmr 명령을 실행하면 외부 테이블 디렉터리를 직접 제거할 수 있다.

비슷한 결과를 만드는 또 다른 방법은 LIKE 키워드를 사용하여 기존 테이블과 동일한 스키마를 가진 새로운 빈 테이블을 생성하는 것이다.

```sql
CREATE TABLE new_table LIKE existing_table;
```
