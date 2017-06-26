---
layout: post
title: Introduction of Graphics Pipeline
description:
modified: 2017-04-02
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/1.jpg
  credit: unsplash
---
##### ** 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

## GPU Pipeline[^1]
> <i>http://alleysark.tistory.com/264의 내용입니다.</i>
>
>  GPU는 CPU와 달리 병렬처리를기본으로 한다.
>
> 사용자와 상호작용하기 위하여 '충분히' 빠른 속도로 한장면 한장면 렌더링 되어야하는 데 CPU에 이와 같은 일을 할당하기에는 낭비에 가깝다.
>
> 때문에 나온 것이 GPU이며, GPU는 화면 렌더링을 보다 빠르고 효율적으로 처리하기 위해 단일 능력은 조금 떨어지는 연산 코어를 굉장히 많이 때려박은 유닛이다. 따라서 각 픽셀 값을 병렬적으로 한번에 처리한다. "그려!"
> 처음 시작할 때는 넣는 과정에 있기때문에 점점 빨라진다.

> Vertex Shader 는 vertex를 적절히 배치해주어야하는 데 그 작업을 진행해준다.
> Rasterization은 추상적인 3D 장면으로부터 화면 픽셀 값 계산을 위한 정보를 결정하는 일을 한다.
> Fragment Shader가 각 픽셀 값을 계산한다.

[^1]: http://alleysark.tistory.com/264

![OpenGL ES 2.0 Pipeline](/images/CG/opengles_20_pipeline.gif)
![OpenGL Pipeline](/images/CG/gl-pipeline.png)
###### [https://glumpy.github.io/modern-gl.html](https://glumpy.github.io/modern-gl.html)

> ### Vertex Shader

- 각 Vertex 들의 위치를 적절히 배치해준다.

> ### Primitive Assembly

-  점, 선, 삼각형을 어떻게 그릴 것인지 정하고 그림

> ### Rasterizer

- 색깔은 Vertex에만 색깔이 칠해진 상태로 각각의 상태(점, 선, 삼각형)에 따라 Fragments 을 할당해준다.

> ### Fragment Shader

- Rasterizer 단계에서 할당된 Fragments에 색깔을 정해 그려준다.


> ### Per Fragment Operation 
![Per_Fragment Operation](/images/CG/per_fragment.jpg)

> #### Depth test

- 거리를 측정하여 가까운 것만 그리고 멀리 있는 것은 지우거나 그리지않는다.
- 참고 : z-buffer testing

> #### Stencil test

 - 모양 자 생각할 것
 - 0이라고 할 때 안그리고 1이라고 할 때 그리고?

> #### Pexel Ownership test

 - 이 Pixel 여러 Thread가 있다면 내 Thread에서 사용하는 것인지, 이 화면의 부분이 나의 것인지 chk

> #### Scissor test

 - 화면의 원하는 부분을 잘라서 사용

> #### Blending

 - Draw 명령마다 그려진 결과물을 어떻게 섞을 것인가를 결정
 - 투명도를 어떻게 섞을 것인가? 같은

<br />

---
