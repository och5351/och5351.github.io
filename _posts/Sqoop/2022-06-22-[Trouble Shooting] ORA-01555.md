---
title: "[Trouble Shooting] ORA-01555"
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

date: 2022-06-21
last_modified_at: 2020-06-21
---

<br>

Sqoop import 를 진행중 만난 에러 이다.

``` java
Error:java.io.IOException: SQLException in nextKeyValue
    ...
Caused by: java.sql.SQLException: ORA-01555: snapshot too old: rollback segment number 336 with name "_SYSSMU336_879580159$" too small
    at oracle.jdbc.driver.T4CTTIoer.processError(T4CTTIoer.java:447)
    at oracle.jdbc.driver.T4CTTIoer.processError(T4CTTIoer.java:396)
    at oracle.jdbc.driver.T4C8Oall.processError(T4C8Oall.java:951)
    at oracle.jdbc.driver.T4CTTIfun.receive(T4CTTIfun.java:513)
    at oracle.jdbc.driver.T4CTTIfun.doRPC(T4CTTIfun.java:227)
    at oracle.jdbc.driver.T4C8Oall.doOALL(T4C8Oall.java:531)
    at oracle.jdbc.driver.T4CPreparedStatement.doOall8(T4CPreparedStatement.java:208)
```



먼저 ORA-01555 에러코드에 대하여 알아보면 아래와 같다.

<br><br>

# ORA-01555

> ORA-01555: 너무 이전 스냅샷:롤백 세그먼트 %s 수에 "%s" 이름으로 된 것이 너무 작습니다
<br>ORA-01555: snapshot too old:rollback segment number %s with name "%s" too small


위와 같이 메시지가 출력되며 ORA-01555는 데이터 훼손이나 데이터 손실과는 아무 관련이 없다. 그런 점에서 안전한 오류이고, 단지 이 오류를 만나는 순간 쿼리를 계속 진행할 수 없다.

<br>

<b><u>ORA-01555 에러는 쿼리를 수행하는 동안 발생하는 다른 Transaction들에 의해 UNDO 세그먼트(롤백 세그먼트) 덮어씌워져서 발생한다.</u></b>

<br><br>


# 해결 방법

> <a href='https://stackoverflow.com/questions/32143202/sqoop-from-oracle-snapshot-too-old'>https://stackoverflow.com/questions/32143202/sqoop-from-oracle-snapshot-too-old</a>

해결 방법은 2가지 정도로 생각한다.

1. 위의 Transaction 이 일어날 시간을 피한다.
2. Map을 늘려서 속도를 올린다.

나는 --num-mappers=1 로 지정하여 실험적으로 당기고 있었기에 시간이 너무 올래 걸렸고, 그 시간 동안 테이블에 어떤 Transaction을 발생시키는 요인이 있었을 듯 하다.
그래서 map 갯수를 올리고 --split-by 옵션을 지정하여 새로 작업을 진행 시켰다. 그리고 Transaction이 일어나지 않는 시간에 실행을 시켰다.

Usage of --num-mappers=10 (i.e. increased parallelism) was sufficient enough to overcome the problem in this instance without impacting the source too much.

Additionally, adding the --direct parameter will cause Sqoop to use an Oracle specific connector which will speed things up further, and will be added to my solution as soon as I convince the DBA on that database to open up the necessary privileges. Direct also supports the option -Doraoop.import.consistent.read={true|false} which seems to mirror the Oracle export utility's CONSISTENT parameter in function (note, defaults to false), in the sense that the undo tablespace would not be used to try to preserve consistency, eliminating the need to race to import before the Undo tablespace fills up altogether.