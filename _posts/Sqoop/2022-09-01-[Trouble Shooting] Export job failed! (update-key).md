---
title: "[Trouble Shooting] Export job failed! at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java at org.apache.sqoop.manager.OracleManager.upsertTable(OracleManager.java at org.apache.sqoop.tool.ExportTool.exportTable(ExportTool.java:87) ..."
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

date: 2022-09-01
last_modified_at: 2020-09-01
---

<br>

```bsh
Export job failed!
        at org.apache.sqoop.mapreduce.ExportJobBase.runExport(ExportJobBase.java:444)
        at org.apache.sqoop.manager.OracleManager.upsertTable(OracleManager.java:510)
        at org.apache.sqoop.tool.ExportTool.exportTable(ExportTool.java:87)
        at org.apache.sqoop.tool.ExportTool.run(ExportTool.java:113)
        at org.apache.sqoop.Sqoop.run(Sqoop.java:151)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.sqoop.Sqoop.runSqoop(Sqoop.java:187)
        at org.apache.sqoop.Sqoop.runTool(Sqoop.java:241)
        at org.apache.sqoop.Sqoop.runTool(Sqoop.java:250)
        at org.apache.sqoop.Sqoop.main(Sqoop.java:259)
```

<br>



해당 에러 메시지 발생 시 sqoop-export 중 --update-key 에 pk 를 확인해야 한다.
Target table에서의 모든 PK를 적어 주면 해결 된다.

<br><br>

```bash
# 만약 Target 테이블 PK가 pk1, pk2, pk3 일 때 
--update-key pk1,pk2,pk3
```