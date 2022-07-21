---
title: "[Trouble Shooting] INFO fs.FSInputChecker: Found checksum error:"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Hadoop
tags:
  - Hadoop
  - Trouble Shooting
toc: true
toc_sticky: true

date: 2022-07-21
last_modified_at: 2022-07-21

---

# 에러 내용

<br>

```
INFO fs.FSInputChecker: Found checksum error:
```

<br><br>

# Solution

<br>

해당 에러는 HDFS put을 할 때 발생하는 체크섬 에러 이다.

<br>

로컬 디렉터리에서 숨김 파일중 crc 파일을 확인하여 삭제 후 put을 한다.