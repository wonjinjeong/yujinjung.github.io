---
layout: post
title: Effective C++_#5
description: "C++가 은근슬쩍 만들어 호출해 버리는 함수들에 촉각을 세우자"
published: false
modified: 2017-08-27
tags: [C++]
---

##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

---

# C++가 은근슬쩍 만들어 호출해 버리는 함수?

## 저절로 선언이 되는 함수
1. 복사 생성자
2. 복사 대입 연산자
3. 소멸자
4. 기본 생성자 (생성자가 없을 경우)
<br/>
이 함수들은 모두 public 멤버이며 inline 함수이다.

```cpp
class Empty { };

// 위와 같이 쓴 것은
// 아래와 같이 쓴 것과 같다
class Empty {
public:
    Empty () { ... }    // 기본 생성자
    Empty (const Empty& rhs) { ... }    // 복사 생성자
    ~Empty () { ... }   // 소멸자

    Empty& operator=(const Empty& rhs) { ... }  // 복사 대입 연산자
};
```
