---
layout: post
title: Effective C++_#13
description: "자원 관리에는 객체가 그만!!"
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

# 자원
Resource(자원)이란 사용을 일단 마치고 난 후엔 시스템에 돌려주어야하는 모든 것을 말한다.  
dk
## 종류
1. 동적 할당한 메모리
2. 파일 서술자
3. 뮤텍스 잠금
4. GUI에서의 font와 brush 등등..
5. 데이터베이스 연결
6. 네트워크 소켓

가져와서 **다** 썼으면 해제해야, 즉 놓아 주어야 한다 는 뜻

---

# 자원 관리에는 객체가 그만
## 예 - 투자를 모델링 해주는 클래스 라이브러리
### Base Class 
Investment

### Derived Class
투자 클래스가 파생되어 있다

### 가정
Investment에서 파생된 클래스의 객체를 사용자가 얻어내는 용도로 팩토리 함수 만을 사용한다.

```cpp
// Investment 클래스 계통에 속한 클래스의 객체를 동적 할당하고
// 그 포인터를 반환합니다.
// 이 객체의 해제는 호출자 쪽에서 직접해야 한다.
Investment* createInvestment();

void f()
{
    // 팩토리 함수 
    Investment *pInv = createInvestment();

    ...
    delete pInv;
}

void f()

```




<br/>

---

## 이것만은 잊지말자
- 