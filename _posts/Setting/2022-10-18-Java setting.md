---
title: "Java setting"
toc: true
# header:
  # image: /assets/images/nifi/nifi_logo.svg
  # caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Setting
tags:
  - Setting
  - Jenkins
toc: true
toc_sticky: true

date: 2022-10-18
last_modified_at: 2022-10-18

---

<br><br>

# 1. 자바 프로젝트 뼈대 생성

<br>

```
1. mkdir example && cd example
2. mkdir -p src/main/java
3. mkdir -p src/test/java
4. mkdir -p src/main/resources
5. mkdir -p target/classes
6. mkdir lib
```

<br><br>

`tree` 명령어로 프로젝트 구조를 조회할 수 있다.

<br><br>

# 2. 소스 코드 작성

<br>

```text
1. cd src/main/java
2. mkdir -p com/example/app && cd com/example/app
3. vi App.java
```

```java
package com.example.app;

public class App {
  public static void main(String[] args) {
    System.out.println("Hello World");
  } 
}
```

<br><br>

# 3. 컴파일

<br>

```bash
javac src/main/java/com/example/app/App.java -d target/classes/com/example/app/
```

```
.
├── lib
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── example
│   │   │           └── app
│   │   │               └── App.java
│   │   └── resources
│   └── test
│       └── java
└── target
    └── classes
        └── com
            └── example
                └── app
                    └── App.class
```

<br><br>

# 4. 실행

<br>

```
java -cp target/classes com.example.app.App
```


<br><br>

# 5. Import Class 작성

<br>

```
1. mkdir src/main/java/com/example/app/service && cd src/main/java/com/example/app/service
2. vi Service.java
```

```java
package com.example.app.service;

public class Service {
    public double compute(double a, double b) {
        return a + b;
    }
}
```

<br><br>

# 6. 컴파일

<br>

```bash
javac -d ./target/classes ./src/main/java/com/example/app/service/Service.java
```

```
.
├── lib
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── example
│   │   │           └── app
│   │   │               ├── App.java
│   │   │               └── service
│   │   │                   └── Service.java
│   │   └── resources
│   └── test
│       └── java
└── target
    └── classes
        └── com
            └── example
                └── app
                    ├── App.class
                    └── service
                        └── Service.class
```

CLASSPATH 등록

```
export CLASSPATH="[project 경로]/target/classes:$CLASSPATH"
```

<br><br>

# 7. App 에 Import

<br>


```java
package com.example.app;

import com.example.app.service.Service;

public class App {
  public static void main(String[] args) {
    if(args.length != 2) {
        System.out.println("두 개의 인수를 주세요!");
        System.exit(0);
      }
      Service service = new Service();
      double a = Double.parseDouble(args[0]);
      double b = Double.parseDouble(args[1]);
      double result = service.compute(a, b);
      System.out.println(String.format("%s 더하기 %s 는 %s 이다.", a, b, result));
  } 
}
```


```
javac -d ./target/classes/ ./src/main/java/com/example/app/App.java
```

<br><br>

# 8. App 실행

<br>

```bash
java com.example.app.App

# 두 개의 인수를 주세요!

java com.example.app.App 5 7

# 5.0 더하기 7.0 는 12.0 이다.
```


<br><br>

# 9. 필요한 jar 파일 import

<br>

파일을 다운 후 lib 폴더에 넣어준다.

```
mkdir ./lib
```


``` java
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
```

<br><br>


# 10. jar 파일 만들기

<br>

jar 파일로 만들어 앱을 실행시켜 보자. 경로를 target/classes 를 이동시켜 target 경로에 app.jar 파일을 만든다.

```
jar cfv ../app.jar .
```

* c : 자바 아카이브 파일을 생성
* f : 파일 이름을 정한다.
* v : 콘솔에 진행상황을 출력한다.

```
added manifest
```

<br><br>

# 11. jar 파일 실행하기

<br>

```
java -cp app.jar com.example.app.App 10 2
```

<br><br>

# 12. Manifest 추가 후 실행 

<br>

resources 폴더에 MANIFEST.TXT 파일을 만들고 추가

```
Main-Class: com.example.app.App
```

target 으로 이동해 앱 실행

```
java -jar app.jar 20 30
```

<br><br>