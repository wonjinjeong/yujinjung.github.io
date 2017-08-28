---
layout: post
title: Effective C++_#8
description: "예외가 소멸자를 떠나지 못하도록 붙들어 놓자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

```cpp
class Widget {
public:
    ...
    ~Widget() { ... }
};

void doSomething()
{
    std::vector<Widget> v;
    ...
}
```
### 소멸자에서 예외가 발생한다면?
만약 Widget이 열개가 있다면 첫번째 것을 실행하던 도중 소멸자에서 예외가 발생하더라도  
뒤에 9개 또한 소멸되어야하기 때문에 계속 소멸자가 호출될 것이다.  
소멸자로 인해 줄줄이 예외가 발생할 것이고 C++은 감당할 수 없는 지경까지 오게 될 것이다.  
#### 때문에 C++은 예외를 내보낼 수 있는 소멸자를 좋아하지 않는다

```cpp
class DBConnection {
public:
    ...
    static DBConnection create();

    // 연결을 닫고 연결이 실패하면 예외를 던지는 클래스
    void close();
};
```
사용자가 DBConnection 객체에 대해 close를 직접 호출해야한다.  
사용자가 까먹을 수도 있기 떄문에 자원 관리 클래스를 따로 만들고 그 클래스의 소멸자에서 close를 호출하게 하는 것이 좋다.  

```cpp
// DBConnection 객체를 관리하는 클래스
class DBConn {
public:
    ...
    ~DBConn()
    {
        db.close();
    }

private:
    DBConnection db;
};


{
    // DBConnection 객체를 생성하고 이것을 DBConn 객체로 넘겨서 관리를 맡긴다
    DBConn dbc(DBConnection::create());

    // DBConn 인터페이스를 통해 DBConnection 객체를 사용한다.
}
// 블록 끝.
// DBConn 객체가 소멸됨에 따라 DBConnection 객체에 대한 close 함수의 호출이 자동으로 이루어진다.
```

close 함수만 잘 성공하면 무난한 코드이지만  
close 함수에서 예외가 발생한다면?  

---
<br>

## 소멸자에서 예외가 발생했을 떄 피하는 방법
### close에서 예외가 발생하면 프로그램을 바로 끝낸다(대개 abort를 호출)

```cpp
DBConn::~DBConn()
{
    try{ db.close(); }
    catch (...) {
        close 호출이 실패했다는 로그;
        std::abort();
    }
}
```
<br>

### close에서 예외가 발생하면 예외를 삼켜버린다

```cpp
DBConn::~DBConn()
{
    try{ db.close(); }
    catch (...) {
        close 호출이 실패했다는 로그;
    }
}
```
예외 삼키기는 그렇게 좋은 방법은 아니다.  
어떤 예외가 발생했는지 모르고 지나칠 수도 있기 때문이다.  
하지만 불완전한 프로그램 종료 및 미정의 동작보다는 분명히 나은 방법이기에 사용을 부득이 하게 된다면  
프로그램이 확실히 신뢰성 있게 실행이 지속되어야 할 것이다.  

<br>

다른 방법도 있지만 일단 여기서 중요한 점은
* 소멸자에서 예외가 발생하지 않도록 해야하고 발생하더라도 예외를 잘 처리해주어야 한다는 것이다.

---

## 이것만은 잊지말자
- 소멸자에서는 예외가 빠져나가면 안 됩니다.  
만약 소멸자 안에서 호출된 함수가 예외를 던질 가능성이 있다면, 어떤 예외이든지 소멸자에서 모두 받아낸 후에 삼켜버리든지 프로그램을 끝내든지 해야합니다.
- 어떤 클래스의 연산이 진행되다가 던진 예외에 대해 사용자가 반응해야 할 필요가 있다면, 해당 연산을 제공하는 함수는 반드시 보통의 함수(즉, 소멸자가 아닌 함수)이어야 합니다.