---
layout: post
title: Shader Programming - Create, Compile, Link 
description:
modified: 2017-04-04
category: OpenGL
tags: [OpenGL, Shader]
image:
  feature: background/Computer/6.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Creating and Compiling Shader

## glCreateShader
```cpp
GLuint glCreateShader(GL_VERTEX_SHADER | GL_FRAGMENT_SHADER)

vertexShader = glCreateShader(GL_VERTEX_SHADER);
```
- Shader 객체 생성
- Vertex Shader or Fragment Shader
- 각각 Input으로 들어간 Shader가 return 되어나온다.

<br />

## glShaderSource
```cpp
void glShaderSource(GLuint shader, GLsizei count, const char ** string, const GLint* len)

glShaderSource(vertexShader, 1, (const char**)&vertexShaderSource, NULL);
```

- 생성된 shader에 Shader source code를 할당한다.

<br />

## glCompileShader
```cpp
void glCompileShader(GLuint shader)

glCompileShader(vertexShader);
```
 
- Shader source code Compile.

<br />

## glGetShaderiv
```cpp
void glGetShaderiv(GLuint shader, GLenum pname, GLint* params)

glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &isShaderCompiled);
```

- p-name - GL_COMPILE_STATUS, GL_DELETE_STATUS, GL_INFO_LOG_LENGTH, GL_SHADER_SOURCE_LENGTH, GL_SHADER_TYPE
- params - pointers to interstorage location for the result of the query 
- 컴파일 된 결과물을 params에 따라 정보를 얻는다.

<br />

## glGetShaderInfoLog
```cpp
void glGetShaderInfoLog(GLuint shader, GLsizei maxLen, GLsizel *len, GLchar * infolog)
```
- Shader Info log 얻기

<br />

## glDeleteShader
```cpp
void glDeleteShader (GLunit shader)
```
입력된 Shader가 삭제된다.

<br />

---

# Creating and Linking a Program

## glCreateProgram
```cpp
GLunit glCreateProgram(void)

shaderProgram = glCreateProgram();
```
- Program 객체를 생성한다.

<br />

## glAttachShader && glDettachShader
```cpp
void glAttachShader(GLuint program, GLuint shader)
void glDettachShader(GLuint program, GLuint shader)

glAttachShader(shaderProgram, fragmentShader);
glAttachShader(shaderProgram, vertexShader);
```

- Program에 Shader를 Attach한다.
- vertex shader, fragment shader 각각 진행해준다.
- Shader를 떼어내고 싶으면 DettachShader

<br />

## glLinkProgram
```cpp
void glLinkProgram(GLunit program)

glLinkProgram(shaderProgram)
```
- Attach 후에 Link를 하게 되면 GPU에서 Run 할 준비가 된다.
- Program을 Link 시킨다.

<br />

## glGetProgramiv
```cpp
void glGetProgramiv(GLuint program, GLenum pname, GLint params)

GLint isLinked;
glGetProgramiv(shaderProgram, GL_LINK_STATUS, &isLinked);
```
- Program 객체의 Link된 상태 결과를 얻는다.

<br />

## glGetProgramInfoLog
```cpp
void glGetProgramInfoLog(GLuint program, GLsizei maxLen, GLsizei *len, GLchar *  infolog)

glGetProgramInfoLog(shaderProgram, infoLogLength, &charactersWritten, infoLog);
```
- Program Info log 를 얻는다.

<br />

## glValidateProgram
```cpp
void glValidateProgram(GLuint program)
```
- Program에 오류가 있는지 없는지 알고 싶다?

<br />

## glUseProgram
```cpp
void glUseProgram(GLuint program)
```
- Use하면 실행 상태가 된다.

<br />

## glDeleteProgram
```cpp
void glDeleteProgram(GLuint program)
```

<br />
