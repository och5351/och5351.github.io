---
title: "[Trouble Shooting] Java heap space"
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

date: 2022-07-07
last_modified_at: 2020-07-07
---

<br>

```bsh
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
```

<br>



해당 에러 메시지는 Sqoop-env.sh 에 대한 Gateway 고급 구성 스니펫(안전 밸브) 의 `sqoop-conf/sqoop-env.sh_client_config_safety_valve 항목 자바 힙 사이즈를 에서
변경할 수 있다. 그러나 기본 256m 정도 주고 있으며, 해당 에러가 날 시 임시적으로 힙 공간을 늘려서 잡아주는게 좋을 듯 하다.

<br><br>

```bsh
export HADOOP_CLIENT_OPTS="$HADOOP_CLIENT_OPTS -Xmx512m"
```