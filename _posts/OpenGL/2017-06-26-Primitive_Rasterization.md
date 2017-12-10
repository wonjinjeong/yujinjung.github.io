---
layout: post
title: Primitives and Rasterization 
description:
modified: 2017-06-26
tags: [OpenGL]
image:
  feature: background/Computer/1.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Primitive Assembly

<figure class="half">
  <a href="/images/CG/Primitive_Rasterization/primitive_assembly_stage.png"><img src="/images/CG/Primitive_Rasterization/primitive_assembly_stage.png" alt=""></a>
  <a href="/images/CG/Primitive_Rasterization/primitive_assembly_stage_2.png"><img src="/images/CG/Primitive_Rasterization/primitive_assembly_stage_2.png" alt=""></a>
  <figcaption><a href="/images/CG/Primitive_Rasterization/primitive_assembly_stage.png" title="primitive_assembly_stage"> primitive_assembly_stage</a></figcaption>
</figure>

<br/>

# Clipping

<figure>
  <a href="/images/CG/Primitive_Rasterization/clipping.png"><img src="/images/CG/Primitive_Rasterization/clipping.png" alt=""></a>
  <figcaption><a href="/images/CG/Primitive_Rasterization/clipping.png" title="">clipping</a></figcaption>
</figure>

- Viewing Volume
- Clipping Triangle / Lines / Points
  - Fully inside - no clipping
  - Fully outside - discard
  - Partially inside - modify triangle

<br/>

# Perspective & Viewport

- 나중에

<br/>

---

# Rasterization

<figure>
  <a href="/images/CG/Primitive_Rasterization/rasterization.png"><img src="/images/CG/Primitive_Rasterization/rasterization.png" alt=""></a>
  <figcaption><a href="/images/CG/Primitive_Rasterization/rasterization.png" title=""> </a></figcaption>
</figure>

# Culling

## glFrontFace 

```cpp
void glFrontFace(GLenum dir)
```

* dir
  - GL_CW / GL_CCW(default)
  - 어떤 방식으로 앞면을 결정할지 정의한다.

<br />

## glCullFace

```cpp
void glCullFace(GLenum mode)
```

* mode
  - GL_FRONT / GL_BACK(default) / GL_FRONT_AND_BACK
  - 어느 면을 그리지 않을 것인지 정한다.
  - 인자로 들어가는 값을 그리지 않는다.

* 예
  ```cpp
  glEnable(GL_CULL_FACE);
  glCullFace(GL_BACK);
  ```
  - 앞면만 그린다.

  ```cpp
  glEnable(GL_CULL_FACE);
  glCullFace(GL_FRONT);
  ```
  - 안쪽 면만 그린다.

<br/>

## glEnable / glDisable

```cpp
glEnable(GL_CULL_FACE);
glDisable(GL_CULL_FACE);
```

- Culling을 활성화 하거나 비활성화한다.

<br/>

## Hidden Surface Removal

<figure>
  <a href="/images/CG/Primitive_Rasterization/zbuffer.png"><img src="/images/CG/Primitive_Rasterization/zbuffer.png" alt=""></a>
  <figcaption><a href="/images/CG/Primitive_Rasterization/zbuffer.png" title=""> z-buffer</a></figcaption>
</figure>

- 불투명한 물체일 경우 우리는 앞에 있는 물체만을 볼 수 있기 때문에 뒤에 있는 물체의 경우 그리지 않아도 무방하다.
- 카메라와 물체 간의 거리(z-buffer)를 측정하여 멀리 있는 물체는 그리지 않는다.