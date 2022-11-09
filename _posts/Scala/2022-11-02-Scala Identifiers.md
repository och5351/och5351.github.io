---
title: "Scala Identifiers"
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

date: 2022-11-02
last_modified_at: 2022-11-02

---

<br><br>

# 1. Alphanumeric Identifiers

<br>

`문자` 나 `_` 로 시작하고 다음에 숫자, 문자, 언더스코어가 들어가면 된다.

```scala
val scala3_ident = 1
val _scala3_ident = 2
```

<br><br>

# 2. Operator Identifiers

<br>

`+` , `-` , `*` , `/`

<br><br>

# 3. Mixed Identifier

<br>

Alphanumeric 후에 `_`를 적고 operator identifier를 적는다

```scala
myMultiplier_*
```

<br><br>

# 4. Literal Identifier

<br>

` ` ` 키워드를 변수로 사용하고 싶다면 backticks 를 사용하여 적어준다.

```scala
`for`
```

<br><br>

# 5. 변수 네이밍 룰

<br>

* 변수는 소문자 대문자를 식별한다.
* 변수는 숫자로 시작할 수 없다.
* `_` 로 변수를 시작할 수 있다.
* 키워드를 활용하여 변수를 만들 수 있으나 추천하지 않는다. backticks 활용
* camel case 네이밍 룰을 활용한다. 
  * UpperCamelCase : 새로운 단어 첫글자는 대문자로 하지만 UpperCamelCase 는 첫글자를 대문자로 한다. > 클래스나 객체 이름에 사용한다.
  * lowerCamelCase : 새로운 단어 첫글자는 대문자로 하지만 lowerCamelCase 는 첫글자를 소문자로 한다. > 함수나 변수에 사용한다.
* `_` 는 변수 이름 중간에 사용하지 않는다.
* `$` 은 사용하지 않는다.

> 스칼라에서는 의례적으로 변수를 소문자화 한다.

<br><br>

# 6. 불변 변수

<br>

* val 의 경우는 값을 변경할 수 없다. (immutable)

* var 의 경우는 값을 변경할 수 있다. (mutable)

```scala
// 데이터 타입을 꼭 안 적어줘도 되지만(타입 추론) 특별하게 데이터 타입을 지정해야 할 경우 사용한다.
val variableName:variableType = value
```

<br><br>

# 7. 스칼라 데이터 타입

<br>

#### 데이터 타입 계층도

<br>
<div align='center'>
<img src="https://user-images.githubusercontent.com/45858414/199382316-978b8569-0003-4bb8-9433-3beb65dbc804.png" width='70%'/>
</div>
<br>

- 모든 데이터 타입들은 클래스
- AnyVal 은 value 타입
- AnyRef 는 참조 타입

<table>
  <thead>
    <tr>
      <th colspan=1>Data Type</th>
      <th colspan=1>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Byte</th>
      <td>8 bit signed value. Range from -128 to 127</td>
    </tr>
    <tr>
      <th>Short</th>
      <td>16 bit signed value. Range -32768 to 32767</td>
    </tr>
    <tr>
      <th>Int</th>
      <td>32 bit signed value. Range -2147483648 to 922337203685477580</td>
    </tr>
    <tr>
      <th>Long</th>
      <td>64 bit signed value. Range -9223372036854775808 to 922337203685477580</td>
    </tr>
    <tr>
      <th>Float</th>
      <td>32 bit IEEE 754 single-precision float</td>
    </tr>
    <tr>
      <th>Double</th>
      <td>64 bit IEEE 754 double-precision float</td>
    </tr>
    <tr>
      <th>Char</th>
      <td>16 bit unsigned Unicode character. Range from U+0000 to U+FFFF</td>
    </tr>
    <tr>
      <th>Boolean</th>
      <td>Either literal true or literal fallse</td>
    </tr>
    <tr>
      <th>Unit</th>
      <td>Corresponds to no value</td>
    </tr>
    <tr>
      <th>String</th>
      <td>A sequence of Chars</td>
    </tr>
    <tr>
      <th>Null</th>
      <td>null or empty reference</td>
    </tr>
  </tbody>
</table>

<br><br>

# 8. 타입 문법

<br>

숫자형 문법

```scala
val (var) variableName: Type = value

// 0L or 0l --> long
// 0.0F or 0.0f --> Float
// 0.0D or 0.0d --> Double

// hexadecimal > 0x3F > 0x or 0X
```

<br>

문자형 문법

```scala
// (') single quote는 char
// (") double qoute 는 String

var c1 = 'A'      // regular character
var c2 = '\u0041' // Unicode character
var c3 = '\t'     // Escape sequence
```

<br>

```scala
var s1 = "A regular string"
var s2 = "A \"regular\" string"

// triple quote 안에 single quote, double quote를 넣어도 전부 문자열로 인식한다.
var s3 = """First line
            Seconde line
            
            Fourth line"""
```

<br><br>

# 9. 타입 변경

<br>

`toInt`, `toFloat`, `toString` 등등이 있다.

```scala
var intVar= 10
var intVarToFloat = intVar.asInstanceOf[Float]
var intVarToFloat2 = intVar.toFloat
var intVarToFloat3: Float = intVar
```

<br><br>

<div align='center'>
<img src='https://user-images.githubusercontent.com/45858414/199402195-c1837a11-cf5e-475b-b0d2-e718ac8fcf83.png' width='70%' />
</div>
<br>

타입이 

<br><br>

# 10. 

