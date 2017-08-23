---
layout: post
title: Effective C++_#3
description: "const를 자주 사용하자"
published: false
modified: 2017-08-24
tags: [C++]
image:
  feature: background/Computer/4.jpg
  credit: unsplash
---

##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

---

# 기회가 있을 때마다 Const를 사용하자

## 이유

- **의미적인 제약**이 가능하다
  - const 키워드가 붙은 객체는 외부 변경이 불가능하다.
  - 컴파일러가 이 제약을 단단히 지켜준다.
- 어떤 값이 불변이어야 한다는 것을 const 를 통해 다른 프로그래머에게 전달할 수 있다.

## 규칙

```cpp
// 비상수 포인터, 비상수 데이터
char greeting[] = "Hello";

// 비상수 포인터, 상수 데이터
const char *p = greeting;
char const *p = greeting;

// 상수 포인터, 비상수 데이터
char * const p = greeting;

// 상수 포인터, 상수 데이터
const char * const p = greeting;
```

포인터(*) 왼쪽의 const는 pointer가 가리키는 **data의 상수성**을 오른쪽의 const는 **pointer의 상수성**을 나타낸다.

### * 를 기준으로

#### 왼쪽
pointer가 가리키는 **DATA의 상수성**

#### 오른쪽
**POINTER의 상수성**

<br/>

---

## STL Iterator

- 포인터를 본뜬 것이기 때문에, 기본적인 동작 원리가 T* 포인터와 비슷하다.
- const iterator 는 T* const 와 의미가 동일하다.
  - 가리키는 대상의 데이터를 변경 가능
- const_iterator 는 const T* 와 의미가 동일하다.
  - 가리키는 대상을 변경 가능

```cpp
std::vector<int> vec;

const std::vector<int>::iterator iter = vec.begin();
*iter = 10; // iter가 가리키는 대상을 변경한다.
++iter;     // iter는 상수이기 때문에 변경이 불가하다.

std::vector<int>::const_iterator iter = vec.begin();
*iter = 10; // *iter가 상수이기 때문에 변경이 불가하다. 
++iter;     // iter 값은 상수가 아니기 때문에 변경이 가능하다.
```

<br/>

---

## 함수 선언

### 위치

- 함수 반환 값
- 각각의 매개 변수
- 멤버 함수
- 함수 전체

### 멤버 함수

- 해당 멤버 함수가 상수 객체에 대해 호출 될 함수라는 것을 알린다.
- 중요한 이유
  - 클래스의 인터페이스를 이해하기 좋게 하기 위해서
    - 그 클래스로 만들어진 객체를 변경할 수 있는 함수는 무엇이고 변경할 수 없는 함수는 무엇인가를 사용자 쪽에서 알기 위해서
  - 상수 객체를 사용하기 위해서
    - 코드의 성능을 높이기 위해 객체를 전달할 때에 '상수 객체에 대한 참조자'로 전달하는 데 그 때에 필요한 것이 const 멤버 함수
- 특징
  - 멤버 변수의 값을 바꾸지 않는다.

---