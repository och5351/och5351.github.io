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
      <td>&gt;</td>
      <td>Greater than</td>
      <td>x &gt; y</td>
    </tr>
    <tr>
      <td>&lt;</td>
      <td>Less than</td>
      <td>x &lt; y</td>
    </tr>
    <tr>
      <td>&ge;</td>
      <td>Greater than or equal to</td>
      <td>x &ge; y</td>
    </tr>
    <tr>
      <td>&le;</td>
      <td>Less than or equal to</td>
      <td>x &le; y</td>
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
      <td>!(x &gt; 100 || x &lt; 50)</td>
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
      <td>&lt;&lt;</td>
      <td>Binary Left-Shift</td>
      <td>x &lt;&lt; 2</td>
    </tr>
    <tr>
      <td>&gt;&gt;</td>
      <td>Binary Right-Shift</td>
      <td>x &gt;&gt; 2</td>
    </tr>
    <tr>
      <td>&gt;&gt;&gt;</td>
      <td>Shift-Right Zero-Fill</td>
      <td>x &gt;&gt;&gt; 2</td>
    </tr>
    <tr>
      <td>=</td>
      <td>Simple assignment</td>
      <td>x = 6</td>
    </tr>
    <tr>
      <td>+=</td>
      <td>Add and Assignment</td>
      <td>x += 4</td>
    </tr>
    <tr>
      <td>-=</td>
      <td>Subtract and Assignment</td>
      <td>x -= 4</td>
    </tr>
    <tr>
      <td>*=</td>
      <td>Multiply and Assignment</td>
      <td>x *= 4</td>
    </tr>
    <tr>
      <td>/=</td>
      <td>Divide and Assignment</td>
      <td>x /= 4</td>
    </tr>
    <tr>
      <td>%=</td>
      <td>Modulus and Assignment</td>
      <td>x %= 4</td>
    </tr>
    <tr>
      <td>&=</td>
      <td>Bitwise AND and Assignment</td>
      <td>x &= 4</td>
    </tr>
    <tr>
      <td>|=</td>
      <td>Bitwise OR and Assignment</td>
      <td>x += 4</td>
    </tr>
    <tr>
      <td>^=</td>
      <td>Bitwise XOR and Assignment</td>
      <td>x ^= 4</td>
    </tr>
    <tr>
      <td>&lt;&le;</td>
      <td>Left-shift and Assignment</td>
      <td>x &lt;&le; 4</td>
    </tr>
    <tr>
      <td>&gt;&ge;</td>
      <td>Right-shift and Assignment</td>
      <td>x &gt;&ge; 4</td>
    </tr>
  </tbody>
</table>


<br><br>

# 3. 예제

<br>

```scala
import scala.math._
sqrt(2)

val myHello = "hello"
// myHello(4) // sugar coating
myHello.apply(4)

// 스칼라의 모든 것은 객체이다.
// 심지어 연산 기호 조차도 객체이다.
1.+(5) // 1 + 1 은 1.+(5) 의 sugar coating 이다.
// res172: Int = 6

val one: Int= 1
one.to(10)

// one: Int = 1
// res174: scala.collection.immutable.Range.Inclusive = Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

val x : BigInt = 123456789
x * x * x * x
// x.*(x).*(x).*(x)

// x: scala.math.BigInt = 123456789
// res176: scala.math.BigInt = 232305722798259244150093798251441

5 / 2
5.0 / 2.0

// res182: Int = 2
// res184: Double = 2.5

val a = 12
val b = 5

~a
a & b
a | b
a ^ b

// a: Int = 12
// b: Int = 5
// res192: Int = -13
// res193: Int = 4
// res194: Int = 13
// res195: Int = 9
```

<br><br>

# 4. 스칼라 연산자 순위

<br>

<table>
  <thead>
    <tr>
      <th colspan=1>Category</th>
      <th colspan=1>Operator</th>
      <th colspan=1>Associativity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Postfix</td>
      <td>() []</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Unary</td>
      <td>! ~</td>
      <td>Right to Left</td>
    </tr>
    <tr>
      <td>Multiplicative</td>
      <td>* / %</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Additive</td>
      <td>+-</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Shift</td>
      <td>&gt;&gt; &gt;&gt;&gt; &lt;&lt;</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Relational</td>
      <td>&gt; &ge; &lt; &le;</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Equality</td>
      <td>== !=</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Bitwise AND</td>
      <td> & </td>
      <td> Left to Right </td>
    </tr>
    <tr>
      <td>Bitwise XOR</td>
      <td>^</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Bitwise OR</td>
      <td>|</td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Locgical AND</td>
      <td> && </td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Logical OR</td>
      <td> || </td>
      <td>Left to Right</td>
    </tr>
    <tr>
      <td>Assignment</td>
      <td>= += -= *= /= %= &gt;&ge; &lt;&le; &= ^= |=</td>
      <td>Right to Left</td>
    </tr>
    <tr>
      <td>Comma</td>
      <td>,</td>
      <td>Left to Right</td>
    </tr>
  </tbody>
</table>