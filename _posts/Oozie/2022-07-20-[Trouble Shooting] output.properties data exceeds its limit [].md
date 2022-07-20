---
title: "[Trouble Shooting] output.properties data exceeds its limit"
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
output.properties data exceeds its limit
```

<br><br>

# Solution

<br>

해당 에러 메시지는 하기 URL 과 같이 조치 하였다.

> https://community.cloudera.com/t5/Support-Questions/Oozie-Shell-action-Output-data-exceeds-its-limit-2048/m-p/235817

<br>

그러나 1024*32 까지 줘도 동일한 문제가 발생하여 다른 부분에 문제가 없는지 확인하였다.


<br>

원인은 tee 구문이었다.

<img src='https://user-images.githubusercontent.com/45858414/179902737-e903c439-45c4-4e37-b35c-7af306491275.png' width ='70%' />


tee 명령어는, 명령어의 출력 결과를 파일과 화면에 동시에 출력할 수 있도록 해주는 명령어이며, 해당 Sqoop 작업의 출력들이 화면에 동시에 나오므로 나오는 에러였다.



