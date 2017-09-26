---
layout: post
title: Effective C++_#2
description: "#define 대신 const, enum, inline을 사용하자"
published: false
modified: 2017-08-22
tags: [C++]
image:
  feature: background/Computer/3.jpg
  credit: unsplash
---

# #define을 쓰려거든 const, enum, inline을 떠올리자
<br/>
## #define 보다 const, enum, inline을 사용해야하는 이유

```cpp
#define ASPECT_RATIO 123.456

const double Aspect_Ratio = 123.456;
```

#### 위 코드를 기준으로 설명하면

### Error msg 로 인한 혼란
#define 같은 것의 경우 preprocessor 단계에서 전부 치환이 되어버리기 때문에 ASPECT_RATIO가 Compiler가 쓰는 Symbol table에 들어가지 않는다.  
그래서 만약 **Error가 발생 할 시에** Error msg로 ASPECT_RATIO가 아닌 123.456을 표시하기 때문에 혼란이 생길 수 있다.  

### 코드 크기
위의 코드처럼 부동소수점 실수 타입일 경우에는 const를 이용하는 것이 코드의 크기를 줄일 수 있다.  
매크로의 경우 preprocessor 단계에서 다 치환하기 대문에 코드안에 실수들의 사본이 **나온 만큼 들어간다.**  
const를 이용한 경우 해당 상수가 여러번 쓰이더라도 사본은 **딱 한 개**만 생긴다.  

<br/>

---

## #define을 상수로 교체할 때 조심할 점

<br/>

### const pointer를 정의하는 경우
상수를 정의 할 때 대개 **헤더파일**에 넣는 것이 보통이기 때문에 **포인터**는 꼭 const로 선언해주고, 포인터가 가리키는 대상까지 const로 선언하는 것이 보통이다.

```cpp
// 포인터와 가리키는 대상까지 const로 선언
const char* const authorName = "Yujin";
const std::string authorName("Yujin");
```

<br/>

### 클래스 멤버로 상수를 정의하는 경우
어떤 상수의 범위를 **class**로 한정하고자 할 때 사용한다.  
그 상수의 사본 개수가 **한 개를 넘지 못하게** 하고 싶다면 **static** member로 만들어야한다.

```cpp
/////////////////
// header file //
/////////////////

class GamePlayer {
private:                      
  static const int NumTurns = 5; // 상수 선언
  int scores[NumTurns];          // 상수를 사용하는 부분
};

//////////////
// cpp file //
//////////////

// NumTurns 의 "정의"
const int GamePlayer::NumTurns;
```

위의 NumTurns 는 '정의가 아니라' **'선언 된 것입니다'**  
Static member로 만들어지는 정수류(각종 정수 타입, char, bool 등) 타입의 클래스 내부 상수는 선언만 해도 무방하다.  
<br/>

#### 클래스 상수의 주소가 필요하거나 정의를 요구하는 경우
클래스 상수의 주소를 구하거나 주소를 구하지 않는데도 컴파일러가 잘못 정의되어서 주소를 요구하는 경우 별도의 정의를 제공해야 한다.  
클래스 상수의 정의는 *헤더 파일이 아닌* **구현 파일에 둡니다**  
정의에는 상수의 초기값이 있으면 안되는 데, 왜냐하면 클래스 상수의 초기값은 해당 상수가 선언된 시점에서 바로 주어지기 때문입니다.  
-> 그 말은 NumTurns는 선언될 당시에 바로 초기화 된다는 뜻이다.

```cpp
// cpp file

const int GamePlayer::NumTurns;
```

#### 오래된 컴파일러를 사용시
초기값
  - 대개 Static member가 선언된 시점에서 초기값을 주는 것이 맞지 않다고 판단한다.
  - 이 때에는 어쩔 수 없이 정의할 때에 초기값을 주도록한다.
클래스 컴파일 도중에 클래스 상수의 값이 필요할 때

```cpp
// Header FILE
class GamePlayer {
private:
  static const int Numturns;
  int scores[NumTurens];
};

// CPP FILE
const int GamePlayer::Numturns = 5;
```

