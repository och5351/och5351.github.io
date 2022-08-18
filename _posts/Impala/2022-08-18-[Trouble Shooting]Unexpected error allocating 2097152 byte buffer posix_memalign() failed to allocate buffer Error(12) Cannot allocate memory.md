---
title: "[Trouble Shooting] Unexpected error allocating byte buffer: posix_memalign() failed to allocate buffer: Error(12): Cannot allocate memory"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Impala
tags:
  - Hadoop
  - Impala
  - Trouble Shooting
toc: true
toc_sticky: true

date: 2022-08-18
last_modified_at: 2022-08-18

---

# 에러 내용

<br>

```
Unexpected error allocating 2097152 byte buffer: posix_memalign() failed to allocate buffer: Error(12): Cannot allocate memory
```

<br><br>

# Solution

<br>

해당 에러는 초기 적재 시 너무 많은 데이터를 insert 할 때 발생하는 에러이다.
가지고 있는 메모리 보다 더 많은 어플리케이션을 동작시켜 발생하는 것을 의심된다.

<br>

해결 방법은 Row를 끊어서 적재를 한다. 메모리를 증설하기보다는 이런 방법을 통하여 나눠 적재하면 문제가 없다.