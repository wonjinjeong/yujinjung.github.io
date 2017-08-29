---
layout: post
title: Effective C++_#9
description: "객체 생성 및 소멸 과정 중에는 절대로 가상 함수를 호출하지 말자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

# 객체 생성 및 소멸 과정 중에는 절대로 가상 함수를 호출하지 말자!!!!

* 호출 결과가 원하는 대로 돌아가지 않으며
* 실행이 되더라도 원하는 대로 돌아가지 않을 것이다

```cpp
// 모든 거래에 대한 기본 클래스
class Transaction {
public:
    Transaction();
    
    // 타입에 따라 달라지는 로그 기록
    virtual void logTransaction() const = 0;

    ...
};

Transaction::Transaction()
{
    ...
    logTransaction();
}


// 파생 class
class BuyTransaction: public Transaction {
public:
    // 이 타입에 맞는 거래내역 로깅 구현
    virtual void logTransaction() const;
}

// 아래 코드가 실행될 때 어떻게?
BuyTransaction b;
```
### 실행되는 순서
```cpp
BuyTransaction b;
```
파생 클래스이기 때문에 기본 클래스인 Transaction 의 생성자가 먼저 생성된다.  
Transaction 의 생성자 안의 logTransaction 호출 시에는 Transaction의 logTransaction가 호출된다.  
BuyTransaction 의 logTransaction 가 호출되지 않는 이유는 파생 클래스 전에 기본 클래스의 생성자가 호출되는 중이기 때문에  
기본 클래스의 생성자 호출이 끝나기 전까지는 파생 클래스로 내려가지 않기 때문이다.  
그래서 의도는 BuyTransaction 의 logTransaction를 호출하려 했을지 몰라도 정작 호출되어지는 함수는 Transaction의 logTransaction 함수 인 것이다.  

* BuyTransaction 객체의 기본 클래스 부분을 초기화하기 위해 Transaction 생성자가 실행되고 있는 동안에는, 그 객체의 타입이 Transaction 이다.  



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
- 