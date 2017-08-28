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



```cpp
```

```cpp
```

```cpp
```

```cpp
```
---

## 이것만은 잊지말자
- 소멸자에서는 예외가 빠져나가면 안 됩니다.  
만약 소멸자 안에서 호출된 함수가 예외를 던질 가능성이 있다면, 어떤 예외이든지 소멸자에서 모두 받아낸 후에 삼켜버리든지 프로그램을 끝내든지 해야합니다.
- 어떤 클래스의 연산이 진행되다가 던진 예외에 대해 사용자가 반응해야 할 필요가 있다면, 해당 연산을 제공하는 함수는 반드시 보통의 함수(즉, 소멸자가 아닌 함수)이어야 합니다.