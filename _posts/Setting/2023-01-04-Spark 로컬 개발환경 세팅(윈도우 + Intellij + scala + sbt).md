---
title: "Spark 로컬 개발환경 세팅(윈도우 + Intellij + scala + sbt)"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Zookeeper
toc_sticky: true
date: 2023-01-04
last_modified_at: 2023-01-04

---

<br><br>

# 1. IntelliJ 설치

<br>

<a href="https://www.jetbrains.com/ko-kr/idea/download/#section=windows">IntelliJ IDEA 설치 URL</a>

<br><br>

# 2. scala 플러그인 설치

<br><br>


# 3. 빌드 툴 SBT 설정

<br><br>

# 4. build.sbt 설정

<br>

```scala
name := "spark-test"

version := "1.0"

val sparkVersion = "2.4.8"

libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % sparkVersion,
  "org.apache.spark" %% "spark-sql" % sparkVersion
)
```

<br><br>

# 5. winutils.exe 다운

<br>

하둡 버전에 맞는 winutils.exe를 다운 받는다.

<a href="https://github.com/steveloughran/winutils/blob/master/hadoop-2.7.1/bin/winutils.exe">winutils.exe 다운 링크</a>

<br><br>

# 6. winutils.exe 경로 지정

<br>

```
C:\winutils\bin\winutils.exe
```

<br><br>

# 7. 샘플 코드

<br>

```scala
import org.apache.spark.sql.SparkSession

object HelloScala {
  def main(args: Array[String]){

    System.setProperty("hadoop.home.dir", "C:\\winutils")

    val spark = SparkSession.builder()
      .config("spark.driver.host", "localhost")
      .master("local[1]")
      .appName("Spark Test")
      .getOrCreate()

  }
}

```

<br><br>