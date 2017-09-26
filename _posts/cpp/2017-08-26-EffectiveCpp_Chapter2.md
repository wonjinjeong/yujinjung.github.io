---
layout: post
title: Effective C++_#5
description: "C++가 은근슬쩍 만들어 호출해 버리는 함수들에 촉각을 세우자"
published: false
modified: 2017-08-27
tags: [C++]
---

# Chapter 1
## C++에 왔으면 C++의 법에 따릅시다

### 항목5 : [C++가 은근슬쩍 만들어 호출해 버리는 함수?]()
### 항목6 : [컴파일러가 만들어낸 함수가 필요없으면 확실히 이들의 사용을 금해버리자]()
### 항목7 : [다형성을 가진 기본 클래스에서는 소멸자를 반드시 가상 소멸자로 선언하자]()
### 항목8 : [예외가 소멸자를 떠나지 못하도록 붙들어 놓자]()
### 항목9 : [객체 생성 및 소멸 과정 중에는 절대로 가상 함수를 호출하지 말자]()
### 항목10 : [대입 연산자는 *this의 참조자를 반환하게 하자]()
### 항목11 : [operator=에서는 자기대입에 대한 처리가 빠지지 않도록 하자]()
### 항목12 : [객체의 모든 부분을 빠짐없이 복사하자]()

<br/>

---

# C++가 은근슬쩍 만들어 호출해 버리는 함수?

## 저절로 선언이 되는 함수
1. 복사 생성자
2. 복사 대입 연산자
3. 소멸자
4. 기본 생성자 (생성자가 없을 경우)
<br/>
이 함수들은 모두 public 멤버이며 inline 함수이다.

```cpp
class Empty { };

// 위와 같이 쓴 것은
// 아래와 같이 쓴 것과 같다
class Empty {
public:
    Empty () { ... }    // 기본 생성자
    Empty (const Empty& rhs) { ... }    // 복사 생성자
    ~Empty () { ... }   // 소멸자

    Empty& operator=(const Empty& rhs) { ... }  // 복사 대입 연산자
};

Empty e1;       // 기본 생성자 호출
Empty e2(e1);   // 복사 생성자 호출
e2 = e1;        // 복사 대입 연산자 호출
```


```cpp
template<class T>
class NamedObject {
public:
    NamedObject(std::string& name, const T& value);

    ..,

private:
    std::string& nameValue;
    const T objectValue;
}
```

위와 같이 멤버 변수에 참조자가 선언 되어 있다면 복사 대입 연산자가 제대로 동작하기 어렵다  
그 이유는 **C++ 참조자는 원래 자신이 참조하고 있는 것과 다른 객체를 참조할 수 없기** 때문이다  
그래서 멤버 변수에 참조자를 쓰려면 직접 복사 대입 연산자를 지정해 주어야한다.  
**상수 멤버**의 경우에도 수정을 허용하지 않기 때문에 비슷하게 동작한다  

* C++ 참조자는 원래 자신이 참조하고 있는 것과 다른 객체를 참조할 수 없다
<br/>

---

## 이것만은 잊지말자
- 컴파일러는 경우에 따라 클래스에 대해 기본 생성자, 복사 생성자, 복사 대입 연산자, 소멸자를 임시적으로 만들어 놓을 수 있습니다

<br>

---

<br>


# 컴파일러가 만들어낸 함수가 필요없으면 확실히 이들의 사용을 금해버리자

