---
title: "Scala operators"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Scala
tags:
  - Scala
toc: true
toc_sticky: true

date: 2022-11-09
last_modified_at: 2022-11-09

---

<br><br>

# 1. 함수와 메소드

<br>

* 코드 블럭을 묶어서 같은 결말 또는 같은 업무로 실행하고 싶을 떄 만든다.
> 객체지향은 객체를 정의하여 내부에 내용들을 정의하고 추상화한다.

* 스칼라에서는 function 들이 이미 만들어진 객체이다.

* function은 변수에 저장할 수 있다.

* 스칼라에서는 모든 것들이 객체다.

<br><br>

# 2. 연산자

<br>

<table>
  <thead>
    <tr>
      <th colspan=1>Operator</th>
      <th colspan=1>Name</th>
      <th colspan=1>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>+</td>
      <td>Addition</td>
      <td>x + y</td>
    </tr>
    <tr>
      <td>-</td>
      <td>Subtraction</td>
      <td>x - y</td>
    </tr>
    <tr>
      <td>*</td>
      <td>Multiplication</td>
      <td>x * y</td>
    </tr>
    <tr>
      <td>/</td>
      <td>Division</td>
      <td>x / y</td>
    </tr>
    <tr>
      <td>%</td>
      <td>Modulus</td>
      <td>x % y</td>
    </tr>
    <tr>
      <td>==</td>
      <td>Equal</td>
      <td>x == y</td>
    </tr>
    <tr>
      <td>!=</td>
      <td>Not equal</td>
      <td>x != y</td>
    </tr>
    <tr>
      <td>&gt</td>
      <td>Greater than</td>
      <td>x &gt y</td>
    </tr>
    <tr>
      <td> &lt </td>
      <td>Less than</td>
      <td>x &lt y</td>
    </tr>
    <tr>
      <td>&gt=</td>
      <td>Greater than or equal to</td>
      <td>x &gt= y</td>
    </tr>
    <tr>
      <td>&lt=</td>
      <td>Less than or equal to</td>
      <td>x &lt= y</td>
    </tr>
    <tr>
      <td>&&</td>
      <td>Returns True if bvoth statements are true</td>
      <td>x &gt= 4 && x &lt= 8 y</td>
    </tr>
    <tr>
      <td>||</td>
      <td>Return True if one of the statements is true</td>
      <td>x &gt 100 || x &lt 50</td>
    </tr>
    <tr>
      <td>!</td>
      <td>Return the opposite of the evaluated result. Return True if false and False if true</td>
      <td>!(x &gt 100 || x &lt 50)</td>
    </tr>
    <tr>
      <td>&</td>
      <td>Binary AND</td>
      <td>x & y</td>
    </tr>
    <tr>
      <td>|</td>
      <td>Binary OR</td>
      <td>x | y</td>
    </tr>
    <tr>
      <td>^</td>
      <td>Binary XOR</td>
      <td>x ^ y</td>
    </tr>
    <tr>
      <td>~</td>
      <td>Binary One's Complement</td>
      <td>~x</td>
    </tr>
    <tr>
      <td>&lt&lt</td>
      <td>Binary Left-Shift</td>
      <td>x &lt&lt 2</td>
    </tr>
    <tr>
      <td>&gt&gt</td>
      <td>Binary Right-Shift</td>
      <td>x &gt&gt 2</td>
    </tr>
    <tr>
      <td>&gt&gt&gt</td>
      <td>Shift-Right Zero-Fill</td>
      <td>x &gt&gt&gt 2</td>
    </tr>
    <tr>
      <td>=</td>
      <td>Simple assignment</td>
      <td>x = 6</td>
    </tr>
  </tbody>
</table>


<br><br>