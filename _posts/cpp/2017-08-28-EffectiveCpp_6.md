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
}
```


---

## 이것만은 잊지말자
- 컴파일러에서 자동으로 제공하는 기능을 허용치 않으려면, 대응되는 멤버 함수를 private으로 선언한 후에 구현은 하지 않은 채로 두십시오. Uncopyable과 비슷한 기본 클래스를 쓰는 것도 한 방법입니다