<br/>

---

## ENUM HACK

위의 초기값 문제로 선언이 아닌 정의할 때에 초기값을 주게되면 클래스 컴파일 도중에 Static member의 값을 요청할 수 없어 오류가 발생 할 수 있다.  
이에 대한 해결책으로 enum hack(나열자 둔갑술)이 있다.

```cpp
// enum hack

class GamePlayer {
private:
  enum { NumTurns = 5 };

  int scores[NumTurns];
}
```

* 동작 방식이 const보다는 #define에 가깝다  
  * 만약 선언한 정수 상수를 가지고 다른 사람이 주소를 얻는다거나 참조자를 쓰는 것이 마음에 들지 않는다면 enum을 사용하는 것이 매우 효율적입니다.  
  // 왜냐하면 enum은 설명한 것처럼 const보다 #define에 가깝기 때문이며  
  // **const의 주소 값을 취하는 것은 합법이지만 #define 및 enum의 주소값을 취하는 것은 불법입니다**
* 정수 상수를 가지고 주소를 얻거나 참조자를 쓰는 것이 싫다면 사용할 것  
* 상당히 많은 코드에서 쓰이고 있기 때문에 눈에 익힐 것  

<br/>

---

## 매크로 함수
매크로 함수도 가급적 인라인 템플릿 함수로 변경 할 것

```cpp
#define CALL_WITH_MAX(a,b) f((a) > (b) ? (a) : (b))

int a = 5; b = 0;
// a가 두번 증가
CALL_WITH_MAX(++a, b);
// a가 한번 증가
CALL_WITH_MAX(++a, b+10);
```

다음과 같은 이상 현상이 발생하기 때문에 매크로 사용을 최대한 지양한다.  
이런 기존 매크로의 효율을 그대로 유지하면서 정규 함수의 모든 동작 방식 및 타입 안전성까지 완벽히 취할 수 있는 방법은 인라인 함수에 대한 템플릿을 준비하는 것이다.

```cpp
template<typename T>
inline void callWithMax(const T& a, const T& b) {
  f ( a > b ? a : b);
}
```

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

## 객체를 구성하는 데이터의 초기화 순서

### 기본 클래스는 파생 클래스보다 먼저 초기화된다
[Link](항목 12)

### 클래스 데이터 멤버는 그들이 선언된 순서대로 초기화된다.
**선언**된 순서대로
```cpp
class ABEntry {
public:

  ABEntry(const std::string& name, const std::string& address, const std::list<PhoneNumber>& phones)

private:
  std::string theName;
  std::string theAddress;
  std::list<PhoneNumber> thePhones;
  int numTimesConsulted;
}
```

다음과 같을 경우 어쩌다가 멤버 초기화 리스트에 넣어진 순서와 위 선언된 순서가 다르더라도 항상 theName부터 순서대로(선언된 순서대로) 초기화가 진행된다.

---

## 비지역 정적 객체의 초기화 순서 문제점

별개의 번역 단위에서 정의된 비지역 정적 객체들의 초기화 순서는 '정해져 있지 않다'

### 정적 객체
  - 자신이 생성된 시점부터 프로그램이 끝날 때까지 살아 있는 객체를 일컫는다.
  - 그러므로 스택 및 힙 기반 객체는 애초부터 정적 객체가 될 수 없다

### 정적 객체의 범주
  - 전역 객체
  - 네임스페이스 유효범위에서 정의된 객체
  - 클래스 안에서 static으로 선언된 객체
  - 함수 안에서 static으로 선언된 객체
  - 그리고 파일 유효범위에서 static으로 정의된 객체

### 구분
  - 지역 정척 객체
    - 함수 안에 있는 정적 객체
  - 비지역 정적 객체

### 소멸 시기
  - 프로그램이 끝날 때 자동으로 소멸

### 번역 단위
  - 컴파일을 통해 하나의 목적 파일을 만드는 바탕이 되는 소스코드
  - #include하는 파일까지 합쳐서 하나의 번역 단위

