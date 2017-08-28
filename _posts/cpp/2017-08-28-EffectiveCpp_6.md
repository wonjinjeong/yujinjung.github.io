---
layout: post
title: Effective C++_#6
description: "컴파일러가 만들어낸 함수가 필요없으면 확실히 이들의 사용을 금해버리자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

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
