---
layout: post
title: Effective C++ Link
description: ""
published: true
modified: 2017-08-28
tags: [C++]
image:
  feature: background/Computer/5.jpg
  credit: unsplash
---


## 1장

- [define 대신 const, enum, inline을 쓰자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-07-05-EffectiveCpp_Chapter1.md)

- [기회가 있을 떄마다 Const를 사용하자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-07-05-EffectiveCpp_Chapter1.md#기회가-있을-때마다-const를-사용하자)

- [객체를 사용하기 전에 반드시 그 객체를 초기화하자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-07-05-EffectiveCpp_Chapter1.md#객체를-사용하기-전에-반드시-그-객체를-초기화하자)

### 2장 생성자 소멸자 및 대입 연산자


- [C++가 은근슬쩍 만들어 호출해 버리는 함수들에 촉각을 세우자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#c가-은근슬쩍-만들어-호출해-버리는-함수)

- [컴파일러가 만들어낸 함수가 필요없으면 확실히 이들의 사용을 금해버리자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#컴파일러가-만들어낸-함수가-필요없으면-확실히-이들의-사용을-금해버리자)

- [다형성을 가진 기본 클래스에서는 소멸자를 반드시 가상 소멸자로 선언하자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#다형성을-가진-기본-클래스에서는-소멸자를-반드시-가상-소멸자로-선언하자)

- [예외가 소멸자를 떠나지 못하도록 붙들어 놓자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#예외가-소멸자를-떠나지-못하도록-붙들어-놓자)

- [객체 생성 및 소멸 과정 중에는 절대로 가상 함수를 호출하지 말자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#객체-생성-및-소멸-과정-중에는-절대로-가상-함수를-호출하지-말자)

- [대입 연산자는 *this의 참조자를 반환하게 하자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#대입-연산자는-this의-참조자를-반환하게-하자)

- [operator=에서는 자기대입에 대한 처리가 빠지지 않도록 하자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#operator-에서는-자기-대입에-대한-처리가-빠지지-않도록-하자)

- [객체의 모든 부분을 빠짐없이 복사하자](https://github.com/YujinJung/yujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_Chapter2.md#객체의-모든-부분을-빠짐없이-복사하자)

### 3장 자원 관리

- [자원 관리에는 객체가 그만!](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-28-EffectiveCpp_13.md)

<br/>

---

책에 있는 내용을 개인적으로 정리하고 있습니다.  

책의 저작권 및 기타 권한은 출판사와 지은이/옮긴이에 있습니다.  
문제가 될 시 [ujin.jung9@gmail.com](ujin.jung9@gmail.com) 으로 메일을 주신다면 바로 삭제하겠습니다.  

- 제 목 : Effective C++
- 지은이 : Scott Meyers
- 옮긴이 : 곽용재
- 발행인 : 유재성
- 발행처 : (주)피어슨에듀케이션코리아

---
