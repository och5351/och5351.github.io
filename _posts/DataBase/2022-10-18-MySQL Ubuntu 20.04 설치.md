---
title: "MySQL Ubuntu 20.04 설치"
toc: true
#header:
  #image: /assets/images/spark/spark_logo.svg
  #caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/f3/Apache_Spark_logo.svg)"
layout: posts
categories:
  - DataBase
tags:
  - DataBase
toc: true
toc_sticky: true

date: 2022-10-18
last_modified_at: 2022-10-18
---

<br>

# 1. apt update 

<br>

```bash
sudo apt-get update
sudo apt-get upgrade
```

<br><br>

# 2. MySQL 서버 설치

<br>

```bash
sudo apt-cache search mysql-server

sudo apt-get install mysql-server
```
  
<br><br>

# 3. 보안 설정

<br>

```bash
sudo mysql_secure_installation
```

1. [질문] 보안이 강한 패스워드를 생성하기 위한 플로그인을 활성화 하는지 
1. [답변] 설치자의 기호에 따라 답변(필자의 경우 Y)
2. [질문] 패스워드의 레벨을 지정 Low(0), Medium(1), Strong(2)
2. [답변] 설치자의 기호에 따라 답변(필자의 경우 0)
3. [입력] root 계정의 비밀번호 입력, 재확인 비밀번호 입력 

### 권한 문제 시
```
Failed! Error: SET PASSWORD has no significance for user 'root'@'localhost' as the authentication method used doesn't store authentication data in the MySQL server. Please consider using ALTER USER instead if you want to change authentication parameters.
```

해당 문제는 root 권한으로 mysql_secure_installation 을 실행시켰음에도 불구 하고 나는 에러 메시지이다.
아래 처럼 진행한다.

```bash
sudo mysql

mysql > ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by '새로운비밀번호';
```

3. [질문] 입력한 비밀번호로 진행할지
3. [답변] 설치자의 기호에 따라 답변 (필자의 경우 Y)
4. [질문] 익명의 사용자를 제거하는지
4. [답변] 설치자의 기호에 따라 답변 (필자의 경우 Y)

<br><br>

# 4. MySQL 재시작 및 접속

<br>

```bash
sudo /etc/init.d/mysql restart
sudo systemctl status mysql.service
sudo mysql -u root -p
```
