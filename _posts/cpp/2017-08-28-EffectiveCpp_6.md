---
layout: post
title: Effective C++_#6
description: "컴파일러가 만들어낸 함수가 필요없으면 확실히 이들의 사용을 금해버리자"
published: false
modified: 2017-08-29
tags: [C++]
---

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
