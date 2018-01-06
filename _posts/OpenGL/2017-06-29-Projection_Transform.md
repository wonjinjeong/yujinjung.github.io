---
layout: post
title: Projection Transform
description:
modified: 2017-06-29
category: OpenGL
tags: [OpenGL, Projection transform, Trnasformation]
image:
  feature: background/Computer/5.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Perspective Projection 

<figure class = "half">
<a href="/images/CG/Perspective/perspective_projection1.png" title="Perspective_projection"><img src = "/images/CG/Perspective/perspective_projection1.png"></a>
<a href="/images/CG/Perspective/ortho_projection.png" title="Ortho_projection"><img src = "/images/CG/Perspective/ortho_projection.png"></a>
<figcapture><a href="https://stackoverflow.com/questions/14095796/opengl-es-not-working-as-desired/14125490" title = "Perspective_projection">Perspective_and_Orthogonal_projection</a></figcapture>
</figure>

<figure>
<a href="/images/CG/Perspective/perspective_projection.png" title="Perspective_projection"><img src = "/images/CG/Perspective/perspective_projection.png"></a>
<figcapture><a href="https://gamedev.stackexchange.com/questions/136007/projection-texture-mapping" title = "Perspective_projection">Perspective_projection</a></figcapture>
</figure>

- 거리에 따라 **Field Of View** 가 변하게 된다.
- 카메라로부터 보이게 되는 영역이 위의 그림처럼 사각뿔 형태를 띄게 된다.
- 사각뿔을 우리가 보고자 하는 영역에 따라 **Near-clip / Far-clip**을 진행한다.
- Far-clip의 경우 너무 멀리 지정하면 안되는 데 그 이유는 보통 255단계로 z-buffer를 할당하는 데 너무 멀게 잡을 경우 **z-buffer 가 잘 작동하지 않을 수 있기 때문**이다.
- clipping을 진행한 사각뿔을 **Frustrum**이라 한다.

<br/>

## 진행하는 방법

- 진행하는 방법은 여러 가지가 존재하며 주어진 parameter에 따라 적절히 사용한다.


1. Near-clip 좌표 / Far-clip좌표 를 이용한 방법
2. Near-clip 좌표 / FOV 를 이용한 방법? 등이 있다. 


--- 