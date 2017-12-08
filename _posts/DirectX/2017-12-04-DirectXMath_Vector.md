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

<!-- TOC -->

- [DirectXMath]({{site.url}}{{page.url}}#directxmath)
- [Vector 형식들]({{site.url}}{{page.url}}#vector-형식들)
    - [XMVECTOR]({{site.url}}{{page.url}}#xmvector)
    - [XMFLOATn]({{site.url}}{{page.url}}#xmfloatn)
    - [사용법]({{site.url}}{{page.url}}#사용법)
- [적재 및 저장 함수]({{site.url}}{{page.url}}#적재-및-저장-함수)
- [매개변수 전달]({{site.url}}{{page.url}}#매개변수-전달)
    - [XMVECTOR 인스턴스를 인수로 해서 함수를 호출할 때]({{site.url}}{{page.url}}#xmvector-인스턴스를-인수로-해서-함수를-호출할-때)
    - [위와 같이 전달할 수 있는 인수가 플랫폼/컴파일러 별로 다르다.]({{site.url}}{{page.url}}#위와-같이-전달할-수-있는-인수가-플랫폼컴파일러-별로-다르다)
        - [XMVECTOR 매개변수 설정 법]({{site.url}}{{page.url}}#xmvector-매개변수-설정-법)
    - [XM_CALLCONV]({{site.url}}{{page.url}}#xm_callconv)
    - [생성자 에서의 XMVECTOR 매개변수 설정 법]({{site.url}}{{page.url}}#생성자-에서의-xmvector-매개변수-설정-법)
- [상수 벡터]({{site.url}}{{page.url}}#상수-벡터)
    - [형식에 따른 사용]({{site.url}}{{page.url}}#형식에-따른-사용)
- [중복적재된 연산자들]({{site.url}}{{page.url}}#중복적재된-연산자들)
- [기타 상수 및 함수]({{site.url}}{{page.url}}#기타-상수-및-함수)
- [설정 함수와 벡터 함수]({{site.url}}{{page.url}}#설정-함수와-벡터-함수)
    - [설정 함수]({{site.url}}{{page.url}}#설정-함수)
    - [벡터 함수]({{site.url}}{{page.url}}#벡터-함수)
- [부동소수점 오차]({{site.url}}{{page.url}}#부동소수점-오차)

<!-- /TOC -->

---

# DirectXMath

- Windows 8 이상에서 Direct3D 응용 프로그램을 위한 표준적인 3차원 수학 라이브러리
- [SSE2]({{site.url}}{{page.url}}#sse2) 명령 집합을 사용한다.
- DirectXMath.h 를 포함시키면 사용이 가능하다.
- 추가적인 자료 형식을 사용하기 위하여 DirectXPackedVector.h 를 포함시켜야한다.
- x64에서는 따로 SSE2를 활성화 할 필요가 없지만 x86 플랫폼을 대상으로 할 때에는 SSE2를 활성화해야한다.
  - 프로젝트 속성 > 구성 속성 > C/C++ > 코드 생성 > 고급 명령 집합 사용

<br/>

> SSE : Streaming SIMD Extensions 2

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
  - Vector 계산 시 SIMD의 장점이 발휘되려면 **__m128을 사용해야한다.**
  - 그래서 **2차원 3차원 벡터에서도 이 형식을 쓰며, 쓰이지 않는 성분은 0으로 설정해서 무시한다.**
- **지역 변수나 전역 변수에** XMVECTOR를 사용한다
  - **16바이트 경계에 alignment** 되어야 하는데(128bit), 지역 변수와 전역 변수에서는 자동으로 이루어진다.

<br/>

## XMFLOATn
XMFLOAT2, XMFLOAT3, XMFLOAT4
- 클래스 자료 멤버에는 XMFLOATn을 사용한다.
  - 저장이니까?

- 계산을 할 때
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
1. **지역 변수나 전역 변수**에는 **XMVECTOR**를 사용
2. **클래스 자료 멤버**에는 **XMFLOATn**을 사용
3. **계산을 수행하기 전**에 적재 함수들을 이용해서 **XMFLOATn을 XMVECTOR로** 변환
4. **XMVECTOR** 인스턴스들로 **계산을 수행**
5. 저장 함수들을 이용해서 **XMVECTOR를 XMFLOATn으로** 변환 후 저장

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
  FXMVECTOR ScalingOrigin,      // First  / F
  float     ScalingOrientation,
  FXMVECTOR Scaling,            // Second / F 
  FXMVECTOR RotationOrigin,     // Third  / F
  float     Rotation,
  GXMVECTOR Translation);       // Fourth / G
```

<br/>

## XM_CALLCONV  

SSE/SSE2 레지스터 활용을 위한 호출 규약이 컴파일러에 따라 다를 수 있는 의존성을 없애기 위해 함수 이름 앞에 XM_CALLCONV 라는 호출 규약 지시자를 붙인다.
> 생성자에서는 사용하지 않는다.

<br/>

## 생성자 에서의 XMVECTOR 매개변수 설정 법

1. **FXMVECTOR** - 처음 세개
2. **CXMVECTOR** - 네번째 이상
  - G, H 사용하지 않음
3. CALLCONV 호출 규약 지시자 **사용 불가**

<br/>

> 위의 내용은 **입력 매개변수**에 해당되는 내용이며 출력의 경우 XMVECTOR는 SSE/SSE2 레지스터를 사용하지 않으므로, 그냥 XMVECTOR가 아닌 매개변수들과 동일하게 취급된다.

<br/>

---

# 상수 벡터

- const XMVECTOR에는 반드시 **XMVECTORF32**형식을 사용해야한다.  
- 중괄호 초기화 구문을 사용할 때 XMVECTORF32 사용

```cpp
static const XMVECTORF32 g_vHalfVector = { 0.5f, 0.5f, 0.5f, 0.5f };
static const XMVECTORF32 g_vHalfVector = { 0.0f, 0.0f, 0.0f, 0.0f };

XMVECTORF32 vRightTop = {
  vViewFrust RightSlope,
  vViewFrust TopSlope,
  1.0f, 1.0f
};

XMVECTORF32 vLeftBottom = {
  vViewFrust LeftSlope,
  vViewFrust BottomSlope,
  1.0f, 1.0f
};
```

XMVECTORF32는 16바이트 경계에 alignment되는 구조체로, XMVECTOR로의 변환 연산자들을 제공한다.    


<br/>

## 형식에 따른 사용
- floating point    - XMVECTORF32
- unsigned integer  - XMVECTORU32
- integer           - XMVECTORI32 
- byte              - XMVECTORU8


> [참고](https://msdn.microsoft.com/ko-kr/library/windows/desktop/ee421019(v=vs.85).aspx)

<br/>

---

# 중복적재된 연산자들

- XMVECTOR 에는 벡터 덧셈, 뺄셈, 스칼라 곱셈을 위해 overloading 된 여러 연산자가 있다.

```cpp
XMVECTOR  XM_CALLCONV operator+  (FXMVECTOR V);
XMVECTOR  XM_CALLCONV operator-  (FXMVECTOR V);

XMVECTOR& XM_CALLCONV operator+= (XMVECTOR& V1, FXMVECTOR V2);
XMVECTOR& XM_CALLCONV operator-= (XMVECTOR& V1, FXMVECTOR V2);
XMVECTOR& XM_CALLCONV operator*= (XMVECTOR& V1, FXMVECTOR V2);
XMVECTOR& XM_CALLCONV operator/= (XMVECTOR& V1, FXMVECTOR V2);

XMVECTOR& XM_CALLCONV operator*= (XMVECTOR& V, float S);
XMVECTOR& XM_CALLCONV operator/= (XMVECTOR& V, float S);

XMVECTOR  XM_CALLCONV operator+  (FXMVECTOR V1, FXMVECTOR V2);
XMVECTOR  XM_CALLCONV operator-  (FXMVECTOR V1, FXMVECTOR V2);
XMVECTOR  XM_CALLCONV operator*  (FXMVECTOR V1, FXMVECTOR V2);
XMVECTOR  XM_CALLCONV operator/  (FXMVECTOR V1, FXMVECTOR V2);
XMVECTOR  XM_CALLCONV operator*  (FXMVECTOR V, float S);
XMVECTOR  XM_CALLCONV operator*  (float S, FXMVECTOR V);
XMVECTOR  XM_CALLCONV operator/  (FXMVECTOR V, float S);


```

> [참고](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.directx_sdk.types.xmvector_methods(v=vs.85).aspx)

<br/>

---

# 기타 상수 및 함수

- DirectXMath 라이브러리에서는 여러 공식의 근삿값을 구할 때 유용한 다음과 같은 상수들을 정의한다.

```cpp
const float XM_PI       = 3.141592654f; // PI;
const float XM_2PI      = 6.283...;     // 2 * PI;
const float XM_1DIVPI   = 0.318...;     // 1 / PI;
const float XM_1DIV2PI  = 0.159...;     // 1 / (2 * PI);
const float XM_PIDIV2   = 1.570...;     // PI / 2;
const float XM_PIDIV4   = 0.785...;     // PI / 4;
```

<br/>

- radian - degree 각도 사이 변환을 위한 인라인 함수와
- 최솟값/최댓값 함수이다.

```cpp
inline float XMConvertToRadians(float fDegrees)
{ return fDegrees * (XM_PI / 180.0f); }
inline float XMConvertToDegrees(float fRadians)
{ return fRadians * (180.0f / XM_PI); }

template<typename T> inline T XMMin(T a, T b) { return (a < b) ? a : b; }
template<typename T> inline T XMMax(T a, T b) { return (a > b) ? a : b; }
```

<br/>

---

# 설정 함수와 벡터 함수

## 설정 함수

```cpp
// Return vector 0
XMVECTOR XM_CALLCONV XMVectorZero();

// Return vector (1, 1, 1, 1)
XMVECTOR XM_CALLCONV XMVectorSplatOne();

// Return vector (x, y, z, w)
XMVECTOR XM_CALLCONV XMVectorSet(float x, float y, float z, float w);

// Return vector (s, s, s, s)
XMVECTOR XM_CALLCONV XMVectorReplicate(float Value);

// SplatX, SplatY, SplatZ
// 각각 (Vx, Vx, Vx, Vx), (Vy, Vy, Vy, Vy), (Vz, Vz, Vz, Vz)
// 비슷하게 SplatOne - (1, 1, 1, 1)
XMVECTOR XM_CALLCONV XMVectorSplatX(FXMVECTOR V);
XMVECTOR XM_CALLCONV XMVectorSplatY(FXMVECTOR V);
XMVECTOR XM_CALLCONV XMVectorSplatZ(FXMVECTOR V);
```

<br/>

## 벡터 함수
- 3차원 벡터에 해당되는 함수이다.
- 3을 2나 4로 바꾸면 해당 차원의 함수가 호출된다.

```cpp
// Length, Length^2
XMVECTOR XM_CALCONV XMVector3Length(FXMVECTOR V);
XMVECTOR XM_CALCONV XMVector3LengthSq(FXMVECTOR V);

// 내적, 외적
XMVECTOR XM_CALCONV XMVector3Dot(FXMVECTOR V);
XMVECTOR XM_CALCONV XMVector3Cross(FXMVECTOR V);

// Normalize
// (Vx/length, Vy/length, Vz/length)
XMVECTOR XM_CALLCONV XMVector3Normalize(FXMVECTOR V);

// v에 수직인 벡터를 돌려준다.
XMVECTOR XM_CALLCONV XMVector3Orthogonal(FXMVECTOR V);

// v1과 v2 사이의 각도를 돌려준다.
XMVECTOR XM_CALLCONV XMVector3AngleBetweenVectors(FXMVECTOR V1, FXMVECTOR V2);

// Normal vector를 기준으로 평행한 벡터와 수직인 벡터를 반환한다.
void XM_CALLCONV XMVector3ComponentsFromNormal(
  XMVECTOR* pParallel,      // [out] parallel to Normal
  XMVECTOR* pPerpendicular, // [out] perpendicular to Normal
  FXMVECTOR V,
  FXMVECTOR Normal
  );

// v1 와 v2가 같은지 다른지
bool XM_CALLCONV XMVector3Equal(FXMVECTOR V1, FXMVECTOR V2);
bool XM_CALLCONV XMVector3NotEqual(FXMVECTOR V1, FXMVECTOR V2);

```


<br/>

---

# 부동소수점 오차

floating point number 들을 비교 할 때에는 부동소수점의 부정확함을 고려해야한다.  
그래서 두 부동소수점의 **상등을 판정** 할 때에는 두 수가 **근사적으로** 같은 지를 보아야한다.  
이를 위해 허용 오차로 사용할 epsilon을 정의하고 상수로서 코드 안에 정의해둔다.

```cpp
// epsilon
const float Epsilon = 0.001f;
bool Equals(float lhs, float rhs)
{
  return fabs(lhs - rhs) < Epsilon ? true : false;
}
```

<br/><br/>



---

해당 책을 참고하였습니다.

- 제 목 : DirectX 12를 이용한 게임프로그래밍 입문
- Link : [Slideshare](https://www.slideshare.net/wegra/directx-12-3d)