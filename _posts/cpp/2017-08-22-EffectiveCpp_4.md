---
layout: post
title: Effective C++_#4
description: "객체를 사용하기 전에 반드시 그 객체를 초기화하자"
published: false
modified: 2017-07-05
tags: [C++]
image:
  feature: background/Computer/4.jpg
  credit: unsplash
---

##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

---

# 객체를 사용하기 전에 반드시 그 객체를 초기화하자

찜찜하게 초기화 안하고 냅둘 바에야 그냥 무조건 항상 초기화를 진행한다

### 초기화
```cpp
// int의 직접 초기화 
int x = 0;

// 포인터의 직접 초기화
const char * test = "A C-style string";

// 입력 스트림에서 읽음으로써 '초기화' 수행
double d;
std::cin >> d;

// 생성자!!
~~
```

## 대입을 초기화와 구분하자

```cpp
ABEntry::ABEntry(const std::string& name, const std::string& address, const std::list<PhoneNumber>& phones)
{
  // '초기화'가 아님
  // 대입을 진행 중
  theName = name;
  theAddress = address;
  thePhones = phones;
  numTimesConsulted = 0;
}
```
C++ 규칙에 의하면 어떤 객체이든 그 객체의 데이터 멤버는  
**생성자의 본문이 실행되기 전에 초기화되어야 한다**고 명기되어 있다.
<br/>

### 해결 방법
대입문 대신 **멤버 초기화 리스트**를 사용한다
```cpp
ABEntry::ABEntry(const std::string& name, const std::string& address, const std::list<PhoneNumber>& phones)
: theName(name),
  theAddress(address),
  thePhones(phones),
  numTimesConsulted(0)
{ }

// Default Constructor
ABEntry::ABEntry()
: theName(),
  theAddress(),
  thePhones(),
  numTimesConsulted(0)
{ }
```
기본 생성자로 초기화하고 싶을 때도 멤버 초기화 리스트를 사용하는 것이 좋다.  
상수와 참조자의 경우 **나중에 수정이 불가능**하기 때문에 항상 초기화해야한다  

이런 식으로 해야할 때 아닐 때를 외우면서 진행하면 실수할 가능성이 분명 존재하기 때문에 그냥 항상 모든 멤버를 초기화하는 습관을 들이는 것이 정말정말 좋다




---
