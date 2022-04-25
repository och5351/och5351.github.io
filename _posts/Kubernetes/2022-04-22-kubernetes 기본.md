---
title: "Kubernetes 기본"
toc: true
header:
  image: /assets/images/kubernetes/kubernetes_logo.svg
  caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/3/39/Kubernetes_logo_without_workmark.svg)"
layout: posts
categories:
  - Kubernetes
tags:
  - Kubernetes
toc: true
toc_sticky: true

date: 2022-03-23
last_modified_at: 2022-04-22
---

<br><br>

<a id="home1"></a>

# 쿠버네티스 아키텍처

<br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128103567-af756b41-4566-4ebd-a43a-e733890f81ba.png"  width="70%" height="70%"/>
</div>
<br><br>

* SRE : Site Reliability Engineer (사이트 신뢰성 엔지니어)의 약어로, 운영 업무만이 아닌 운영의 신뢰성 향상, 자동화를 목적으로 하는 프로그램 개발 담당 엔지니어
* 마스터는 k8s 클러스터의 단일 장애점이 되지 않도록 다중화 할 수 있다.
* 쿠버네티스에 연결 가능한 노드의 개수는 버전 1.11 기준으로 5,000대 가능.

<br><br>

## 목차

- [Kubernetes 의 장점](#1)
- [Kubernetes 세부 아키텍처](#2)
- [쿠버네티스를 구성하는 기본 컴포넌트와 플러그인(컨테이너)](#3)
- [애드온 컴포넌트](#4)
- [쿠버네티스 계층 구조](#5)
- [쿠버네티스 API](#6)
- [오브젝트](#7)
- [워크로드(Workload)](#8)
- [컨테이너(Container)](#9)
- [파드(Pod)](#10)
- [컨트롤러(Controller)](#11)
- [설정(Configuration)](#12)
- [서비스(Service)](#13)
- [스토리지(Storage)](#14)


<br><br>
<a id="1"></a> 

# Kubernetes 의 장점

<br><br><br>

### 1. 애플리케이션의 잦고, 빠른 출시 시기

<br>
쿠버네티스의 롤아웃, 롤백 기능을 통하여 새로운 기능을 자주 출시하고 버그 수정을 긴급하게 투입하는 것과 같이 민감한 작업을 안전하게 자동화 해 준다.
정식 운영 중인 서비스의 앱 컨테이너를 무정지로 교체할 수 있고 교체 중 발생하는 성능 저하와 프로그램 충돌로 인한 서비스 정지를 막기 위해 컨테이너 교체 정책을 설정할 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 2. 무정지 서비스

<br>
쿠버네티스의 자기 회복 기능을 통하여 무정지 서비스 운영을 도와준다. 응답이 없어진 컨테이너를 재기동하며, 쿠버네티스 클러스터 내에 지정한 수만큼 컨테이너가 돌도록 관리 가능

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 3. 초기 비용을 낮추고 비지니스 상황에 맞게 규모를 조정

<br>
컨테이너 기술은 앱과 실행 환경을 하나로 묶어서 배포할 수 있게 해준다.그리고 쿠버네티스는 복수의 노드 위에서 컨테이너가 돌아갈 수 있도록 해준다.
이 때 클러스터의 각 노드들이 똑같은 스펙일 필요가 없으며, 비지니스의 초기 단계에서는 스펙이 낮고 저렴한 가상ㅎ서버를 사용하다가, 비지니스가 확대되면 고성능의 가상 서버나 물리 서버를 투입시키는 
전략을 취하여 초기 비용을 낮출 수 있다.

k8s 클러스터 내에서 컨테이너를 다른 노드로 옮기기 위해서는 먼저 해당 노드에 스케줄이 되지 않도록 설정하고, 해당 노드의 모든 컨테이너를 추방시키면 된다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 4. 쿠버네티스와 외부 서비스와의 연동

<br>
데이터베이스에 대한 컨태이너화는 신중하게 접근할 필요가 있다.(컨테이너는 태생적으로 언제든지 재시작될 수 있는 일시적인 존재로 상태를 포함하지 않는 것을 전제로 하기 때문)
이 때 한이브리드한 시스템을 구축하는 것이 한 가지 대안이 될 수 있다. 예를 들면, 클라우드의 DBaaS(Database as a Service)나 온프레미스에서 관리하는 데이터베이스와 연동하는 것.
이런한 연동을 위해 쿠버네티스는 외부 서비스를 내부 DBS에 등록하는 기능 제공

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 5. 개발 환경과 운영 환경의 분리

<br>
컨테이너의 개발이 완료되어 테스트까지 끝났다면 정식 서비스 때 배포하기 전까지는 이미지를 다시 빌드하지 않는 것이 좋다. (테스트로 검증되지 않은 기능이 포함될 여지가 있기 때문)
운영 환경에서 사용하는 엔드포인트나 인증 정보는 테스트 환경과 다르기 마련이며, 데이터베이스의 접속 주소나 HTTPS를 위한 인증서 등이 다르다.

쿠버네티스에서는 클러스터를 여러 개의 가상 환경으로 분할 하는 것이 가능. 그리고 각각의 가상환경에 설정 파일, 보안이 필요한 인증서나 비밀번호를 저장할 수 있다.
즉, 테스트가 완료된 컨테이너의 이미지를 그대로 정식 운영 환경에 배포할 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 6. 온프레미스와 클라우드 위에 구축

<br>
대형 클라우드 업체들은 EKS(AWS), AKS(Azure), IKS(IBM Cloud)와 같은 쿠버네티스 서비스를 제공한다. 쿠버네티스는 인프라의 복잡성을 감추며, 일관된 인터페이스로 다룰 수 있도록 설계되었다.

온프레미스와 클라우드 환경에서 동일한 인터페이스로 조작하며 운영할 수 있다.

<br>

<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 7. 애플리케이션 중심의 오케스트레이션

<br>
애플리케이션 개발자가 YAML 파일을 기술하여 쿠버네티스에 제출하면 로드밸런서, 저장소, 네트워크, 런타임 등의 환경이 구성된다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 8. 특정 기업에 종속되지 않는 표준 기술

<br>
쿠버네티스 프로젝트는 원래 구글에서 시작되었지만, CNCF에 기증되어 오픈 소스 프로젝트로 운영되고 있다. 170여 개의 회사가 참가하고 있기 때문에 특정 회사에 종속되지 않은 포준 기술로 자리 잡았음.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

### 9. 서버들의 가동률 높이기

<br>
CPU 사용률이 낮은 서버가 많은 것은 퍼블릭 클라우드에서도 온프레미스에서도 바람직하지 않다. 한편, 쿠버네티스에서 사용되는 컨테이너 기술은 애플리케이션이 정해진 서버에서 돌지 않아도 된다는 자유를 제공한다.
또한, CPU 사용 시간이나 메모리 요구량도 간단히 제어할 수 있다.

이 기술 덕분에 쿠버네티스는 가동률이 적은 서버의 애플리케이션을 한 곳에 모을 수 있다. 그래서 서버의 CPU 가동률을 높게 유지하면서도 안정적으로 서비스를 제공한다는 상반되는 요구사항을 충족시킬 수 있는 것.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<br><br>
<a id="2"></a> 

# Kubernetes 세부 아키텍처

<br>

Kubernetes는 Master와 Node로 구성 된다.

<br>
<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/128174332-28867e33-561c-44f5-a860-f76da00cda23.png" width="70%" height="70%"/>
</div>
<br><br>

<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">이름</th>
            <th colspan="1">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>NODE</td>
            <td>각 노드의 역할을 의미, 제어를 담당하는 master와 실행을 담당하는 node1과 node2가 존재</td>
        </tr>
        <tr>
            <td>NAME</td>
            <td>컨테이너의 실행 단위인 파드의 이름. 네임스페이스(Namespace) 내에서 유일한 이름이 되도록 해시 문자열이 추가로 붙기도 함.</td>
        </tr>
        <tr>
            <td>IMAGE</td>
            <td>컨테이너의 이미지와 태그</td>
        </tr>
    </thead>
</table>
</div>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="3"></a>

# 쿠버네티스를 구성하는 기본 컴포넌트와 플러그인(컨테이너)

<div align="center">
<table>
    <thead>
        <tr>
            <th colspan="1">구성 요소</th>
            <th colspan="1">개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>kubectl</td>
            <td>k8s 클러스터를 조작하기 위한 도구로 가장 빈번하게 이용되는 커맨드 라인 인터페이스.</td>
        </tr>
        <tr>
            <td>kube-apiserver</td>
            <td>kubectl 등의 API 클라이언트로부터 오는 REST 요청을 검증하고, API 오브젝트를 구성하고 상태를 보고한다.</td>
        </tr>
        <tr>
            <td>kube-scheduler</td>
            <td>쿠버네티스의 기본 스케줄러이며, 새로 생성된 모든 파드에 대해 실행할 최적의 노드를 선택한다. 스케줄러는 파드가 실행 가능한 노드를 찾은 다음 점수를 계산하여 가장 점수가 높은 노드를 선택한다.</td>
        </tr>
        <tr>
            <td>kube-controller-manager</td>
            <td>컨트롤러를 구동하는 마스터상의 컴포넌트</td>
        </tr>
        <tr>
            <td>cloud-controller-manager</td>
            <td>API를 통해서 클라우드 서비스와 연계하는 컨트롤러로, 클라우드 업체에서 개발한다.</td>
        </tr>
        <tr>
            <td>etcd</td>
            <td>k8s 클러스터의 모든 관리 데이터는 etcd에 저장된다. 이 etcd는 CoreOS가 개발한 분산 키/값 저장소로 신뢰성이 요구되는 핵심 데이터의 저장 및 접근을 위해 설계 되었다.</td>
        </tr>
        <tr>
            <td>kubelet</td>
            <td>
                <p>kubelet은 각 노드에서 다음과 같은 역할을 수행한다.</p>

* 파드와 컨테이너의 실행
* 파드와 노드의 상태를 API 서버에 보고
* 컨테이너의 동작을 확인하는 프로브 실행
* 내장된 cAdvisor를 통해 메트릭 수집 및 공개           
            
</td>
        </tr>
        <tr>
            <td>kube-proxy</td>
            <td>
                <p>kube-proxy는 각 노드에서 동작하며 로드밸런싱 기능을 제공한다.</p>
        
* 서비스와 파드의 변경을 감지하여 최신 상태로 유지
* iptables 규칙을 관리
* 서비스명과 ClusterIP를 내부 DNS에 등록
            
</td>
        </tr>
        <tr>
            <td>coredns</td>
            <td>파드가 서비스 이름으로부터 IP 주소를 얻기 위해 사용된다. 버전 1.11부터, kube-dns 대신 coredns가 사용되었다. 이전의 kube-dns가 부족했던 신뢰성, 보안성, 유연성이 coredns에서 개선되었다. CoreDNS 프로젝트는 CNCF가 관리한다.</td>
        </tr>
    </thead>
</table>
</div>
<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="4"></a>

# 애드온 컴포넌트

<br>
<table>
    <thead>
        <tr>
            <th colspan="1">구성 요소</th>
            <th colspan="1">개요</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>kube-flannel</td>
            <td>kube-flannel은 모든 노드에서 실행되어 여러 노드 사이에서 IPv4 네트워크를 제공한다. 이에 따라 컨테이너(파드)는 k8s 클러스터 내부에서 사용되는 IP 주소를 바탕으로 다른 노드에 있는 파드와 통신할 수 있다. 네트워크 접근 제어가 필요한 경우에는 calico를 사용해야 한다.</td>
        </tr>
        <tr>
            <td>kube-apiserver</td>
            <td>kubectl 등의 API 클라이언트로부터 오는 REST 요청을 검증하고, API 오브젝트를 구성하고 상태를 보고한다.</td>
        </tr>
        <tr>
            <td>kube-scheduler</td>
            <td>쿠버네티스의 기본 스케줄러이며, 새로 생성된 모든 파드에 대해 실행할 최적의 노드를 선택한다. 스케줄러는 파드가 실행 가능한 노드를 찾은 다음 점수를 계산하여 가장 점수가 높은 노드를 선택한다.</td>
        </tr>
        <tr>
            <td>kube-controller-manager</td>
            <td>컨트롤러를 구동하는 마스터상의 컴포넌트</td>
        </tr>
        <tr>
            <td>cloud-controller-manager</td>
            <td>API를 통해서 클라우드 서비스와 연계하는 컨트롤러로, 클라우드 업체에서 개발한다.</td>
        </tr>
    </thead>
</table>

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="5"></a>

# 쿠버네티스 계층 구조

<br>

클라이언트 도구인 kubectl을 통해 k8s 클러스터를 조작하는 사람을 제일 위에 놓은 k8s 클러스터의 구성도.
k8s를 조작하는 사람으로는 애플리케이션 개발자, SRE(Site Reliability Engineer), 그리고 서비스 운영 및 감시 담당자가 있을 수 있다.

<br>
예시) k8s 클러스터의 구성 개념도
<div align="center">
<img src = "https://user-images.githubusercontent.com/45858414/128276338-c3643b97-0cf8-471a-88c5-d4f54d5d6dc2.jpeg" width="70%" height="70%"/>
</div>

<br>

마스터 노드의 역할은 아래와 같다.

1. 쿠버네티스의 API 서버로서 클라이언트로부터의 명령을 받아들이고 실행
2. 컨테이너를 파드 단위로 스케줄링 및 삭제
3. 파드의 컨트롤러 기능과 외부 리소스 관리

>> 쿠버네티스의 스케줄링은 파드를 실행할 노드를 정하는 것을 의미 

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="6"></a>

# 쿠버네티스 API

<br>
쿠버네티스에 대한 조작은 모두 API를 통해 이뤄진다. 커맨드라인 유저 인터페이스인 kubectl은 마스터 노드상의 kuve-apiserver에게 쿠버네티스 API규약에 맞게 기술된 목표 상태 선언서인 매니페스트를 YAML 형식 혹은 JSON 형식으로 전송하여 오브젝트를 만들고, 바꾸고, 제거하는 일을 한다.
또한, 다양한 프로그램 언어로 API 라이브러리가 제공된다. 예를 들면 파이썬, GO 언어를 사용하여 쿠버네티스 운영을 자동화하는 것이 가능하다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="7"></a>

# 오브젝트

<br>

* 쿠버네티스 오브젝트란 k8s 클러스터 내부의 엔티티로서, 이 후 설명할 파드, 컨트롤러, 서비스 등의 인스턴스를 의미. 
* 각 오브젝트는 메타데이터에 기술된 이름에 의해 식별되므로 반드시 이름을 부여 해야 한다. (같은 종류(kind)의 오브젝트의 이름은 하나의 네임스페이스에서 반드시 유일해야 함.)
* 네임스페이스는 k8s 클러스터를 논리적으로 분할하여 사용하기 위해 존재하는 기능이다. 

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="8"></a>

# 워크로드(Workload)

<br>

* 워크로드란 오브젝트의 카테고리를 나타내는 용어로 컨테이너와 파드, 그리고 컨트롤러의 그룹을 의미.
* 컨테이너의 실행을 관리하기 위해 사용
* 애플리케이션이나 프로그램의 부하를 의미하기도 함

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="9"></a>

# 컨테이너(Container)

<br>

* 쿠버네티스에서는 컨테이너만을 독자적으로 실행하는 것은 불가능.(반드시 파드내에서 실행해야 한다.)
* 이미지 이름, 실행 명령어, 실행 인자, 환경 변수, 볼륨, CPU 사용 시간과 메모리 크기의 요청값 및 상한 값 등 설정 가능.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="10"></a>

# 파드(Pod)

<br>

* 파드는 컨테이너를 실행하기 위한 오브젝트.
* 한 개 혹은 여러 개의 컨테이너를 담을 수 있다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="11"></a>

# 컨트롤러(Controller)

<br>

* 파드의 실행을 제어하는 오브젝트
* 여러 종류의 컨트롤러가 있어 각 컨트롤러의 기능을 이해하고 목적에 맞게 구별해서 사용해야 함.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="12"></a>

# 설정(Configuration)

<br>

* 컨테이너 내 애플리케이션의 설정값이나 비밀번호 등의 정보는 배포된 네임스페이스로부터 취득하는 것이 좋다.
* ConfigMap 은 설정을 저장할 수 있다.
* Secret 오브젝트는 비밀번호와 같은 기밀 정보를 담을 수 있다.


<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="13"></a>

# 서비스(Service)

<br>

* 서비스는 파드와 클라이언트를 연결하는 역할을 수행.
* 서버 역할을 수행하는 파드가 클라이언트의 요청을 받을 수 있도록 대표 IP 주소를 취득하여 내부 DNS에 등록.
* 대표 IP 주소로의 요청 트래픽을 지정된 파드들에 부하 분산하며 전송하는 역할도 수행.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>

<a id="14"></a>

# 스토리지(Storage)

<br>

파드나 컨테이너는 실행 시에만 존재하는 일시적인 존재이기 때문에 중요한 데이터를 컨테이너의 파일 시스템에 저장 하지 않는다.
데이터를 잃지 않기 위해서는 <a href="https://kubernetes.io/ko/docs/concepts/storage/persistent-volumes">퍼시스턴트 볼륨(Persistent Volume)</a>을 사용하여 전원이 꺼져도 데이터가 유지되는 스토리지 시스템에 데이터를 저장해야 한다.

복수의 노드에서 접속 가능한 Persistent Volume 은 쿠버네티스의 범위에 포함되지 않으므로 외부 스토리지 시스템을 연동해야 한다.
쿠버네티스는 스토리지를 계층적으로 추상화한 오브젝트를 제공한다.

<br>
<div align="right"> 

[목차로](#home1) 
</div><br><br>


### 출처: 15단계로 배우는 도커와 쿠버네티스
### 출처: kubernetes document