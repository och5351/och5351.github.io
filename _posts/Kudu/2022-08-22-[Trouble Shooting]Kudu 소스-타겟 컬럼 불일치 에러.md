---
title: "[Trouble Shooting] Not found in Kudu table 'impala::SCHEMA.TABLE': Not found: No tablet covering the requested range partition: NonCoveredRange { lower_bound: (<start>), upper_bound: (RANGE (PARTITION KEY): <redacted>), ttl: 193268ms } (1 of 24996626 similar)"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Kudu
tags:
  - Hadoop
  - Kudu
  - Trouble Shooting
toc: true
toc_sticky: true

date: 2022-08-22
last_modified_at: 2022-08-22

---

# 에러 내용

<br>

```
Not found in Kudu table 'impala::SCHEMA.TABLE': Not found: No tablet covering the requested range partition: NonCoveredRange { lower_bound: (<start>), upper_bound: (RANGE (PARTITION KEY): <redacted>), ttl: 193268ms } (1 of 24996626 similar)
```

<br><br>

# Solution

<br>

해당 에러는 Kudu 에 insert 할 때 소스 테이블과 타겟 테이블의 Column 순서가 일치 하지 않을 경우 발생한다.

<br>

해당 명령어로 아래 테이블 스키마를 확인한다.

```SQL
show create table 
```

스키마 확인 후 column 순서에 맞게 insert 구문을 지정하여 준다.

```SQL
 CREATE TABLE SCHEM.TARGET_TB (
	COL1	STRING	COMMENT	'NONE',
	COL2	STRING	COMMENT	'NONE',
	COL3	STRING	COMMENT	'NONE',
	COL4	STRING	COMMENT	'NONE',
	COL5	STRING	COMMENT	'NONE',
	COL6	STRING	COMMENT	'NONE',
	COL7	STRING	COMMENT	'NONE',
	COL8	STRING	COMMENT	'NONE',
	COL9	STRING	COMMENT	'NONE',
	COL10	STRING	COMMENT	'NONE'
	PRIMARY KEY (COL1, COL2, COL3, COL4, COL5, COL6)
)

```



```SQL
UPSERT INTO TABLE SCHEM.TARGET_TB
SELECT 
 	COL1	STRING	COMMENT	'NONE',
	COL2	STRING	COMMENT	'NONE',
	COL3	STRING	COMMENT	'NONE',
	COL4	STRING	COMMENT	'NONE',
	COL5	STRING	COMMENT	'NONE',
	COL6	STRING	COMMENT	'NONE',
	COL7	STRING	COMMENT	'NONE',
	COL8	STRING	COMMENT	'NONE',
	COL9	STRING	COMMENT	'NONE',
	COL10	STRING	COMMENT	'NONE'
FROM SCHEM.SOURCE_TB
```