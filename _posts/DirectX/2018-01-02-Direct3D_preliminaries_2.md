---
layout: post
title: Direct3D 초기화 기본 지식 - 2
description: ""
published: true
modified: 2017-01-02
tags: [DirectX]
image:
  feature: background/Computer/4.jpg
  credit: unsplash
---

# Direct3D 12의 개요

## Direct3D
- Microsoft의 DirectX API에서 3D Graphics 연산과 출력을 담당하는 부분
- 응용 프로그램에서 GPU를 제어하고 프로그래밍하는데 쓰이는 저수준 그래픽 API
- **Direct3D라는 간접층과 HW Driver**가 응용 프로그램과 그래픽 하드웨어 사이에 서 Direct3D 명령들을 시스템의 GPU가 이해하는 기계어로 번역한다.

<br/>

---

#  COM
- DirectX의 프로그래밍 언어 독립성과 하위 호환성을 가능하게 하는 기술
- 필요한 COM Interface를 가리키는 포인터를 얻어서 사용
  - 함수를 이용해서
  - 다른 COM Interface의 Method를 이용해서
- 다 사용한 뒤에는 Release()를 호출 - [Link](https://msdn.microsoft.com/en-us/library/windows/desktop/ms682317(v=vs.85).aspx)
<br/>

## Microsoft::WRL::ComPtr
- WRL : Windows Runtime Library
- ComPtr : COM Smart Pointer / 범위를 벗어나면 자동으로 Release() 호출
- Method 몇가지
  - Get : 이 ComPtr과 연결된 인터페이스에 대한 포인터를 검색합니다
  - GetAddressOf : 이 ComPtr이 나타내는 인터페이스에 대한 포인터를 포함하는, ptr_ 데이터 멤버의 주소를 검색합니다
  - Reset : 이 ComPtr과 연관된 인터페이스의 포인터에 대한 모든 참조를 해제합니다
- [MSDN Link](https://msdn.microsoft.com/ko-kr/library/br244983.aspx)

<br/>

---

# Texture Format
- texel들을 저장하기 위해 설계된 구조
- 특정 형식의 자료 원소들만 담을 수 있다.
- 구체적인 형식은 DXGI_FORMAT이라는 열거형으로 저장
- Mipmap, filter
- [MSDN Link](https://msdn.microsoft.com/en-us/library/windows/desktop/ff476906(v=vs.85).aspx)

## 자료 원소 형식
- 구체적인 형식은 DXGI_FORMAT이라는 열거형으로 저장 (DirectX Graphics Infrastructure)
- Link : [DXGI](https://msdn.microsoft.com/en-us/library/windows/desktop/bb205075(v=vs.85).aspx) / [DXGI Format](https://msdn.microsoft.com/en-us/library/windows/desktop/mt426648(v=vs.85).aspx)

<br/>

* DXGI_FORMAT_R숫자G숫자B숫자A숫자_형식
* 앞에 DXGI_FORMAT_
* 이어서 RGBA 순으로 성분 개수 표현
  * 1개 R숫자 - 숫자 : 몇 비트
  * 2개 R숫자G숫자
  * 3개 R숫자G숫자B숫자
  * 4개 R숫자G숫자B숫자A숫자
* 형식
  * FLOAT : 부동소수점
  * UNORM, UINT : unsigned normalization[0,1], unsignend int
  * SNORM, SINT : signed normalization[-1,1], signend int
  * TYPELESS : 무형식(16비트)

<br/>

* DXGI_FORMAT_R32G32B32_FLOAT
* DXGI_FORMAT_R16G16B16A16_UNORM
* DXGI_FORMAT_R8G8B8A8_UINT

<br/>

---

# The Swap Chain and Page Flipping

- Back buffer 와 Front Buffer를 두고 Back buffer에는 화면에 표시되기 전 미리 그려놓는 용도로, Front buffer는 화면에 보여주는 형식을 띈다.
- Front를 보여주고 Back에서 그린 뒤 Back과 Front로 설정된 buffer의 역할을 바꾼다.(pointer만 바꾸는 식으로 Back buffer가 Front 역할을 하도록 / Front가 Back 역할을 하도록) 
- [OpenGL Double Buffering]({{site.url}}/EGL/#double-buffering)

<br/>

---

# Depth Buffering

- Image data 가 아닌 깊이 자료를 담고 있다.
- 범위 : 0.0 ~ 1.0
  - 0으로 갈 수록 view frustrum 상 가장 가까운 물체
  - 1으로 갈 수록 view frustrum 상 가장 먼 물체
- Depth Buffer와 Back Buffer는 **일대일 대응**이다.
- 물체 그리는 순서같은 것이나 작동 방식에 따라 잘못 작동할 여지가 있다.

<br/>

## 자료 원소 형식

- DXGI_FORMAT_D32_FLOAT_S8X24_UINT
  - D32_FLOAT : Depth buffer 32bit float
  - S8X24_UINT: Stencil buffer 8bit and (용도 없는 채우기(padding))용으로 24bit unsigned int
    - 이래서 테스트 문제에 byte padding 나온건가..? 뭐지
  ...

<br/><br/>

---

해당 책을 참고하였습니다.

- 제 목  : DirectX 12를 이용한 게임프로그래밍 입문
- Link  : [Slideshare](https://www.slideshare.net/wegra/directx-12-3d)
- Link2 : [DirectXMath Github](https://github.com/Microsoft/DirectXMath)