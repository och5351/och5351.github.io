---
title: "Hive export to loacl"
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

date: 2022-07-21
last_modified_at: 2022-07-21
---

<br>

hive 에서 테이블 데이터를 local로 export 하고 싶을 때가 있다.

이 때는 아래 명령어를 활용한다.


<br>

```bash
hive --outputformat=csv2 -e "select * from ${schema}.${table}" >/etl/temp/test3.csv
```

<br>

--outputformat 옵션은 기본적으로 제공하는 포맷들이 csv 외에도 있다.

```
--outputformat=[table/vertical/csv2/tsv2/dsv/csv/tsv]  format mode for result display
    Note that csv, and tsv are deprecated - use csv2, tsv2 instead
```

<br>

해당 옵션은 table, vertical, csv2, tssv2, dsv 등을 제공한다. 





