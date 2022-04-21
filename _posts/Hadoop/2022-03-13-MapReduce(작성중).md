---
title: "MapReduce(작성중)"
header:
  image: /assets/images/hadoop/hadoop_logo.svg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
layout: posts
categories:
  - Hadoop
tags:
  - Hadoop
  - Cloudera
  - CDP
toc: true
toc_sticky: true
toc_label: "MapReduce 정리"
# toc_icon: "sticky-note"
---

<a id="home1"></a>

# 맵리듀스란?

<br>

데이터 처리를 위한 프로그래밍 모델. 단순하지만 유용한 프로그램을 작성할 수 있으며

<br><br>

## 목차

- [맵과 리듀스](#1)
- [하둡 처리 패턴](#2)
- [관계형 데이터베이스와의 비교](#3)
- [그리드 컴퓨팅과의 비교](#4)
- [자발적 컴퓨팅과의 비교](#5)
- [하둡 역사](#6)

<br><br>
<a id="1"></a>

# 맵과 리듀스

<br>

맵리듀스 작업은 크게 맵 단계와 리듀스 단계로 구분된다. 각 단계는 입력과 출력으로 키-값의 쌍을 가지며, 그 타입은 프로그래머가 선택한다. 또한 프로그래머는 맵 함수와 리듀스 함수를 작성해야 한다. <br>

<div align="center">
<img src="https://user-images.githubusercontent.com/45858414/147897985-4ab97da6-a0ca-4772-906b-9fc3aae0bc20.png" width="70%" height="70%">
<p>맵 리듀스의 논리적 데이터 흐름</p>
</div>

<br>

<br>

[목차로](#home1)

<br><br>
<a id="1"></a>

# 전체 데이터 질의

<br>
