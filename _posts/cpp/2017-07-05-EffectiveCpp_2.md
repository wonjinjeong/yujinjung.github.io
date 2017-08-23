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

##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

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
