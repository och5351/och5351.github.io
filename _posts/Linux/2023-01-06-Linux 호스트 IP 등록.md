---
title: "Linux 계정 Password 만료일 변경"
header:
  #image: /assets/images/linux/ubuntu_logo.png
  #caption: "Photo credit: [**Unsplash**](http://wiki.hash.kr/images/6/6e/%EC%9A%B0%EB%B6%84%ED%88%AC_%EB%A1%9C%EA%B3%A0.png)"
layout: posts
categories:
  - Linux
tags:
  - Linux
toc: true
toc_sticky: true

date: 2022-12-20
last_modified_at: 2022-12-20
---


``` bash
# chage 명령어를 사용하여 변경
# sudo chage -E 'YYYY-MM-DD' 계정
sudo chage -E '2999-12-31' user1
```