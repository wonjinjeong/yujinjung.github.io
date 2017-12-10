---
layout: post
title: Shader Programming - Uniform, Attribute
description:
modified: 2017-06-26
tags: [OpenGL]
image:
  feature: background/Computer/6.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Uniform

* rendering하는 동안 변하지 않는 변수이다.
* 마치 C 언에에서의 **global extern variable**의 개념이라고 생각하면 된다.
* OpenGL application으로 부터 값을 전달받을 수 있다.
* **vertex shader와 fragment shader에서 모두 공유하여** 사용하는 것이 가능하지만, 값을 읽는 것만 가능하다.
* 예) light position, light color

## glGetActiveUniform

```cpp
void glGetActiveUniform(GLuint prog, GLuint index, GLsizei bufSize, 
                        GLsizei *len, GLint* size, GLenum *type, char *name)
```

<br />

## glGetUniformLocation

```cpp
GLint glGetUniformLocation(GLuint program, const char * name)

int textureLocation = glGetUniformLocation(shaderProgram, "s_texture");
int matrixView = glGetUniformLocation(shaderProgram, "view");
int projView = glGetUniformLocation(shaderProgram, "proj");
```
- Uniform 변수의 Location 값을 받아오는 함수이다.
- shader code 에 대응되는 변수의 Location을 반환한다.

<br />

## glUniform...

```cpp
void glUniform{1234}{if}( int location, T value );
void glUniform{1234}{if}v( int location, sizei count, T value );
void glUniformMatrix{234}fv( int location, sizei count, boolean transpose, const float *value );

glUniformMatrix4fv(projView, 1, GL_FALSE, projTransform.getTransformationMatrix());
glUniform4fv(loc, 1, light_in.amb);
glUniform1f(loc, material_in.shine);
loc = glGetUniformLocation(shaderProgram, "s_texture");
glUniform1i(loc, 1);
```
- 여러 가지의 형태가 존재한다.
- Uniform 으로 들어가는 형태에 따라 다른 함수를 사용하여 Uniform 변수를 전달한다.

<br />

---

# Attribute

* uniform과 마찬가지로 OpenGL application으로 부터 값을 전달받을 수 있다.
* **vertex shader에서만** 사용이 가능하다.
* **vertex별로 다른 값을** 가질 수 있다.
* uniform과 마찬가지로 값을 읽는 것만 가능하다.
* 예) vertex position, vertex normal

## glBindAttribLocation

```cpp
const unsigned int VertexArray = 0;
const unsigned int textureCoord = 1;
const unsigned int NormalArray = 2;

glBindAttribLocation(shaderProgram, VertexArray, "myVertex");
glBindAttribLocation(shaderProgram, textureCoord, "inCoord");
glBindAttribLocation(shaderProgram, NormalArray, "aNormal");
```
* 2번째 Parameter에 vertex shader에 대응되는 3번째 parameter의 Location 을 할당한다. 

<br />

## glEnableVertexAttribArray

```cpp
glEnableVertexAttribArray(NormalArray);
glEnableVertexAttribArray(textureCoord);
glEnableVertexAttribArray(VertexArray);
```

<br />

## glVertexAttribPointer

```cpp
glVertexAttribPointer(VertexArray, 3, GL_FLOAT, GL_FALSE, 32, 0);
glVertexAttribPointer(textureCoord, 2, GL_FLOAT, GL_FALSE, 32, (void*)12);
glVertexAttribPointer(NormalArray, 3, GL_FLOAT, GL_FALSE, 32, (void*)20);
```
* VBO에 저장된 값을 각각 Location에 저장한다.
* Buffer를 여러개 두고 해도 되고 한 곳에 넣고 위와 같이 잘라서 저장하는 것도 가능하다.


<br />

---

# varying
* vertex shader에서 fragment shader로 값을 **전달하는 경우**에 사용한다.
* vertex shader와 fragment shader에 **모두 선언하여 사용한다.**
* varying type 변수에 vertex shader에서 각 vertex 별로 값을 기록하면, fragment shader에서는 perspective correctly interpolated value를 얻게된다.
* vertex shader에서는 값을 읽고 쓰는 것이 모두 가능하지만, fragment shader에서는 값을 읽는 것만 가능하다.
* 예) texture coordinates, normal vectors for per fragment Phong shading, vertex color, light value for Gouraud shading

---

* 참고
    * [http://wanochoi.com/?p=1114](http://wanochoi.com/?p=1114)