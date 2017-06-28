---
layout: post
title: Primitives 
description:
modified: 2017-06-26
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/2.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Point Sprites

## GL_POINTS
  * gl_PointSize - build in variable for point radius
  * gl_PointCoord - vec2 variable in fragment shader
  * PointCoord cna be used as texture corrdinate to draw textured point sprinte

```cpp
uniform sampler2D s_texSprite;

void main(void)
{
  gl_FragColor = texture2D(s_texSprite, gl_PointCoord);
}
```

<br/>

---

# Lines

## GL_LINES

<figure>
  <a href="/images/CG/Primitive/gl_line_loop.png"><img src="/images/CG/Primitive/gl_lines.png" alt=""></a>
  <figcaption><a href="/images/CG/Primitive/gl_lines.png" title="GL_LINES"> GL_LINES</a></figcaption>
</figure>

<br/>

## GL_LINE_STRIP

<figure>
	<a href="/images/CG/Primitive/gl_line_strip.png"><img src="/images/CG/Primitive/gl_line_strip.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/gl_line_strip.png" title="GL_LINE_STRIP">GL_LINE_STRIP</a></figcaption>
</figure>


  - n vertices -> (n-1) lines
  - (v0, v1), (v1, v2), (v2, v3), ...

<br/>

## GL_LINE_LOOP

<figure>
	<a href="/images/CG/Primitive/gl_line_loop.png"><img src="/images/CG/Primitive/gl_line_loop.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/gl_line_loop.png" title="GL_LINE_LOOP"> GL_LINE_LOOP</a></figcaption>
</figure>

  - n vertices -> (n) lines
  - (v0, v1), (v1, v2), (v2, v3), ... , (vn, v0)
  - 닫힌다.

* 모두 glDrawArrays 나 glDrawElemnets로 그린다.

* glLineWidth
  - set line width

<br/>

---

# Triangles
## GL_TRIANGLES

<figure>
	<a href="/images/CG/Primitive/gl_triangles.png"><img src="/images/CG/Primitive/gl_triangles.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/gl_triangles.png" title="GL_TRIANGLES"> GL_TRIANGLES</a></figcaption>
</figure>

  - n vertices -> n/3 triangles 
  - (v0, v1, v2), (v3, v4, v5), ...

## GL_TRIANGLE_STRIP

<figure>
	<a href="/images/CG/Primitive/gl_triangle_strip.png"><img src="/images/CG/Primitive/gl_triangle_strip.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/gl_triangle_strip.png" title="GL_TRIANGLE_STRIP"> GL_TRIANGLE_STRIP</a></figcaption>
</figure>

  - n vertices -> (n-2) triangles 
  - (v0, v1, v2), (v2, v1, v3), ...

## GL_TRIANGLE_FAN

<figure>
	<a href="/images/CG/Primitive/gl_triangle_fan.png"><img src="/images/CG/Primitive/gl_triangle_fan.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/gl_triangle_fan.png" title="GL_TRIANGLE_STRIP"> GL_TRIANGLE_STRIP</a></figcaption>
</figure>

  - n vertices -> (n-2) triangles 
  - (v0, v1, v2), (v0, v2, v3), (v0, v3, v4), ...
  - v0처럼 한 점이 고정되고 부채꼴처럼 두 점씩 잡아서 삼각형을 그린다.

* 모두 glDrawArrays 나 glDrawElemnets로 그린다.

<br/>

---

# Draw Primitives

## glDrawArrays

```cpp
void glDrawArrays(GLenum mode, GLint first, GLsizei count)
```

<figure>
	<a href="/images/CG/Primitive/glDrawArrays.png"><img src="/images/CG/Primitive/glDrawArrays.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/glDrawArrays.png" title="glDrawArrays"> glDrawArrays</a></figcaption>
</figure>

* mode
  * mode specifies the primitive to render.
  * primitive를 생성하는 규칙을 지정한다.
  * GL_POINTS
  * GL_LINES, GL_LINE_STRIP, GL_LINE_LOOP
  * GL_TRIANGLES, GL_TRIANGLE_STRIP, GL_TRIANGLE_FAN

* first
  * first specifies the starting vertex index in the enabled vertex arrays
  * vertex array에서 시작할 곳을 지정한다.


<br/>

## glDrawElements

<figure>
	<a href="/images/CG/Primitive/glDrawElements.png"><img src="/images/CG/Primitive/glDrawElements.png" alt=""></a>
	<figcaption><a href="/images/CG/Primitive/glDrawElements.png" title="glDrawElements"> glDrawElements</a></figcaption>
</figure>

```cpp
void glDrawElements(GLenum mode, GLsizei count, GLenum type, const GLvoid *indices)
```

* mode
  * mode specifies the primitive to render.
  * primitive를 생성하는 규칙을 지정한다.
  * GL_POINTS
  * GL_LINES, GL_LINE_STRIP, GL_LINE_LOOP
  * GL_TRIANGLES, GL_TRIANGLE_STRIP, GL_TRIANGLE_FAN

* count
  * count specifies the number of indices
  * indice에서 몇 개씩 뽑아서 사용할 것인지를 지정한다.

* type
  * type specifies the type of element indices stored in indices.
  * GL_UNSIGNED_BYTE, GL_UNSIGNED_SHORT, GL_UNSIGNED_INT ...

* indices
  * indices specifies a pointer to location
  * indice에는 vertex array의 몇번째 값을 가져다가 쓸건지 저장되어 있다.

