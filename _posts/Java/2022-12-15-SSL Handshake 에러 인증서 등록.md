---
title: "SSL Handshake 에러 인증서 등록"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Java
tags:
  - Java
toc: true
toc_sticky: true

date: 2022-12-15
last_modified_at: 2022-12-15

---

<br><br>

Maven 레포지토리에서 dependencies 를 가져올 때 아래와 같은 에러가 날 수 있다.

``` bash
download error: Caught javax.net.ssl.SSLHandshakeException (PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target) while downloading https://repo1.maven.org/maven2/org/scala-sbt/sbt/1.8.0/sbt-1.8.0.pom
```

해당 에러는 Repository가 https 로 넘어가면서 부터 인증서를 요구하기 때문이라고 한다.

그러므로 설치한 jdk에서 security 쪽 cacerts에 해당 repository 인증서를 등록해줘야 한다.

<br><br>

# 1. 인증서 다운

<br>

간단한 방법이다. 
해당 url을 타고 들어가 자물쇠 표시를 눌러 인증서를 다운받으면 된다. 
이 때 root 를 내려받아야 한다.

<img src="https://user-images.githubusercontent.com/45858414/207743936-5cc16ff9-6694-4e78-b62d-88d1ee97c29f.png" width="70%" />

<br>

<img src="https://user-images.githubusercontent.com/45858414/207743946-43eecdc9-cef2-40f2-bdd2-279abd8422e0.png" width="70%" />

<br>

<img src="https://user-images.githubusercontent.com/45858414/207743954-bbf92a11-a6db-4bb0-a870-b62e6b3c93d5.png" width="70%" />

<br><br>

# 2. 인증서 적용

<br>

```bash
keytool -importcert -keystore “%JAVA_HOME%\jre\lib\security\cacerts" -storepass changeit -trustcacerts -alias "CERT" -file “내보내기 한 인증서"

# 예제
# keytool -importcert -keystore "C:\Program Files (x86)\java\java-1.8.0-openjdk-1.8.0.332-1.b09.ojdkbuild.windows.x86_64\jre\lib\security\cacerts" -storepass changeit -trustcacerts -alias "CERT" -file "repo1.maven.org.crt"
```


<br><br>