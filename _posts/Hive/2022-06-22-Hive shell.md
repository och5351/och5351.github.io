---
title: "Hive shell"
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

date: 2022-06-22
last_modified_at: 2022-06-22
---

<br>

하이브 쉘은 HiveQL 명령어로 하이브와 상호작용하는 하이브의 기본 도구다. HiveQL은 sql과 유사한 하이브의 질의 언어다. (MySQL에 영향을 받아 SQL문이 비슷하다.)

# 특징

* 명령어는 반드시 `;` 으로 끝나야 한다.
* SQL처럼 HiveQL은 일반적으로 대소문자를 구분하지 않는다.(문자열 비교 제외)
* 탭 키를 사용하면 하이브가 제공하는 예약어와 함수를 자동 완성할 수 있다. (처음 설치하고 이 명령어를 실행하면 사용자 머신에 메타스토어 데이터베이스를 만들기 때문에 수 초가 걸린다.)
* 데이터베이스는 사용자가 hive 명령어를 실행한 위치에 metastore_db라는 디렉터리를 만들어 필요한 파일을 저장
* 비대화식 모드로 실행할 수 있다. `-f` 옵션을 사용하면 지정한 파일에 대해서만 hive 명령어가 실행된다.
```bash
hive -f script.q
```
* 간단한 스크립트는 -e 옵션을 이용하여 명령행에 직접 입력하는 방법도 있다. 이 경우 스크립트 끝에 세미콜론을 붙이지 않아도 된다.
```bash
hive -e 'SELECT * FROM dummy'
```
* 실행 시 -S 옵션을 붙이면 불필요한 메시지의 출력을 막아 쿼리에 대한 출력 결과만 볼 수 있다.
```bash
hive -S -e 'SELECT * FROM dummy'
```
* 명령어 앞에 !를 붙여 호스트 운영체제의 명령어를 실행하는 기능이 있다.
* dfs 명령어로 하둡 파일시스템을 조작하는 기능이 있다.

<br><br>

# 예제

1. 하이브 테이블 생성
```SQL
CREATE TABLE records (
  year STRING,
  temperature INT,
  quality INT
)ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t';
```
HiveQL 에서만 사용되는 ROW FORMAT은 데이터 파일의 각 행은 탭으로 분리된 텍스트로 지정한다. 하이브는 탭과 개행 문자로 필드와 행을 각각 구분하고, 각 행에는 세 개의 필드(테이블의 컬럼에 상응하는)가 존재한다고 가정한다.

<br>

2. 지정된 로컬 파일을 하이브의 웨어하우스 디렉터리에 로드

``` SQL
LOAD DATA LOACL INPATH 'PATH'
OVERWRITE INTO TABLE records;
```
하이브는 특별한 파일 포맷을 요구하지 않기 때문에 파일을 파싱하고 그것을 내부 데이터베이스 포맷으로 저장하는 방식의 시도는 전혀 하지 않는다. 파일은 그대로 저장되고 하이브는 아무것도 변경하지 않는다.

로컬 파일시스템에 저장할 경우 hive.metastore.warehouse.dir 속성으로 설정하며, 기본값은 /user/hive/warehouse 하위 디렉터리로 저장된다. 따라서 records 테이블의 데이터 파일은 로컬 파일 시스템의 /user/hive/warehouse/records 디렉터리에서 찾을 수 있다.

``` bash
ls /user/hive/warehouse/records/
```

하이브는 특정 테이블을 질의할 때 모든 파일을 읽는다.

LOAD DATA 구문의 OVERWRITE 키워드는 해당 테이블의 디렉터리에 존재하는 모든 파일을 삭제하는 기능이다. OVERWRITE를 생략하면 새로운 파일은 단순히 그 테이블의 디렉터리에 추가된다. 이 때 동일한 이름의 파일이 있으면 이전 파일을 덮어쓴다.

3. 하이브 쿼리 실행

```SQL
SELECT year, MAX(temperature)
FROM records
WHERE temperature != 9999 AND quality IN (0, 1, 4, 5, 9)
GROUP BY year;
```

하이브가 이 쿼리를 사용자 대신 맵리듀스 잡으로 변환하여 실행하고 그 결과를 콘솔에 출력한다. 하이브가 지원하는 SQL 구조와 사용자가 쿼리를 통해 요청할 수 있는 데이터의 포맷과 같은 일부 제약사항을 보여주지만 원시 데이터를 대상으로 SQL 쿼리를 실행할 수 있게 해주는 능력이 있다.













