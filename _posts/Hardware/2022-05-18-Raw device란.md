---
title: "Raw Device 란"
header:
  #image: /assets/images/linux/ubuntu_logo.png
  #caption: "Photo credit: [**Unsplash**](http://wiki.hash.kr/images/6/6e/%EC%9A%B0%EB%B6%84%ED%88%AC_%EB%A1%9C%EA%B3%A0.png)"
layout: posts
categories:
  - Hardware
tags:
  - Hardware
toc: true
toc_sticky: true

date: 2022-05-18
last_modified_at: 2022-05-18
---

# Raw Device 

<br>

Raw device 는 File System이 Setup 되지 않은 disk drive이다. Raw Device는 데이터베이스와 같이 주로 자신들의 캐싱 시스템을 갖고 있는 경우에 서비스 프로그램에서 많이 사용한다. 운영체제의 캐싱시스템은 일반적인 상황에서는 성능에 지대한 영향을 미치지만 데이터베이스 등에서는 이미 자신의 방식으로 캐싱을 하기 때문에 두번의 캐싱으로 인해 부하가 생길 수 있다. 

그러므로 운영체제가 지원하는 부분을 사용하지 않게 하기 위해서 raw device를 사용한다. 또 두대 이상의 머신과 공유하고 있는 disk로 되는 OPS의 경우 shared disk를 OS file system으로 구성하면 한쪽에서만 이를 볼 수 있고 동시에 access하는 것이 불가능하다. 그러므로 raw device를 사용해야하며, raw device를 사용하면 os를 거치지 않으므로 보다 빠른 access를 기대할 수 있다.

<br><br>