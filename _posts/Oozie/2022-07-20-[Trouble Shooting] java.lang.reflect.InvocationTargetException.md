---
title: "[Trouble Shooting] java.lang.reflectInvocationTargetExceptiont"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Oozie
tags:
  - Oozie
  - Trouble Shooting
toc: true
toc_sticky: true

date: 2022-07-20
last_modified_at: 2022-07-20

---

# 에러 내용

<br>

```
java.lang.reflect.InvocationTargetException
```

<br><br>

# Solution

<br>

해당 에러 메시지는 확인이 더 필요하지만 Sqoop 구문 중 리눅스 기능인 `&&` 해당 기능 때문임으로 짐작한다.

<br>

`&&` 는 앞의 명령어가 실행되었을 때 성공한 경우에 다음 명령어를 실행한다.

그러나 sqoop을 실행시키기 위해 만들었던 shell 작업 중 하나가 문제가 있었다. 그래서 다음 구문이 실행되지 않고 중단되어 Oozie job 이 실패로 표시 된 듯 하다. 


