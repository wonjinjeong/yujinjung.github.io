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

```cpp
class Point {
public:
    Point(int xCoord, int yCoord);
    ~Point();

private:
    int x, y;
};
```

```cpp
```

```cpp
```

---

## 이것만은 잊지말자
- 다형성을 가진 기본 클래스에는 반드시 가상 소멸자를 선언해야 합니다. 즉, 어떤 클래스가 가상 함수를 하나라도 갖고 있으면, 이 클래스의 소멸자도 가상 소멸자이어야 합니다.
- 기본 클래스로 설계되지 않았거나 다형성을 갖도록 설계되지 않은 클래스에는 가상 소멸자를 선언하지 말아야 합니다.