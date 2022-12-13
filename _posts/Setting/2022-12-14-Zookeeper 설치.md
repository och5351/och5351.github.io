---
title: "Zookeeper 설치"
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
date: 2022-12-14
last_modified_at: 2022-12-14

---

<br><br>

# 1. 계정 생성

<br>

```bash
useradd zookeeper
passwd zookeeper
```

<br><br>

# 2. hosts 등록

<br>

```bash
vi /etc/hosts

<IP>  <alias>
...
```


# 3. 계정 변경

<br>

```bash
su - zookeeper
```

<br><br>

# 4. Zookeeper 다운로드

<br>

```bash
wget "http://apache.mirror.cdnetworks.com/zookeeper/zookeeper-3.5.10/apache-zookeeper-3.5.10-bin.tar.gz"
```

<br><br>

# 5. 압축해제

<br>

```bash
tar xvfzp apache-zookeeper-3.6.3-bin.tar.gz
```

<br><br>

# 6. link 설정

<br>

```bash
ln -s apache-zookeeper-3.6.3-bin zookeeper
```

<br><br>



<br><br>

# 7. 설정 파일 변경(데이터 디렉터리 경로와 서버 설정)

<br>

```bash
cd zookeeper/conf
cp zoo_sample.cfg zoo.cfg
vi zoo.cfg

tickTime=2000
initLimit=10
syncLimit=5
dataDir=/home/zookeeper/zookeeper/data
clientPort=2181
server.1=zoo-1:2888:3888
server.2=zoo-2:2888:3888
server.3=zoo-3:2888:3888
```

<br><br>

# 8. myid 생성

<br>

```bash
mkdir -p /home/zookeeper/zookeeper/data
cd /home/zookeeper/zookeeper/data
touch myid
echo 1 > myid # 노드 순서대로 기입
```

<br><br>

# 9. zookeeper 서버 실행 (3서버 모두 실행)

<br>

```bash
cd /home/zookeeper/zookeeper/bin
./zkServer.sh start
```

<br><br>

# 10. 실행 확인

<br>

```
./zookeeper/bin/zkServer.sh status
```

<br><br>

# 서버 중지

<br>

```
./zkServer.sh stop
```

<br><br>