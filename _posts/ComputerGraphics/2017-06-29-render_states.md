---
layout: post
title: Render States(수정중)
description:
modified: 2017-04-11
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/5.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

##

### Coverage
primitive 값들이 지나가는 pixel을 표시해주는 것.
> 지나가면 1.0
> 안지나가면 0.0
anti-aliasing? 하면 pixel의 값이 0.5로 표시될 수도

### Vertex Buffer Object


### Clipping
Scene에서 벗어난 영역을 그리지않는 것
9개 분면을 만들어서 chk

### Viewport
원점은 왼쪽 아래
GLsize - unsigined intager

### Culling
어디를 안 그릴건가?

open gl es 는 거대한 state machine이다. 3600개의 state
고정된 것(1.0일때)을 2.0으로 가면서 Vertex / Fragment shader로 바귀면서 좀 더 융통성있게

### Blending
새로 그리는 것을 어디다가 그릴지
뒤? 앞?
Source값과 destination 값을 어떻게 섞을 건지를 정함
해당 과정을 진행하는 function이 있음
Blending을 안하면 Source가 덮어쓴다(나중에 그린게 덮어씀)

f는 factor
op = operator + - 등
C는 color값
