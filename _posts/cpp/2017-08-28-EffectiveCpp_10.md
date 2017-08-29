---
layout: post
title: Effective C++_#10
description: "대입 연산자는 *this의 참조자를 반환하게 하자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

### C++ 의 대입 연산은 여러 개가 사슬처럼 엮일 수 있는 성질이 있다(컴파일러 수업 때처럼)
```cpp
int x, y, z;
x = y = z = 15;

x = (y = (z = 15));
```

z 에 15 할당되고 그게 y에 할당되고 이런 식  
이렇게 할당되려면 대입 연산자가 대입이 끝나고 좌변 인자에 대한 참조자를 반환해주어야 한다.  
이런 구현은 일종의 관례로써 따라주는 것이 좋다.

```cpp
class Widget {
public:
    ...
    Widget& operator=(const Widget& rhs)
    {
        ...
        return *this;
    }
    ...

    // 모든 형태의 대입 연산자에서 가능해야 한다.
    // = 뿐만 아니라 
    // +=, -=, *=, /= 등에도 동일한 규약이 적용
    Widget& operator+=(const Widget& rhs)
    {
        ...
        return *this;
    }
};
```

---

## 이것만은 잊지말자
- 대입 연산자는 *this의 참조자를 반환하도록 만드세요