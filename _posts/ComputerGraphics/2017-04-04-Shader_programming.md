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
/*> ### Input

- Vertex Shader "or" Fragment Shader

> ### Output

- 각각 Input으로 들어간 Shader가 return 되어나온다.
*/
void glDeleteShader (GLunit shader)
// Shaer들이 들어간다.

void glShaderSource(GLuint shader, GLsizei count, const char ** string, const GLint* len)
// Shader, size, program source code

void glCompileShader(GLuint shader)
// Shader를 컴파일.

void glGetShaderiv(GLuint shader, GLenum pname, GLint* params)
//p-name - GL_COMPILE_STATUS, GL_DELETE_STATUS, GL_INFO_LOG_LENGTH, GL_SHADER_SOURCE_LENGTH, GL_SHADER_TYPE
//params - pointers to interstorage location for the result of the query
// 컴파일 된 결과물을 params에 따라 정보를 얻는다.

void glGetShaderInfoLog(GLuint shader, GLsizei maxLen, GLsizel *len, GLchar * infolog)
// Shader의 상태를 보여준다.
```
<br />
<br />
## Creating and Linking a Program
<br />
```cpp
GLunit glCreateProgram(void)
// Program 일단 하나 생성하고
void glDeleteProgram(GLuint program)
void glAttachShader(GLuint program, GLuint shader)
// Shader를 또 Attach 한다
// 첨부포일 보내 듯이 첨부해서
// Vertex, Fragment Shader 총 두번 일어나고
glDettachShader(GLuint program, GLuint shader)
// Shader를 떼어내고 싶으면 DettachShader
glLinkProgram(GLunit program)
// Attach 후에 Link를 하게 되면 GPU에서 Run 할 준비가 된다.
void glGetProgramiv(GLuint program, GLenum pname, GLint params)
void glGetProgramInfoLog(GLuint program, GLsizei maxLen, GLsizei *len, GLchar *  infolog)
void glValidateProgram(GLuint program)
// Program에 오류가 있는지 없는지 알고 싶다?
void glUseProgram(GLuint program)
// Use하면 실행 상태가 된다.
```

<br />
---
<br />
