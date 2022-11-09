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

// String.length
// String.foreach(operation)
// String.toUpperCase()
// String.toLowerCase()
//String.charAt(index)
// String.concat(anotherString)
// String.trim
// String.equals(anotherString) // 문자열 까지만 비교
// String.eq(anotherString) // point 까지 비교
// String.split("separator")

```

```scala
// split

var string6 = "I really enjoyed the movie Parasite, this year's Academy Award winner"

val arr1 = string6.split(' ')

val arr2 = "Directly,form,a,sting,literal".split(',')
```

```scala
// Starts or Ends With
// "someStringLiteral".startsWith("prefix", index)

"someStringLiteral".startsWith("Str", 4)
"someStringLiteral".startsWith("some")

// "someStringLiteral".endsWith("suffix")
"someStringLiteral".endsWith("al")

//"someStringLiteral".indexOf("character")
//"someStringLiteral".indexOf("character", index) index 부터 찾는다.
//"someStringLiteral".indexOf("prefix")
//"someStringLiteral".indexOf("prefix", index)

"someStringLiteral".indexOf("S")
"someStringLiteral".indexOf("S", 5)
"someStringLiteral".indexOf("Li")
"someStringLiteral".indexOf("NO")

// lastIndexOf() 맨 마지막에 있는 것 

// "someStringLiteral".substring(sIndex) non inclusive
// "someStringLiteral".substring(sIndex, endIndex)

"someStringLiteral".substring(3)
"someStringLiteral".substring(3, 7)
```

<br><br>

# 3. Regula Expressions

<br>


<table>
  <thead>
    <tr>
      <th colspan=1>Pattern</th>
      <th colspan=1>Matches</th> 
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>abc...</td>
      <td>Letters</td>
    </tr>
    <tr>
      <td>123...</td>
      <td>Digits</td>
    </tr>
    <tr>
      <td>\d</td>
      <td>Any digit</td>
    </tr>
    <tr>
      <td>\D</td>
      <td>Any non-digit character</td>
    </tr>
    <tr>
      <td>.</td>
      <td>Any character</td>
    </tr>
    <tr>
      <td>\.</td>
      <td>Literal period</td>
    </tr>
    <tr>
      <td>[abc]</td>
      <td>a, b, pr c</td>
    </tr>
    <tr>
      <td>[^abc]</td>
      <td>NOT a, b, nor c</td>
    </tr>
    <tr>
      <td>[a-z]</td>
      <td>Characters a to z</td>
    </tr>
    <tr>
      <td>\w</td>
      <td>Any alphanumeric character</td>
    </tr>
    <tr>
      <td>\W</td>
      <td>Any non-alphanumeric character</td>
    </tr>
    <tr>
      <td>{m}</td>
      <td>m repetitions</td>
    </tr>
    <tr>
      <td>*</td>
      <td>Zero or more repetitions<td>
    </tr>
    <tr>
      <td>+</td>
      <td>One or more repetitions</td>
    </tr>
    <tr>
      <td>?</td>
      <td>Optional character</td>
    </tr>
    <tr>
      <td>\s</td>
      <td>Any whitespace character</td>
    </tr>
    <tr>
      <td>\S</td>
      <td>Any non-whitespace character</td>
    </tr>
    <tr>
      <td>(...)</td>
      <td>Capture group<td>
    </tr>
    <tr>
      <td>(abc|def)</td>
      <td>abc or def</td>
    </tr>
  </tobdy>
</table>

<br>

```scala
// 2016-02-14
// raw"(\d{4})-(\d{2})-(\d{2})".r

// "(S|s)cala.r or "[Ss]cala

// xxxx or xxx-xxxx-xxxx
// "[0-9]{3}[- ][0-9]{4}[- ][0-9]{4}".r

// ERRxxxx
// raw"ERR-(\d+)".r

// regularExpression.findFirstIn(stringToSearch)
"Hello".r.findFirstIn("Hello World")
// regular Expression.findAllIn(stringToSearch)

// someString.replaceFirst("srchStr", "replaceStr")
"Hello World".replaceFirst("World", "Universe")
// someString.replaceAll("srchStr", "replaceStr")

// someString.replaceFistIn("srchStr", "replaceStr")
"World".r.replaceFirstIn("Hello World", "Universe")
// RegEx.replaceAllIn("srchStr", "replaceStr")

// stringToMatch.equals(otherString)
"Hello World".equals("Hello World")
//stringToMatch.equalsIgnoreCase(otherString)

// stringToMatch.matches(regularExpressionString)
"Hello World".matches("H[a-z]* W[a-z]*")

"A Scala String".length
"A Scala String".toUpperCase
"A Scala String".toLowerCase()
"A Scala String".foreach(println)

val scalaStringLiteral = "A scala String"
val nulString = null
val scalaString1 = new String("A Scala String")
val scalaString2 = new String("A Scala String")

scalaStringLiteral.eq("A Scala String")

val string6 = "I really enjoyed the movie Parasite, this year's academy award winner"
val arr1 = string6.split(' ')
val arr2 = "Directly,from,a,string,literal".split(',')
val result = "SKC&C Learning Scala".startsWith("SK")

// Regular Expression
val pattern1 = """Scala \d{1}\.\d{1,2}""".r
val seq1 = "We are learning Scala 2.12"
pattern1.findFirstIn(seq1)

// Regular Expression
val pattern1 = raw"ERR-(\d+)".r
val seq1 = "ERR-39763"
pattern1.findFirstIn(seq1)

// Regular Expression
val datePattern = """(\d{4})-(\d{2})-(\d{2})""".r
val seq1 = "2022-11-09"
datePattern.findFirstIn(seq1)
seq1 match {
    case datePattern(year, month, day) => s"$year was goot for wind"
}

// Regular Expression
val pattern1 = "[1-5]+".r
val seq1 = "12 67 93 43 51"
val result = pattern1.findAllIn(seq1)

result.foreach(println)

//pattern1: scala.util.matching.Regex = [1-5]+
//seq1: String = 12 67 93 43 51
//result: scala.util.matching.Regex.MatchIterator = non-empty iterator
//12
//3
//43
//51

// Regular Expression
"Hello World and World".replaceFirst("World", "Universe")
// res162: String = Hello Universe and World

// Regular Expression
"Hello World and World".replaceAll("World", "Universe")
// res162: String = Hello Universe and Universe
```



<br><br>

# 4. 