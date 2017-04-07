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

## Creating and Compiling Shader
<br />
### glCreateShader
```cpp
GLuint glCreateShader(GL_VERTEX_SHADER | GL_FRAGMENT_SHADER)
```
> ### Input

- Vertex Shader "or" Fragment Shader

> ### Output

- 각각 Input으로 들어간 Shader가 return 되어나온다.

```cpp
void glDeleteProgram  (GLunit shader)
```

```cpp
void glShaderSource(GLuint shader, GLsizei count, const char ** string, const GLint* len)
```

```cpp
void glCompileShader(GLuint shader)
```

```cpp
void glGetShaderiv(GLuint shader, GLenum pname, GLint* params)

//p-name - GL_COMPILE_STATUS, GL_DELETE_STATUS, GL_INFO_LOG_LENGTH, GL_SHADER_SOURCE_LENGTH, GL_SHADER_TYPE
//params - pointers to interstorage location for the result of the query
```

```cpp
void glGetShaderInfoLog(GLuint shader, GLsizei maxLen, GLsizel *len, GLchar * infolog)
```

## Creating and Compiling Shader
<br />
```cpp
GLunit glCreateProgram(void)
```

```cpp
void glDeleteProgram(GLuint program)
```

```cpp
void glAttachShader(GLuint program, GLuint shader)
```

```cpp
glDettachShader(GLuint program, GLuint shader)
```

```cpp
glLinkProgram(GLunit program)
```

```cpp
void glGetProgramiv(GLuint program, GLenum pname, GLint params)

```

```cpp
void glGetProgramInfoLog
```

```cpp
```

```cpp
```
 void void glAttachShader glAttachShaderglAttachShaderglAttachShader glAttachShader (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader)
 void void glDettachShader glDettachShader glDettachShader glDettachShader (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader) (GLuint program, shader)
 void void void glLinkProgram glLinkProgram glLinkProgram glLinkProgram glLinkProgram glLinkProgram (GLuint program) (GLuint program) (GLuint program) (GLuint program) (GLuint program) (GLuint program) (GLuint program)
 void void glGetProgramiv glGetProgramiv glGetProgramiv glGetProgramiv (GLuint program, (GLuint program, (GLuint program, (GLuint program, (GLuint program, (GLuint program, GLenum GLenum pname pname , GLint , GLint , GLint params params )

