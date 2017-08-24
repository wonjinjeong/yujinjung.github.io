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
  theName = name;
  theAddress = address;
  thePhones = phones;
  numTimesConsulted = 0;
}

```


---
