---
title: "[Trouble Shooting] ERROR yarn.Client Application diagnostics message User application exited with status 1"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Spark
tags:
  - Spark
  - Trouble Shooting
toc: true
toc_sticky: true

date: 2022-08-03
last_modified_at: 2022-08-03

---

# 에러 내용

<br>

```sh
> spark-submit --master yarn --deploy-mode cluster --num-executors 4 *.py

...

ERROR yarn.Client: Application diagnostics message: User application exited with status 1
```

<br><br>

# Solution

<br>

해당 에러가 발생했을 시는 파이썬 코드를 한 번 더 확인해본다.

zepplin 이나 pyspark 에서 단위 모듈을 한 번씩 돌려 보며 에러를 확인하는 방법을 취한다.

본인은 python 파일에서 개행 시 함수 내부에 `\` 붙여서 에러를 해결하였다.
<br><br>