모든 객체 하나하나가 세상에 하나밖에 없다고 가정해보자  
그 상태에서는 복사가 진행되어서는 안된다(세상에 하나밖에 없기 때문에 복제가 불가능)  
하지만 cpp의 경우 [5에서 설명했듯이](https://github.com/UjinJung/ujinjung.github.io/blob/master/_posts/cpp/2017-08-26-EffectiveCpp_5.md) 기본생성자/복사생성자/복사대입연산자/소멸자 등이 자동으로 생성되기 때문에  
원치 않더라도 별다른 조치를 취하지 않을 경우에는 복사가 진행된다 
```cpp
class HomeForSale { ... };

HomeForSale h1;  
HomeForSale h2;

// h1을 복사하려 하지만 세상에 하나만 존재해야 하기 때문에 컴파일이 되면 안된다
// 하지만 의도와 다르게 복사 생성자는 선언하지 않게되면 자동으로 생성되기 때문에
// 복사가 진행된다
HomeForSale h3(h1);

// h2를 복사하려 하는데 복사가 되면 안된다
h1 = h2;
```

자동으로 생성되는 함수의 경우 모두 public 멤버가 된다  
하지만 꼭 public 멤버로 선언해야한다는 뜻은 아니다  
필요 없는 것은 private으로 선언을 하여 불필요한 호출을 방지하면 된다  
<br/>
**하지만** 이에 문제가 있는데 멤버 함수 및 friend 함수에 호출 당할 위험이 여전히 도사린다는 점이다  
이 문제점까지 막기 위해서는 **선언만 하고 정의는 하지 않는** 트릭을 쓰게 되면  
만약 해당 함수에 진입하더라도 그 순간 정의되지 않은 함수를 사용 중이라고 에러를 뿜어낼 것이다  
<br/>
이런 **멤버 함수를 private 멤버로 선언하고 일부러 정의하지 않는 방법**은 널리 퍼지면서 하나의 기법으로 굳어지기까지 했다. (복사 방지책으로 최고)

```cpp
class HomeForSale {
public:
    ...

private:
    ...
    HomeForSale(const HomeForSale&);    // 선언만 있다
    HomeForSale operator=(const HomeForSale&);
};
```

### 추가
링크 시점의 에러를 컴파일 시점의 에러로 옮기기  
따로 별도의 기본 클래스에 넣고 이것으로부터 HomeForSale을 파생시키면 된다  

```cpp
class Uncopyable {
protected:
    Uncopyable() { }    // 생성과 소멸을 허용
    ~Uncopyable() { }   

private:
    Uncopyable(const Uncopyable&);  // 복사는 방지
    Uncopyable& operator=(const Uncopyable&);
};

class HomeForSale: private Uncopyable {
    ...
};
```


---

## 이것만은 잊지말자
- 컴파일러에서 자동으로 제공하는 기능을 허용치 않으려면, 대응되는 멤버 함수를 private으로 선언한 후에 구현은 하지 않은 채로 두십시오. Uncopyable과 비슷한 기본 클래스를 쓰는 것도 한 방법입니다


<br>

---

<br>

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

<br>

---

<br>


# 예외가 소멸자를 떠나지 못하도록 붙들어 놓자

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

<br>

---

<br>


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
* 파생 클래스이기 때문에 기본 클래스인 Transaction 의 생성자가 먼저 생성된다.  
* Transaction 의 생성자 안의 logTransaction 호출 시에는 Transaction의 logTransaction가 호출된다.  
* BuyTransaction 의 logTransaction 가 호출되지 않는 이유는 파생 클래스 전에 기본 클래스의 생성자가 호출되는 중이기 때문에  
* 기본 클래스의 생성자 호출이 끝나기 전까지는 파생 클래스로 내려가지 않기 때문이다.  
* 그래서 의도는 BuyTransaction 의 logTransaction를 호출하려 했을지 몰라도 정작 호출되어지는 함수는 Transaction의 logTransaction 함수 인 것이다.  
* BuyTransaction 객체의 기본 클래스 부분을 초기화하기 위해 Transaction 생성자가 실행되고 있는 동안에는, 그 객체의 타입이 Transaction 이다.  

<br>

### 해결을 위한 방법
* 기본 클래스의 가상 함수를 비가상 함수로 변경하고
* 파생 클래스에서 비가상 함수로 변경한 함수에 **원하는 정보를 전달**해 줍니다.

```cpp
class Transaction {
public:
    explicit Transaction(const std::string& logInfo);
    void logTransaction(const std::string& logInfo) const; // 비가상 함수로 변경
};

Transaction::Transaction(const std::string& logInfo)
{
    logTransaction(logInfo);
}

class BuyTransaction {
public:
    BuyTransaction(params)
        : Transaction(createLogInfo(params))    // 로그 자료를 기본 클래스로 넘김
    { ... }

private:
    static std::string createLogInfo(params);
};
```

---

## 이것만은 잊지말자
- 생성자 혹은 소멸자 안에서 가상 함수를 호출하지 마세요.
- 가상 함수라고 해도, 지금 실행 중인 생성자나 소멸자에 해당되는 클래스의 파생 클래스 쪽으로는 내려가지 않으니까요

<br>

---

<br>


# 대입 연산자는 *this의 참조자를 반환하게 하자

### C++ 의 대입 연산은 여러 개가 사슬처럼 엮일 수 있는 성질이 있다(컴파일러 수업 때처럼)
```cpp
int x, y, z;
x = y = z = 15;

x = (y = (z = 15));
```

z 에 15 할당되고 그게 y에 할당되고 이런 식  
이렇게 할당되려면 대입 연산자가 대입이 끝나고 좌변 인자에 대한 참조자를 반환해주어야 한다.  
이런 구현은 일종의 관례로써 따라주는 것이 좋다.

```cpp
class Widget {
public:
    ...
    Widget& operator=(const Widget& rhs)
    {
        ...
        return *this;
    }
    ...

    // 모든 형태의 대입 연산자에서 가능해야 한다.
    // = 뿐만 아니라 
    // +=, -=, *=, /= 등에도 동일한 규약이 적용
    Widget& operator+=(const Widget& rhs)
    {
        ...
        return *this;
    }
};
```

---

## 이것만은 잊지말자
- 대입 연산자는 *this의 참조자를 반환하도록 만드세요

<br>

---

<br>


# operator= 에서는 자기 대입에 대한 처리가 빠지지 않도록 하자

## 자기대입

```cpp
class Widget { ... };

Widget w;
...
w = w;

// 예를 들면
// i 랑 j 가 같으면????
a[i] = a[j];

// 가리키는 대상 같으면????
*px = *py;
```
 
## 문제가 발생하는 코드

```cpp
class Bitmap { ... };

class Widget {
    ...

private:
    Bitmap *pb;
};

Widget& Widget::operator= (const Widget& rhs)
{
    delete pb;
    pb = new Bitmap(*rhs.pb);

    return *this;
}
// 근데 위에서 pb랑 rhs.pb랑 같은 거면??
// 자기대입 한거면??
```
* pb랑 rhs.pb랑 같은거면?!
* 동시에 delete 해버림..

### 예방 1
```cpp
Widget& Widget::operator= (const Widget& rhs)
{
    if(this == rhs) return *this;

    delete pb;
    pb = new Bitmap(*rhs.pb);

    return *this;
}
```

### 예방 2
```cpp
Widget& Widget::operator= (const Widget& rhs)
{
    Bitmap *pOrig = pb;
    pb = new Bitmap(*rhs.pb);
    delete pOrig;

    return *this;
}
```

---

## 이것만은 잊지말자

- operator= 을 구현할 때, 어떤 객체가 그 자신에 대입되는 경우를 제대로 처리하도록 만듭시다  
원본 객체와 복사 대상 객체의 주소를 비교해도 되고, 문장의 순서를 적절히 조정할 수도 있으며, 복사 후 맞바꾸기 기법을 써도 됩니다.
- 두 개 이상의 객체에 대해 동작하는 함수가 있다면, 이 함수에 넘겨지는 객체들이 사실 같은 객체인 경우에 정확하게 동작하는지 확인해보세요.

<br>

---

<br>


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

<br>

---

<br>

