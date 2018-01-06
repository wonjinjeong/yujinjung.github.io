---
layout: post
title: Regular Expression
description:
modified: 2017-04-11
category: Compiler
tags: [Compiler, Regular expression]
image:
  feature: background/Computer/4.jpg
  credit: unsplash
---
#####  개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

## Regular Expression
정규 표현식은 수학적으로 정의된 기호와 연산을 이용하여 언어를 귀납적으로 정의하기 위한 방법이다.<br/>
정규 표현식에서 정의되는 연산은 *, concatenation, | 이 있으며 피연산자는 기호의 유한 집합인 알파벳이나 기호가 된다.<br/>

r** = r*
(s|t)r = sr|tr
..

## Regular Definition
정규 정의는 다음과 같은 형태를 나타낸다.
![regulardef](/images/compiler/regulardef.png)

### Example
letter_ -> A|B|...|Y|Z|a|b|...|z|_
digit -> 0|1|2|...|9|
id -> letter_(letter|digit)*

## 그 외
1. (r)+ = r r*
2. r? = r \| e
3. [abc] = a\|b\|c
    [a-z] = a\|b\|...\|z

## Transition Diagram
### States
1. start States
2. accept States
<br/>

### edges(transitions)
<br/>

### actions
1. goto
2. accept
3. retract
4. fail


---
* 참고
    * [http://untitledtblog.tistory.com/10](http://untitledtblog.tistory.com/10)
