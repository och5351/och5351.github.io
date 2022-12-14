---
title: "Zookeeper 란"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Zookeeper
tags:
  - Zookeeper
  - Hadoop
toc: true
toc_sticky: true

date: 2022-12-14
last_modified_at: 2022-12-14

---

<br><br>

코디네이션 서비스 시스템으로서 하둡 하위 프로젝트로 부터 시작 됐다고 한다.

<br><br>

# 1. 구조 

<br>

디렉터리 구조이며 , 해당 디렉터리 노드에 데이터를 저장할 수 있다.

```bash
[zk: localhost:2181(CONNECTED) 0] ls /
[kafka_znode, zookeeper]
```


<br><br>

# 2. Persistent Node

<br>

노드에 데이터를 저장하면 일부러 삭제하지 않는 이상 삭제되지 않고 영구히 저장된다.

<br><br>

# 3. Ephemeral Node 

<br>

노드를 생성한 클라이언트의 세션이 연결되어 있을 경우만 유효하다. 즉 클라이언트 연결이 끊어지는 순간 삭제 된다. 이를 통해서 클라이언트가 연결이 되어 있는지 아닌지를 판단하는데 사용할 수 있다.(클러스터를 구성할 때 클러스터내에 서버가 들어오면, 이 Ephemeral Node 로 등록하면 된다.)

<br><br>

# 4. Sequence Node 

<br>

노드를 생성할 때 자동으로 sequence 번호가 붙는 노드이다. 주로 분산락을 구현하는 데 이용한다.

<br><br>

# 5. Watcher

<br>

Watch 기능은 Zookeeper 클라이언트가 특정 znode에 watch를 걸어놓으면,  해당 znode가 변경이 되었을 때, 클라이언트로 callback 호출을 날려서 클라이언트에 해당 znode가 변경이 되었음을 알려준다. 그리고 해당 watcher는 삭제된다.

<br><br>