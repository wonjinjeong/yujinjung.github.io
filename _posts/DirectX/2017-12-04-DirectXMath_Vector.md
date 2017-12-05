---
layout: post
title: DirectXMath Vector 
description: ""
published: true
modified: 2017-12-05
tags: [DirectX]
image:
  feature: background/Computer/1.jpg
  credit: unsplash
---

DirectXMath
- Windows 8 이상에서 Direct3D 응용 프로그램을 위한 표준적인 3차원 수학 라이브러리
- [SSE2]({{site.url}}{{page.url}}/#sse2) 명령 집합을 사용한다.
- DirectXMath.h 를 포함시키면 사용이 가능하다.
- 추가적인 자료 형식을 사용하기 위하여 DirectXPackedVector.h 를 포함시켜야한다.
- x64에서는 따로 SSE2를 활성화 할 필요가 없지만 x86 플랫폼을 대상으로 할 때에는 SSE2를 활성화해야한다.
  - 프로젝트 속성 > 구성 속성 > C/C++ > 코드 생성 > 고급 명령 집합 사용

<br/>

---

# Vector 형식들

## XMVECTOR
- 크기 - 32bit 부동소수점 값 네 개(x, y, z, w)(128bit)
- SIMD 명령 하나로 위의 4개의 값을 한번에 처리가 가능

```cpp
typedef __m128 XMVECTOR;
```
- 차원에 상관없이 Vector일 경우 __m128을 사용한다.
  - Vector 계산 시 SIMD의 장점이 발휘되려면 **벡터가 __m128이 되어야한다.**
  - 그래서 **2차원 3차원 벡터에서도 이 형식을 쓰며, 쓰이지 않는 성분은 0으로 설정해서 무시한다.**
- **지역 변수나 전역 변수에는 XMVECTOR를 사용한다**
  - 16바이트 경계에 alignment 되어야 하는데, **지역 변수** 와 **전역 변수**에서는 자동으로 이루어진다.

<br/>

## XMFLOATn
XMFLOAT2, XMFLOAT3, XMFLOAT4
- 클래스 자료 멤버에는 XMFLOATn을 사용한다.
  - 저장이니까?

- 계산을 할 때 **
  - 계산을 할 때 XMFLOATn을 사용하면 SIMD의 장점을 취할 수 없기 때문에 XMVECTOR 형식으로 변환하여 사용한다.
  - 적재 함수
    - XMFLOATn -> XMVECTOR
  - 저장 함수
    - XMVECTOR -> XMFLOATn

- 정의  

```cpp
struct XMFLOAT2
{
  float x;
  float y;

  XMFLOAT2() {}
  XMFLOAT2(float _x, float _y) : x(_x), y(_y) {}
  explicit XMFLOAT2(_In_reads_(2) const float *pArray)
    : x(pArray[0]), y(pArray[1])
  
  XMFLOAT2& operator= (const XMFLOAT2& Float2)
  { x = Float2.x; y = Float2.y; return *this; }
};

struct XMFLOAT3
{
  float x;
  float y;
  float z;

  XMFLOAT3() {}
  XMFLOAT3(float _x, float _y, float _z) : x(_x), y(_y), z(_z) {}
  explicit XMFLOAT3(_In_reads_(3) const float *pArray)
    : x(pArray[0]), y(pArray[1]), z(pArray[2]) {}
  
  XMFLOAT3& operator= (const XMFLOAT3& Float3)
  { x = Float3.x; y = Float3.y; z = Float3.z; return *this; }
};

struct XMFLOAT4
{
  float x;
  float y;
  float z;
  float w;

  XMFLOAT4() {}
  XMFLOAT4(float _x, float _y, float _z, float _w)
    : x(_x), y(_y), z(_z), w(_w) {}
  explicit XMFLOAT4(_In_reads_(4) const float *pArray)
    : x(pArray[0]), y(pArray[1]), z(pArray[2]), w(pArray[3]) {}
  
  XMFLOAT4& operator= (const XMFLOAT4& Float4)
  { x = Float4.x; y = Float4.y; z = Float4.z; w = Float4.w; return *this; }
};
```

<br/>

## 사용법
1. 지역 변수나 전역 변수에는 **XMVECTOR**를 사용
2. 클래스 자료 멤버에는 XMFLOATn을 사용
3. 계산을 수행하기 전에 적재 함수들을 이용해서 XMFLOATn을 XMVECTOR로 변환
4. XMVECTOR 인스턴스들로 계산을 수행
5. 저장 함수들을 이용해서 XMVECTOR를 XMFLOATn으로 변환

<br/>

---


# 적재 및 저장 함수

```cpp
// 적재 및 저장
// XMFLOATn 을 XMVECTOR 에 적재
XMVECTOR XM_CALLCONV XMLoadFloatn(const XMFLOATn *pSource);

// XMVECTOR 값을 XMFLOATn 에 저장
void XM_CALLCONV XMStoreFloatn(XMFLOATn *pDestination, FXMVECTOR V);

// -----------------------
// 특정 성분 하나만 읽거나 변경
// 읽기
float XM_CALLCONV XMVectorGetX(FXMVECTOR V);
float XM_CALLCONV XMVectorGetY(FXMVECTOR V);
float XM_CALLCONV XMVectorGetZ(FXMVECTOR V);
float XM_CALLCONV XMVectorGetW(FXMVECTOR V);

// 쓰기
XMVECTOR XM_CALLCONV XMVectorSetX(FXMVECTOR V, float x);
XMVECTOR XM_CALLCONV XMVectorSetY(FXMVECTOR V, float y);
XMVECTOR XM_CALLCONV XMVectorSetZ(FXMVECTOR V, float z);
XMVECTOR XM_CALLCONV XMVectorSetW(FXMVECTOR V, float w);
```

<br/>

---


# 매개변수 전달

## XMVECTOR 인스턴스를 인수로 해서 함수를 호출할 때  

**효율성을 위해** XMVECTOR 값이 **스택**이 아니라 **SSE/SSE2 레지스터**를 통해서 함수에 전달되게 해야 한다.  

<br/>

## 위와 같이 전달할 수 있는 인수가 플랫폼/컴파일러 별로 다르다.  

이런 플랫폼/컴파일러에 대한 의존성을 없애기 위해서  
XMVECTOR 매개변수에 대해 FXMVECTOR, GXMVECTOR, HXMVECTOR, CXMVECTOR를 사용  

### XMVECTOR 매개변수 설정 법

1. **FXMVECTOR** - 처음 세개
2. **GXMVECTOR** - 네번째
3. **HXMVECTOR** - 다섯째 여섯째
4. **CXMVECTOR** - 그 이상

FFFGHHCCCCCCCCC..  
5. 다른 형식의 매개 변수에 영향을 받지 않는다.

```cpp
inline XMMATRIX XM_CALLCONV XMMatrixTransformation2D(
  FXMVECTOR ScalingOrigin,      // First
  float     ScalingOrientation,
  FXMVECTOR Scaling,            // Second 
  FXMVECTOR RotationOrigin,     // Third 
  float     Rotation,
  GXMVECTOR Translation);       // Fourth
```

<br/>

## XM_CALLCONV  

SSE/SSE2 레지스터 활용을 위한 호출 규약도 컴파일러에 따른 의존성을 없애기 위해 함수 이름 앞에 XM_CALLCONV 라는 호출 규약 지시자를 붙인다.

<br/>

## 생성자 에서의 XMVECTOR 매개변수 설정 법

1. **FXMVECTOR** - 처음 세개
2. **CXMVECTOR** - 네번째 이상
  - G, H 사용하지 않음
3. CALLCONV 호출 규약 지시자 **사용 불가**

---

### 용어 정리
#### SSE2 
- Streaming SIMD Extensions 2

#####

---

해당 책을 참고하였습니다.

- 제 목 : DirectX 12를 이용한 게임프로그래밍 입문
- Link : [Slideshare](https://www.slideshare.net/wegra/directx-12-3d)