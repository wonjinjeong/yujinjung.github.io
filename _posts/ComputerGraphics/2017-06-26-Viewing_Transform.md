---
layout: post
title: Viewing Transform
description:
modified: 2017-06-27
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/4.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Coordinate System & Degree of Freedom

## Coordinate System

1. Model Transform
  - 오른손 좌표계

2. 

<br/>

## Degree of Freedom [^1]

[^1]: [Six degrees of freedom, Wikipedia](https://en.wikipedia.org/wiki/Six_degrees_of_freedom)

<figure>
  <a href="/images/CG/viewTransform/6DOF.jpg"><img src="/images/CG/viewTransform/6DOF.jpg" alt=""></a>
  <figcaption><a href="https://en.wikipedia.org/wiki/Six_degrees_of_freedom" title="viewTransform">Six degrees of freedom</a></figcaption>
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
  <a href="/images/CG/viewTransform/viewTransform.jpg"><img src="/images/CG/viewTransform/viewTransform.jpg" alt=""></a>
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

## Camera 설정 [^3]

<figure>
  <a href="/images/CG/viewTransform/viewTransform2.jpg" title="viewTransform"><img src="/images/CG/viewTransform/viewTransform2.jpg" alt=""></a>
  <figcaption><a href="https://www.slideshare.net/deepakofheart/3-d-viewing" title="viewTransform">viewTransform</a></figcaption>
</figure>

<figure>
  <a href="/images/CG/viewTransform/modelviewcoordinates.png" title="Model View Coordinates"><img src="/images/CG/viewTransform/modelviewcoordinates.png" alt=""></a>
  <figcaption><a href="http://www.pling.org.uk/cs/cgv.html" title="viewTransform"> Model_View_Coordinates</a></figcaption>
</figure>

1. VRP
  - view reference point
  - 카메라의 위치

2. VPN
  - view plane normal vector
  - 카메라가 view plane 을 바라보는 방향의 normal vector

3. VUP
  - view up vector
  - 카메라에서 위의 방향으로 수직으로 뻗은 vector
  - 카메라의 **Roll** 여부를 측정하기 위해 사용한다.



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
	static float LoadTransformationMatrix[] =
	{
		1.0f, 0.0f, 0.0f, -p[0],
		0.0f, 1.0f, 0.0f, -p[1],
		0.0f, 0.0f, 1.0f, -p[2],
		0.0f, 0.0f, 0.0f, 1.0f,
	};

	// View TransformationMatrix
	mat4 temp;
	temp.setTransformationMatrix(LoadTransformationMatrix);

	ajouMultMatrix(temp);
}
```



[^2]: [OpenGL Viewing, 대갈장군's Blog](http://diehard98.tistory.com/entry/OpenGL-Viewing-Viewing-Modeling-Projection-Viewport-transformation)
[^3]: [3dviewing, slideshare](https://www.slideshare.net/deepakofheart/3-d-viewing)



--- 