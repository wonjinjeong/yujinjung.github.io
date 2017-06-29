---
layout: post
title: Viewing Transform
description:
modified: 2017-06-29
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/4.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Coordinate System & Degree of Freedom

## Coordinate System (작성중)

1. Model Transform
  - 오른손 좌표계

2. 

<br/>

## Degree of Freedom [^1]

[^1]: [Six degrees of freedom, Wikipedia](https://en.wikipedia.org/wiki/Six_degrees_of_freedom)

<figure>
  <a href="/images/CG/viewTransform/6DOF.jpg" title="Six degrees of freedom"><img src="/images/CG/viewTransform/6DOF.jpg" alt=""></a>
  <figcaption><a href="https://en.wikipedia.org/wiki/Six_degrees_of_freedom" title="Six degrees of freedom">Six degrees of freedom</a></figcaption>
</figure>

1. Change position
  * forward/backward (surge)
  * up/down (heave)
  * left/right (sway)

2. Change in orientation through rotation
  * roll (x axis)
  * pitch (y axis)
  * yaw (z axis)


<br/>

---

# Frames in OpenGL

## Two transform matrices
  1. **VIEW MATRIX**
    - 카메라를 이동시켜서 보는 것
  2. **MODEL MATRIX**
    - 사물을 이동시켜서 보는 것
  
## 간단한 개념 설명

<figure>
  <a href="/images/CG/viewTransform/viewTransform.jpg" title="View Transform with Camera"><img src="/images/CG/viewTransform/viewTransform.jpg" alt=""></a>
  <figcaption><a href="/images/CG/viewTransform/viewTransform.jpg" title="viewTransform"> viewTransform</a></figcaption>
</figure>

OpenGL에서 Viewing을 설정하는 것은 카메라로 사진을 찍는 것과 유사하다고 한다.[^2]

### Viewing Transform

- 찍을 곳을 향해 카메라를 설치한다. 
- (OpenGL) 카메라의 위치를 설정한다. 
- 첫번째 사진

<br/>

### Modeling Transform

- 찍을 곳을 비춘 이후에 사람 또는 사물을 카메라 안으로 들어오거나 나가도록 조정한다.
- (OpenGL) 모델의 위치를 설정한다. **
- 두번째 사진

<br/>

### Projection Trasform

- 카메라 초점을 맞춰준다.
- (OpenGL) Projection 을 통해 View Plane을 설정해 필요없는 부분은 잘라내고 버린다.

<br/>

### Viewport Transformation

- 사진 현상하기
- (OpenGL) 모니터에 그려내기

<br/>

---

# Viewing Transform

## Camera 설정 [^3] [^4] [^5]

<figure class = "half">
  <a href="/images/CG/viewTransform/viewTransform2.jpg" title="View_Transform"><img src="/images/CG/viewTransform/viewTransform2.jpg" alt=""></a>
  <a href="/images/CG/viewTransform/modelviewcoordinates.png" title="Model View Coordinates"><img src="/images/CG/viewTransform/modelviewcoordinates.png" alt=""></a>
  <figcaption>[Link] <a href="https://www.slideshare.net/deepakofheart/3-d-viewing" title="viewTransform">View_Transform</a></figcaption>
   <figcaption>[Link] <a href="http://www.pling.org.uk/cs/cgv.html" title="viewTransform">Model_View_Coordinates</a></figcaption>
</figure>

<figure>
  <a href="/images/CG/viewTransform/ModelSpace.png" title="Model_Space"><img src="/images/CG/viewTransform/ModelSpace.png" alt=""></a>
  <figcaption>[Link] <a href="http://www.codinglabs.net/article_world_view_projection_matrix.aspx" title="Model_Space">Three teapots each one in its own model space</a></figcaption>
</figure>

<figure>
  <a href="/images/CG/viewTransform/WorldSpace.png" title="World_Space"><img src="/images/CG/viewTransform/WorldSpace.png" alt=""></a>
  <figcaption>[Link] <a href="http://www.codinglabs.net/article_world_view_projection_matrix.aspx" title="World_Space">Three teapots set in World Space</a></figcaption>
</figure>

<figure>
  <a href="/images/CG/viewTransform/WorldToView.png" title="World_To_View_Space"><img src="/images/CG/viewTransform/WorldToView.png" alt=""></a>
  <figcaption>[Link] <a href="http://www.codinglabs.net/article_world_view_projection_matrix.aspx" title="World_To_View_Space">On the Left two teapots  and a camera in World Space; On the right everything is transformed into View Space (World Space is represented only to help visualize the transformation)</a></figcaption>
</figure>

1. VRP
  - View Reference Point
  - 카메라의 위치
  - Viewing Coordinate System의 Origin
  - p = [x y z 1]<sup>t</sup>

2. VPN
  - View Plane Normal vector
  - 카메라가 view plane 을 바라보는 방향의 normal vector
  - 사물의 위치(at) - 카메라의 위치(eye) 를 normalize
  - Viewing Coordinate System의 첫번째 축 (n axis)
  - n = [n<sub>x</sub> n<sub>y</sub> n<sub>z</sub> 0]<sup>t</sup>
  - n<sub>x</sub><sup>2</sup> + n<sub>y</sub><sup>2</sup> + n<sub>z</sub><sup>2</sup> = 1 

3. VUP
  - View UP vector
  - 카메라에서 위의 방향으로 수직으로 뻗은 vector
  - 카메라의 **Roll** 여부를 측정하기 위해 사용한다.
  - Viewing Coordinate System의 두번째 축 (v axis)
  - v = [v<sub>x</sub> v<sub>y</sub> v<sub>z</sub> 0]<sup>t</sup>
  - v<sub>x</sub><sup>2</sup> + v<sub>y</sub><sup>2</sup> + v<sub>z</sub><sup>2</sup> = 1 

4. u
  - VPN과 VUP의 벡터곱이다.
  - u = n X v
  - Viewing Coordinate System의 세번째 축 (u axis)

<br/>

## Calculate Viewing Matrix

<figure class = "half">
  <a href="/images/CG/viewTransform/qview.gif" title="R_T_matrix"><img src="/images/CG/viewTransform/qview.gif" alt=""></a>
  <a href="/images/CG/viewTransform/mview.gif" title="Mview_matrix"><img src="/images/CG/viewTransform/mview.gif" alt=""></a>
  <figcaption><a href="/images/CG/viewTransform/qview.gif" title="Calculate_Viewing_Matrix">Calculate_Viewing_Matrix</a></figcaption>
</figure>

* Model / View / Proj 의 곱해지는 순서가 중요하다.
* MVP 라고 부르기도 하며 Model View Proj 순으로 곱해진다.
* 곱 표현시 앞에 나오는 것이 나중에 곱해지게 되어서 다음과 같이 표현된다.
  - q<sub>proj</sub> = M<sub>proj</sub> * M<sub>view</sub> * M<sub>model</sub> * q<sub>world</sub>

<br/>

## Default Case
1. Camera Position (VRP)
  - p = (0, 0, 0)
2. View Plane Normal (VPN)
  - n = (0, 0, 1)
3. View UP vector (VUP)
  - u = (0, 1, 0)

<br/>

## Source Code

  ```cpp
  void viewSetLookat(const float eyex, const float eyey, const float eyez,
    const float atx, const float aty, const float atz,
    const float upx, const float upy, const float upz) {
    float p[3] = { 0, };
    float n[3] = { 0, };
    float u[3] = { 0, };
    float v[3] = { 0, };

    p[0] = eyex; p[1] = eyey; p[2] = eyez;
    n[0] = atx - eyex; n[1] = aty - eyey; n[2] = atz - eyez;
    float I = sqrt(n[0] * n[0] + n[1] * n[1] + n[2] * n[2]);
    n[0] /= I;
    n[1] /= I;
    n[2] /= I;

    u[0] = upy * n[2] - upz * n[1];
    u[1] = upz * n[0] - upx * n[2];
    u[2] = upx * n[1] - upy * n[0];
    I = sqrt(u[0] * u[0] + u[1] * u[1] + u[2] * u[2]);
    u[0] /= I;
    u[1] /= I;
    u[2] /= I;

    v[0] = n[1] * u[2] - n[2] * u[1];
    v[1] = n[2] * u[0] - n[0] * u[2];
    v[2] = n[0] * u[1] - n[1] * u[0];

    // R matrix
    mat4TransformationMatrix[0] = u[0];
    mat4TransformationMatrix[1] = v[0];
    mat4TransformationMatrix[2] = n[0];
    mat4TransformationMatrix[3] = 0.0f;

    mat4TransformationMatrix[4] = u[1];
    mat4TransformationMatrix[5] = v[1];
    mat4TransformationMatrix[6] = n[1];
    mat4TransformationMatrix[7] = 0.0f;

    mat4TransformationMatrix[8] = u[2];
    mat4TransformationMatrix[9] = v[2];
    mat4TransformationMatrix[10] = n[2];
    mat4TransformationMatrix[11] = 0.0f;

    mat4TransformationMatrix[12] = 0.0f;
    mat4TransformationMatrix[13] = 0.0f;
    mat4TransformationMatrix[14] = 0.0f;
    mat4TransformationMatrix[15] = 1.0f;

    // Load TransformationMatrix
    // T matrix
    static float LoadTransformationMatrix[] =
    {
      1.0f, 0.0f, 0.0f, -p[0],
      0.0f, 1.0f, 0.0f, -p[1],
      0.0f, 0.0f, 1.0f, -p[2],
      0.0f, 0.0f, 0.0f, 1.0f,
    };

    // View TransformationMatrix
    // Assign TransformationMatrix in temp for multiply
    mat4 temp;
    temp.setTransformationMatrix(LoadTransformationMatrix);

    // R * T -> M(view) matrix
    ajouMultMatrix(temp);
  }
  ```



[^2]: [OpenGL Viewing, 대갈장군's Blog](http://diehard98.tistory.com/entry/OpenGL-Viewing-Viewing-Modeling-Projection-Viewport-transformation)
[^3]: [3dviewing, slideshare](https://www.slideshare.net/deepakofheart/3-d-viewing)
[^4]: [Computer Graphics and Visualisation](http://www.pling.org.uk/cs/cgv.html)
[^5]: [Article - World, View and Projection Transformation Matrices](http://www.codinglabs.net/article_world_view_projection_matrix.aspx)


--- 