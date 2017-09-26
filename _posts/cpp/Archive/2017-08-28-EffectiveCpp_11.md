---
layout: post
title: Effective C++_#11
description: "operator=에서는 자기대입에 대한 처리가 빠지지 않도록 하자"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

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