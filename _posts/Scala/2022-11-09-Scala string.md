---
title: "Scala string"
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

# 1. String 기본

<br>

```scala
var string1 = "I am a string"
var string2 = "I'm d string"
var string3 = "Put \" quotes\" by escaping"
var string4 = "Special \t characters \n insert"

var string5 = """
Hello
String
World!
"""
```

```scala
var myLanguage = "Scala"
var myExperience = 7
var myString = s"I love $myLanguage"
```

```scala
import java.time.Year
val thisYear = Year.now.getValue

var myLanguage = "Scala"
var myExperience - 7
var myString1 = s"I love $myLanguage."
var myString2 = s"I started learning it in ${thisYear - myExperience}"
```

```scala
var numQ = 56
var numC = 53
var resultStr = f"""You got $numC questions out of
                $numQ correct. Your score was
                ${numC.toFloat/numQ.toFloat*100}%.2f%%"""
var string5 = "Use \n to print nuw lines"
var string6 = "Use \\n to print nuw lines"
var jsonString1 = "{\"key1\": \"value1\"}"
var jsonString2 = """
{"key1": "value1",
"key2":"value2",
"key3":"value3"}
"""

var jsonString3 = 
"""
    |{"key1":"value1",
    | "key2":"value2",
    | "key3":"value3"}
""".stripMargin

import java.time.Year

val thisYear = Year.now.getValue

var myLanguage = "Scala"
var myExperience = 7
var myString1 = s"I love $myLanguage."
var myString2 = s"I started learning it in ${thisYear - myExperience}"

var numQuestions = 56
var numCorrect = 53
var resultStr = f"""You got $numCorrect questions out of
                $numQuestions correct. Your score was
                ${numCorrect.toFloat/numQuestions.toFloat*100}%.2f%%"""
                
var string5 = raw"You cat print a new line with \n"
```

<table>
  <thead>
    <tr>
      <th colspan=1>구분</th>
      <th colspan=1>설명</th>
    <tr>
  </thead>
  <tbody>
    <tr>
      <td>%c</td>
      <td>character</td>
    </tr>
    <tr>
      <td>%d</td>
      <td>decimal (integer) number (base 10)</td>
    </tr>
    <tr>
      <td>%e</td>
      <td>exponential floating-point number</td>
    </tr>
    <tr>
      <td>%f</td>
      <td>floating-point number</td>
    </tr>
    <tr>
      <td>%i</td>
      <td>integer (base 10)</td>
    </tr>
    <tr>
      <td>%o</td>
      <td>octal number (base 8)</td>
    </tr>
    <tr>
      <td>%s</td>
      <td>a string of characters</td>
    </tr>
    <tr>
      <td>%u</td>
      <td>unsigned decimal (integer) number</td>
    </tr>
    <tr>
      <td>%x</td>
      <td>unmber in hexadecimal (base 16)</td>
    </tr>
    <tr>
      <td>%%</td>
      <td>print a percent sign</td>
    </tr>
    <tr>
      <td>\%</td>
      <td>print a percent sign</td>
    </tr>
  </tbody>
</table>

<br><br>

# 2. String Utility

<br>

```scala
String.length
String.foreach(operation)
String.toUpperCase()
String.toLowerCase()
String.charAt(index)
String.concat(anotherString)
String.trim
String.equals(anotherString) // 문자열 까지만 비교
String.eq(anotherString) // point 까지 비교
String.split("separator")
```
