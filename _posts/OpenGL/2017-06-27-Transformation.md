---
layout: post
title: Transformation
description:
modified: 2017-06-28
tags: [OpenGL]
image:
  feature: background/Computer/3.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Objectives
  1. Rotation
  2. Translation
  3. Scaling

  * Point 나 Vector를 이동해서 변형한다.

<figure>
  <a href="/images/CG/Transformation/linear_transform.png"><img src="/images/CG/Transformation/linear_transform.png" alt=""></a>
  <figcaption><a href="/images/CG/Transformation/linear_transform.png" title="linear_transform"> linear_transform</a></figcaption>
</figure>

<br/>

---

# Transition 

- 이동을 진행한다.

## 과정

- p 에서 q로 이동할 때 (  <sup>t</sup> = transpose 상태)
* p = [x y z 1]<sup>t</sup>
* q = [x' y' z' 1]<sup>t</sup>
* dx dy dz = 이동 거리

* q = p + d~

## Translation Matrix

- q = Tp
- q = p + d~
- [q<sub>x</sub>] = [p<sub>x</sub> + d<sub>x</sub>] 
- [q<sub>y</sub>] = [p<sub>y</sub> + d<sub>y</sub>] 
- [q<sub>z</sub>] = [p<sub>z</sub> + d<sub>z</sub>] 

- T

  [1 0 0 d<sub>x</sub>]  
  [0 1 0 d<sub>y</sub>]  
  [0 0 1 d<sub>z</sub>]  
  [0 0 0  1 ]   

<br/>

---

# Rotation

- 각 축에 대한 rotation
- z축에 대한 rotation
- q<sub>x</sub> = p<sub>x</sub>cosA - p<sub>y</sub>sinA
- q<sub>y</sub> = p<sub>x</sub>sinA + p<sub>y</sub>cosA
- q<sub>z</sub> = p<sub>z</sub>

## Rotation Matrix

###  X축 Y축 Z축

<figure>
<a href="/images/CG/Transformation/2drotate.png"><img src="/images/CG/Transformation/2drotate.png"></a>
<figcaption>[Link] <a href="https://www.slideshare.net/champ_yen/opengl-es-2x-programming-introduction">2D Rotate</a></figcaption>
</figure>

<figure class="third">
  <a href="/images/CG/Transformation/rotatex.gif"><img src = "/images/CG/Transformation/rotatex.gif" alt=""></a>
  <a href="/images/CG/Transformation/rotatey.gif"><img src = "/images/CG/Transformation/rotatey.gif" alt=""></a>

  <figcaption><a href="/images/CG/Transformation/rotatex.gif" title="">Rotate about x y z axis</a> </figcaption>
</figure>


<figure>
<a href="/images/CG/Transformation/rotatez.gif"><img src = "/images/CG/Transformation/rotatez.gif" alt=""></a>
<figcaption><a href="/images/CG/Transformation/2drotate.png">2D Rotate</a></figcaption>
</figure>

<br/>

---

# Scaling

- 각 축에 대해서 늘리거나 줄인다.
- q<sub>x</sub> = s<sub>x</sub>x
- q<sub>y</sub> = s<sub>y</sub>x
- q<sub>z</sub> = s<sub>z</sub>x

<figure>
  <a href="/images/CG/Transformation/scaling.gif"><img src = "/images/CG/Transformation/scaling.gif" alt=""></a>
  <figcaption><a href="/images/CG/Transformation/scaling.gif" title="">Scaling</a> </figcaption>
</figure>
