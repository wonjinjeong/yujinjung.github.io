---
layout: post
title: Effective C++_#13
description: ""
published: false
modified: 2017-08-29
tags: [C++]
---

##### 빨리 끝내자

---

# 자원
Resource(자원)이란 사용을 일단 마치고 난 후엔 시스템에 돌려주어야하는 모든 것을 말한다.  

## 종류
1. 동적 할당한 메모리
2. 파일 서술자
3. 뮤텍스 잠금
4. GUI에서의 font와 brush 등등..
5. 데이터베이스 연결
6. 네트워크 소켓

가져와서 **다** 썼으면 해제해야, 즉 놓아 주어야 한다 는 뜻

---

## 이것만은 잊지말자
- 