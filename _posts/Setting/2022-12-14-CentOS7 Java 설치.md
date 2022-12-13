---
title: "CentOS7 Java 설치"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Java
toc_sticky: true
date: 2022-12-14
last_modified_at: 2022-12-14

---

<br><br>

# 1. 설치 된 자바 버전 확인

<br>

```bash
yum list installed | grep java
```

<br><br>

# 2. JDK 설치

<br>

```bash
yum install java-1.8.0-openjdk -y
```

<br><br>

# 3. 환경 변수 등록

<br>

```bash
readlink -f /usr/bin/java

# /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64/jre/bin/java

vi ~/.bashrc

export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.352.b08-2.el7_9.x86_64
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar

source ~/.bashrc
```

<br><br>

# 4. java version 확인

<br>

```bash
java -version
```

<br><br>

# 5. wget 설치

<br>

```bash
yum install wget -y
```

<br><br>