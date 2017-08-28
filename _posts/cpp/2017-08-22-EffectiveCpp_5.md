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

Empty e1;       // 기본 생성자 호출
Empty e2(e1);   // 복사 생성자 호출
e2 = e1;        // 복사 대입 연산자 호출
```


```cpp
template<class T>
class NamedObject {
public:
    NamedObject(std::string& name, const T& value);

    ..,

private:
    std::string& nameValue;
    const T objectValue;
}
```

위와 같이 멤버 변수에 참조자가 선언 되어 있다면 복사 대입 연산자가 제대로 동작하기 어렵다  
그 이유는 **C++ 참조자는 원래 자신이 참조하고 있는 것과 다른 객체를 참조할 수 없기** 때문이다  
그래서 멤버 변수에 참조자를 쓰려면 직접 복사 대입 연산자를 지정해 주어야한다.  
**상수 멤버**의 경우에도 수정을 허용하지 않기 때문에 비슷하게 동작한다  

* C++ 참조자는 원래 자신이 참조하고 있는 것과 다른 객체를 참조할 수 없다
<br/>

---

## 이것만은 잊지말자
- 컴파일러는 경우에 따라 클래스에 대해 기본 생성자, 복사 생성자, 복사 대입 연산자, 소멸자를 임시적으로 만들어 놓을 수 있습니다
