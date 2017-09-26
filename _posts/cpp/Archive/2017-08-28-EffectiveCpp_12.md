---
layout: post
title: Effective C++_#12
description: "객체의 모든 부분을 빠짐없이 복사하자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

# 객체의 모든 부분을 빠짐없이 복사하자
- 기본으로 제공되어지는 복사 함수를 사용할 경우에는 멤버 변수 추가 유무에 상관없이 **모두 빠짐없이** 복사를 진행하여 준다.  
- 하지만 복사 함수를 새로 정의하는 순간 그러한 것이 사라지고 모든 것을 사용자가 직접 복사를 해주어야 한다.
- 때문에 까먹고 빼먹게 되어 **부분복사**로 진행되는 경우가 있다.
- 그렇기 때문에 부분 복사가 되지 않도록 각별히 유의하여 모두 복사 할 수 있도록 한다.

## 파생 클래스 일 경우에는 기본 클래스 초기화까지 신경을 쓰자
```cpp
// priority 는 멤버함수
// PriorityCustomer = Derived / Customer = Base
PriorityCustomer::PriorityCustomer(const PriorityCustomer& rhs)
: Customer(rhs), priority(rhs.priority)
{
    log~~;
}

PriorityCustomer& PriorityCustomer::operator=(const PriorityCustomer& rhs)
{
    log ~;

    Customer::operator=(rhs);
    priority = rhs.priority;

    return *this;
}
```

---

## 이것만은 잊지말자
- 객체 복사 함수는 주어진 객체의 모든 데이터 멤버 및 모든 기본 클래스 부분을 빠트리지말고 복사해야합니다.
- 클래스의 복사 함수 두 개를 구현할 때 한쪽을 이용해서 다른 쪽을 구현하려는 시도는 절대로 하지 마세요.  
그 대신, 공통된 동작을 제3의 함수에다 분리해 놓고 양쪽에서 이것을 호출하게 만들어서 해결합시다.