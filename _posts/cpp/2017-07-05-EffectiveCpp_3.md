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

// iter는 T* const처럼 동작합니다
const std::vector<int>::iterator iter = vec.begin();
*iter = 10; // iter가 가리키는 대상을 변경한다.
++iter;     // iter는 상수이기 때문에 변경이 불가하다.

// cIter는 T* const처럼 동작합니다
std::vector<int>::const_iterator cIter = vec.begin();
*cIter = 10; // *cIter가 상수이기 때문에 변경이 불가하다. 
++cIter;     // iter 값은 상수가 아니기 때문에 변경이 가능하다.
```

<br/>

---

## 함수 선언

### 위치

- 함수 반환 값
- 각각의 매개 변수
- 멤버 함수
- 함수 전체

---

## 멤버 함수 - p62

- 해당 멤버 함수가 상수 객체에 대해 호출 될 함수라는 것을 알린다.
- 위의 내용이 중요한 이유
  - 클래스의 인터페이스를 이해하기 좋게 하기 위해서
    - 그 클래스로 만들어진 객체를 변경할 수 있는 함수는 무엇이고 변경할 수 없는 함수는 무엇인가를 사용자 쪽에서 알기 위해서
  - 상수 객체를 사용하기 위해서
    - 코드의 성능을 높이기 위해 객체를 전달할 때에 '상수 객체에 대한 참조자'로 전달하는 데 그 때에 필요한 것이 const 멤버 함수
    - const 키워드의 유무를 통해 멤버 함수들의 **오버로딩**이 가능하다
    - 상수 객체가 생기는 경우는 ( 상수 객체에 대한 포인터 | 참조자 로 객체가 전달될 때 ) 이다
  
    
- 특징
  - 멤버 변수의 값을 바꾸지 않는다.

### 멤버 함수가 상수 멤버라는 것의 의미?
여기에는 두 가지의 큰 개념이 자리 잡고 있다  
* 비트 수준 상수성 (bitwise constness) / 물리적 상수성 (physical constness)
* 논리적 상수성(logical constness)

#### 비트 수준 상수성
어떤 멤버 함수가 그 객체의 어떤 데이터 멤버도 건드리지 않아야 그 멤버 함수가 **'const'**임을 인정하는 개념이다.
- static member는 제외
- C++ 에서 정의하고 있는 상수성이다

#### 예외
```cpp
class CTextBlock {
public:
  ...
  char& operator[] (std::size_t position) const
  { return pText[position]; }

private:
  char *pText;
};

// 이후 문제가 되는 코드
const CTextBlock cctb("Hello");
char *pc = &cctb[0];

*pc = 'J';
```

위의 코드를 보면 **operator[]** 함수의 **반환 값**에는 const가 없고 **함수**에만 const가 들어가있다.  
해당 함수 자체만 놓고 본다면 함수에만 const가 붙어있기 때문에 함수 안에서 값의 변경이 이루어지는 지만 보면 된다.  
그래서 살펴보면 해당 함수는 pText 값을 직접적으로 변경하는 것이 아닌 반환하고 있기 때문에 비트 수준의 상수성을 만족한다고 볼 수 있다.  
하지만 이후 문제가 되는 코드를 살펴보면  
pc에 포인터를 넘겨주고 있는 것을 볼 수 있는 데 **operator[]** 함수에서 반환 값을 const로 지정하지 않았기때문에 pc가 const가 아님에도 문제를 일으키지 않는다.  
그 후 pc 가 값을 변경하더라도 **어떠한 문제도 발생하지 않음**을 발견할 수 있는 데  
이러한 비트 수준 상수성의 논리적 오류로 인해 보완책으로 생겨난 것이 **논리적 상수성**이다

#### 코드 중복을 피하는 법
캐스팅을 이용한다

```cpp
class TextBlock {
public:
  ...
  const char& operator[](std::size_t position) const
  {
    ...
    return text[position];
  }

  char& operator[](std::size_t position)
  {
    return 
      const_cast<char&>{  // const 떼어내는 cast
        // cosnt 붙이는 cast
        static_cast<const TextBlock&> (*this)[position];
      };
  }

  ...

};
```


---

### 이것만은 잊지 말자!!
* const를 붙여 선언하던 컴파일러가 사용상의 에러를 잡아내는 데 도움을 줍니다. const는 어떤 유효범위에도 있는 객체에도 붙을 수 잇으며, 함수 매개변수 및 반환 타입에도 붙을 수 있으며, 멤버 함수에도 붙을  수 있습니다.
* 컴파일러 쪽에서 보면 비트수준 상수성을 지켜야 하지만, 우리는 논리적인 상수성을 사용해서 프로그래밍 해야 합니다.
* 상수 멤버 및 비상수 멤버 함수가 기능적으로 서로 똑같게 구현되어 있을 경우에는 코드 중복을 피하는 것이 좋은 데, 이 때 비상수 버전이 상수 버너을 호출하도록 만들어라.

---