### 그래서 문제가 뭐?
  - 별도로 컴파일 된 소스 파일이 두 개 이상 있으며
  - 각 소스파일에 비지역 정적 객체(전역 객체, 네임스페이스에 잇는 객체, 클래스/파일에 있는 정적 객체)가 한 개 이상 들어 있는 경우에는 어떻게?
  <br/>
  = 한쪽 번역 단위에 있는 비정적 객체의 초기화가 진행되면서 다른쪽 번역 단위에 있는 비정적 객체가 사용되는 데, 불행히도 다른 번역 단위에 있는 객체가 초기화되어 있지 않을지도 모른 다는 것.
    - 이유는 아까 말했듯이 별개의 번역 단위에서 정의된 비지역 정적 객체들의 초기화 순서는 '정해져 있지 않다'라는 사실 때문에


그래서 이에 대한 해결책으로 비지역 정적 객체에 대응해서 함수를 하나씩 만들고 그 안에 각 객체를 static으로 선언하는 것입니다

<br/>
그렇게 선언을 진행한 후에 참조자로 반환을 하게 됩니다

<br/>
이런 방식을 통해서 비지역 정적 객체를 "지역 정적 객체"로 변경할 수 있게 됩니다
<br/>
지역 정적 객체는 함수 호출 중에 그 객체의 정의에 최초로 닿았을 떄 초기화가 진행되도록 만들어져있다.
그러므로 비지역 정적 객체를 지역 정적 객체로 다루는 함수를 만들어 초기화가 진행되는 것을 명확히 해야 나중에 문제가 생기지 않는다.

```cpp
// 문제가 되는 코드
class FileSystem {
public:
  ...
  std::size_t numDisk() const;
  ...
};
extern FileSystem tfs;

// 위 객체를 사용하는 객체(클래스)
class Directory {
public:
    Directory( params );
    ...
};
// Constructor
Directory::Directory( params )
{
    ...
    std::size_t disks = tfs.numDisks(); // tfs 사용
    ...
}

// 문제 발생
Directory tempDir(params);

// tfs가 tempDir보다 먼저 초기화되지 않으면 문제 발생
// 소스 파일도 다르고 제작자도 다르다고 가정
// 해결을 위해서는 확실하게 tfs가 tempDir보다 먼저 초기화 된다는 것을 알아야함
```

```cpp
// 해결이 된 코드
class FileSystem { ... };  // 이전과 같은 코드

FileSystem& tfs()
{
    // 지역 정적 객체(함수 안)를 정의하고 초기화 함
    // 그리고 참조자로 반환을 함
    static FileSystem fs;
    return fs;
}

class Directory { ... };

Directory::Directory( params )
{
    ...
    // tfs 를 tfs()로 변경
    std::size_t disks = tfs().numDisks();
    ...
}

Directory& tempDir()
{
    static Directory td;
    return td;
}
```

---

## 이것만은 잊지 말자!
- [기본 제공 타입의 객체는 직접 손으로 초기화합니다. 경우에 따라 저절로 되기도 하고 안되기도 하기 때문입니다.](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#객체를-사용하기-전에-반드시-그-객체를-초기화하자)
- [생성자에서는 데이터 멤버에 대한 대입문을 생성자 본문 내부에 넣는 방법으로 멤버를 초기화하지 **말고**, **멤버 초기화 리스트를 즐겨 사용**합시다](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#해결-방법)
[그리고 초기화 리스트에 데이터 멤버를 나열할 때는 클래스에 각 데이터 멤버가 **선언된 순서**와 똑같이 나열합시다](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#객체를-구성하는-데이터의-초기화-순서)
- [여러 번역 단위에 있는 비지역 정적 객체들의 초기화 순서 문제는 피해서 설계해야 합니다. 비지역 정적 객체를 지역 정적 객체로 바꾸면 됩니다](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#비지역-정적-객체의-초기화-순서-문제점)


---
