---
layout: post
title: Effective C++_#3
description: "const를 자주 사용하자"
modified: 2017-07-05
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

- 포인터 왼쪽의 const는 pointer가 가리키는 data의 상수성을 오른쪽의 const는 pointer의 상수성을 나타낸다.

<br/>

---

## STL Iterator

- 포인터를 본뜬 것이기 때문에, 기본적인 동작 원리가 T* 포인터와 비슷하다.
- Iterator 앞에 붙는 const는 T* const와 의미가 동일하다.
- 

---