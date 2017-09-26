---
layout: post
title: Effective C++_#7
description: "다형성을 가진 기본 클래스에서는 소멸자를 반드시 가상 소멸자로 선언하자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---
# 다형성을 가진 기본 클래스에서는 소멸자를 반드시 가상 소멸자로 선언하자


```cpp
class TimeKeeper {
public:
    TimeKeeper();
    ~TimeKeeper();
    ...
};

class AtomicClock: public TimeKeeper { ... };
class WaterClock: public TimeKeeper { ... };
class WristWatch: public TimeKeeper { ... };

TimeKeeper* getTimeKeeper();    // factory function
```

### factory function
새로 생성된 파생 클래스 객체에 대한 기본 클래스 포인터를 반환하는 함수  
factory function이 소멸하는 과정에서 문제점이 발생한다

### 문제
1. 반환하는 포인터가 파생클래스(AtomicClock) 객체에 대한 포인터라는 점
2. 이 포인터가 가리키는 객체가 삭제될 때는 기본 클래스 포인터를 통해 삭제 된다는 점
3. 기본 클래스에 들어 있는 소멸자가 **비가상 소멸자**라는 점

소멸자가 호출되는 데 기본 클래스의 소멸자가 비가상 소멸자 이기 때문에 기본 클래스만 소멸되어지고  
파생 클래스의 경우 소멸되지 못할 뿐만 아니라 소멸자 자체가 호출되지 못한다  
해결 방법으로 아래처럼 **virtual**을 추가해 **가상 소멸자**로 만들어준다

```cpp
class TimeKeeper {
public:
    TimeKeeper();
    // virtual 을 통해 파생 클래스의 소멸까지 확실히 진행된다.
    virtual ~TimeKeeper();
    ...
};

TimeKeeper *ptk = getTimeKeeper();
...
delete ptk;
```

## 가상 소멸자를 대하는 자세
### 가상 소멸자가 없는 클래스
아 저 클래스는 기본 클래스로 쓰일 맘이 없구나  
기본 클래스로 쓰일 생각이 없다면 가상 소멸사를 선언하는 것은 안좋은 습관이다  

### 가상 소멸자가 있는 클래스
다형성을 가진 기본 클래스로 쓰이겠구나!



---

## 이것만은 잊지말자
- 다형성을 가진 기본 클래스에는 반드시 가상 소멸자를 선언해야 합니다. 즉, 어떤 클래스가 가상 함수를 하나라도 갖고 있으면, 이 클래스의 소멸자도 가상 소멸자이어야 합니다.
- 기본 클래스로 설계되지 않았거나 다형성을 갖도록 설계되지 않은 클래스에는 가상 소멸자를 선언하지 말아야 합니다.