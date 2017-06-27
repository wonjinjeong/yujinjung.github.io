---
layout: post
title: Primitives 
description:
modified: 2017-06-26
tags: [Computer Graphics, OpenGL]
image:
  feature: background/Computer/1.jpg
  credit: unsplash
---
##### 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.
---

# Primitives
## Point Sprites
* GL_POINTS
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

## Lines
### GL_LINES

![GL_LINES](/images/CG/gl_lines.png)

  - n vertices -> n/2 lines
  - (v0, v1), (v2, v3), (v4, v5), ...

### GL_LINE_STRIP

![GL_LINE_STRIP](/images/CG/gl_line_strip.png)

  - n vertices -> (n-1) lines
  - (v0, v1), (v1, v2), (v2, v3), ...

### GL_LINE_LOOP

![GL_LINE_LOOP](/images/CG/gl_line_loop.png)

  - n vertices -> (n) lines
  - (v0, v1), (v1, v2), (v2, v3), ... , (vn, v0)
  - 닫힌다.

* 모두 glDrawArrays 나 glDrawElemnets로 그린다.

* glLineWidth
  - set line width

<br/>

---

## Triangles
### GL_TRIANGLES

![GL_TRIANGLES](/images/CG/gl_triangles.png)

  - n vertices -> n/3 triangles 
  - (v0, v1, v2), (v3, v4, v5), ...

### GL_TRIANGLE_STRIP

![GL_TRIANGLE_STRIP](/images/CG/gl_triangle_strip.png)

  - n vertices -> (n-2) triangles 
  - (v0, v1, v2), (v2, v1, v3), ...

### GL_TRIANGLE_FAN

![GL_TRIANGLE_FAN](/images/CG/gl_triangle_fan.png)

  - n vertices -> (n-2) triangles 
  - (v0, v1, v2), (v0, v2, v3), (v0, v3, v4), ...
  - v0처럼 한 점이 고정되고 부채꼴처럼 두 점씩 잡아서 삼각형을 그린다.

* 모두 glDrawArrays 나 glDrawElemnets로 그린다.

