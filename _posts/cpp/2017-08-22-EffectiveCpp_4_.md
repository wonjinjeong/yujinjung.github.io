---
layout: post
title: 'Effective C++_#4'
description: '객체를 사용하기 전에 반드시 그 객체를 초기화하자'
published: false
modified: 2017-07-05T00:00:00.000Z
tags:
    - C++
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
  
## 객체를 구성하는 데이터의 초기화 순서
  
  
### 기본 클래스는 파생 클래스보다 먼저 초기화된다
  
[Link](항목 12 )
  
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
  
- [기본 제공 타입의 객체는 직접 손으로 초기화합니다. 경우에 따라 저절로 되기도 하고 안되기도 하기 때문입니다.](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#객체를-사용하기-전에-반드시-그-객체를-초기화하자 )
- [생성자에서는 데이터 멤버에 대한 대입문을 생성자 본문 내부에 넣는 방법으로 멤버를 초기화하지 **말고**, **멤버 초기화 리스트를 즐겨 사용**합시다](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#해결-방법 )
[그리고 초기화 리스트에 데이터 멤버를 나열할 때는 클래스에 각 데이터 멤버가 **선언된 순서**와 똑같이 나열합시다](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#객체를-구성하는-데이터의-초기화-순서 )
- [여러 번역 단위에 있는 비지역 정적 객체들의 초기화 순서 문제는 피해서 설계해야 합니다. 비지역 정적 객체를 지역 정적 객체로 바꾸면 됩니다](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-22-EffectiveCpp_4.md#비지역-정적-객체의-초기화-순서-문제점 )
  
  
---
  