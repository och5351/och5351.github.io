---
title: "Spark, Scala Version 확인"
toc: true
# header:
#   image: /assets/images/spark/spark_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/f3/Apache_Spark_logo.svg)"
layout: posts
categories:
  - Spark
tags:
  - Spark
toc: true
toc_sticky: true

date: 2022-12-20
last_modified_at: 2022-12-20
---

<br><br>

Spark-shell 접속 후 아래와 같이 확인 가능하다.


```scala
println(sc.version)
// 2.4.7.7.1.7.1000-141

println(scala.util.Properties.versionString)
// version 2.11.12
```

<br><br>
