---
title: "boundary query"
toc: true
# header:
#   image: /assets/images/sqoop/sqoop_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/b/b4/Apache_Sqoop_logo.svg)"
layout: posts
categories:
  - Sqoop
tags:
  - Sqoop
toc: true
toc_sticky: true

date: 2022-11-23
last_modified_at: 2022-11-23
---

<br>

Sqoop 은 `--split-by col` 를 지정하게 되면 아래와 같은 sql을 만들어 범위를 지정한다.

```sql
SELECT min(col), max(col)
FROM TABLE
```

그러나 이 경우 column이 문자라면 min, max 구분이 어렵고 저렇게 SQL이 들어간다면 map 분배가 매우 힘들다.
날짜의 경우 string으로 저장이 되어 있다면 min, max를 따지기 어려운 경우를 예로 든다.

그럴 경우 `--boundary-query` 를 활용하여 min, max 구문을 바꿔준다면 map에 고르게 분배된다.

```sql
-- --boundary-query
SELECT 0, 9 FROM DUAL
```

```sql
-- --split-by
SELECT * FROM MOD(SUBSTR(COL,14,1),10)
```

해당 옵션들을 넣은 sqoop구문은 다음과 같이 생성한다.

```sh
sqoop \
--options-file 옵션 파일 경로 \
--as-orcfile \
--delete-target-dir \
--target-dir /HDFS 경로 \
--class-name 클래스 이름 \
--map-column-java col=type \
--map-column-hive "col=type" \
--query "SELECT * \
FROM USER.TABLE \
partition(PARTITION_NAME) \
WHERE DATE >= '20221101070000000000' AND \$CONDITIONS" \
--boundary-query "SELECT 0,9 FROM DUAL" \
--split-by "mod(substr(DATE,14,1),10)" \
--num-mappers 10
```

그러나 이 경우 map 에 적절히 데이터를 분배하더라도 Index를 탄다면 인덱스 조회 후 point 한 쪽의 파일을 접근해야하는 비용이 발생한다. 한번 수집할 데이터가 6~7억 건의 경우 인덱스를 타면 더 느려지는 경우로 보고 index 설정을 끄는 방법을 선택한다. 그러므로 Sqoop의 Index 꺼주는 기능을 활용한다.

`oraoop.import.hint` 라는 항목으로 `NO_INDEX(t)` 를 넣어준다.

```xml
<!--orapp-site.xml-->
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one
  or more contributor license agreements.  See the NOTICE file
  distributed with this work for additional information
  regarding copyright ownership.  The ASF licenses this file
  to you under the Apache License, Version 2.0 (the
  "License"); you may not use this file except in compliance
  with the License.  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing,
  software distributed under the License is distributed on an
  "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
  KIND, either express or implied.  See the License for the
  specific language governing permissions and limitations
  under the License.
-->

<!-- Put OraOop-specific properties in this file. -->

<configuration>

  <property>
    <name>oraoop.oracle.session.initialization.statements</name>
    <value>alter session disable parallel query;
           alter session set "_serial_direct_read"=true;
           alter session set tracefile_identifier=oraoop;
           --alter session set events '10046 trace name context forever, level 8';
    </value>
    <description>A semicolon-delimited list of Oracle statements that are executed, in order, to initialize each Oracle session.
                 Use {[property_name]|[default_value]} characters to refer to a Sqoop/Hadoop configuration property.
                 If the property does not exist, the specified default value will be used.
                 E.g. {oracle.sessionTimeZone|GMT} will equate to the value of the property named "oracle.sessionTimeZone" or
                 to "GMT" if this property has not been set.
    </description>
  </property>

  <property>
    <name>mapred.map.tasks.speculative.execution</name>
    <value>false</value>
    <description>Speculative execution is disabled to prevent redundant load on the Oracle database.
    </description>
  </property>

  <property>
    <name>oraoop.import.hint</name>
    <value>NO_INDEX(t)</value>
    <description>Hint to add to the SELECT statement for an IMPORT job.
                 The table will have an alias of t which can be used in the hint.
                 By default the NO_INDEX hint is applied to stop the use of an index.
                 To override this in oraoop-site.xml set the value to a blank string.
    </description>
  </property>

<!--
  <property>
    <name>oraoop.block.allocation</name>
    <value>ROUNDROBIN</value>
    <description>Supported values are: ROUNDROBIN or SEQUENTIAL or RANDOM.
                 Refer to the OraOop documentation for more details.
    </description>
  </property>
-->

<!--
  <property>
    <name>oraoop.import.omit.lobs.and.long</name>
    <value>false</value>
    <description>If true, OraOop will omit BLOB, CLOB, NCLOB and LONG columns during an Import.
    </description>
  </property>
-->

<!--
  <property>
    <name>oraoop.table.import.where.clause.location</name>
    <value>SUBSPLIT</value>
    <description>Supported values are: SUBSPLIT or SPLIT.
                 Refer to the OraOop documentation for more details.
    </description>
  </property>
-->

<!--
  <property>
    <name>oraoop.oracle.append.values.hint.usage</name>
    <value>AUTO</value>
    <description>Supported values are: AUTO or ON or OFF.
                 ON:
                     OraOop will use the APPEND_VALUES Oracle hint during a Sqoop export, when inserting
                     data into an Oracle table.
                 OFF:
                     OraOop will not use the APPEND_VALUES Oracle hint during a Sqoop export.
                 AUTO:
                     For OraOop 1.1, the AUTO setting will not use the APPEND_VALUES hint.
    </description>
  </property>
-->

</configuration>
```

이 후 `--direct` 옵션을 활용한다. 그리고 sqoop 옵션 파일에서 import 를 빼서 적용한다.


```sh
sqoop import \
-Doraoop.import.partitions=파티션 이름 \
-Doraoop.jdbc.url.verbatim=true \
--options-file 옵션 파일 경로 \
--direct \
--as-orcfile \
--delete-target-dir \
--target-dir /HDFS 디렉터리 경로 \
--class-name 클래스 이름 \
--map-column-java col=type \
--map-column-hive "col=type" \
--table 테이블 이름 \
--where "DATE >= '20221101070000000000'" \
--num-mappers 6
```