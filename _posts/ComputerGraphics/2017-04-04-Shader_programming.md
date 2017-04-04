---
layout: post
title: Shader Programming
description:
modified: 2017-04-04
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/marius-masalar-132751.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

### glCreateShader
```cpp
GLuint glCreateShader(GL_VERTEX_SHADER | GL_FRAGMENT_SHADER)
```
> ### Input

- Vertex Shader "or" Fragment Shader

> ### Output

- 각각 Input으로 들어간 Shader가 return 되어나온다.
