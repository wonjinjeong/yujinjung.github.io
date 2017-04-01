---
layout: post
title: Sample
description:
modified: 2017-03-30
tags: [Compiler]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---
##### ** 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

## The Structure of a Compiler

<img src="/images/compiler/structureCompiler.png" height="400px" align="center" />


### Analysis part
> lexical analyzer - 단어들을 분석 (어떻게 연결되는 지는 관심이 없다)
> syntax analyzer - 문법을 분석 (형태가 어떻게 되어있는 지에 대해)
> semantic analyzer - 의미를 분석 (자세하게는 하지 않기 때문에 semi-semantic이라고도 한다)
### Synthesis part
> intermediate code generator ~ machine dependent code optimizer
#### Optimizer는 두군데에 다 속한다.
<br />

## Lexical Analysis

<img src="/images/compiler/lexical.png" height="280px" align = "center" />

input String을 의미있j는 단위(lexemes)로 나누고 token을 생성한다.(lexeme -> token)
공백이나 주석은 lexical analysis 동안에 삭제된다.
또한 Symbol table & Literal table을 관리한다.

*Literal*
number - 3.141592
string - "Hello world"



## Syntax Analysis

syntax analyser 또는 parser가 수행한다.
문법적인 form(grammatical phrase)에 맞는지 chk한다.
parse tree | syntax tree 를 생성한다.
syntax tree is compressed by parse tree.

### Parse tree
* hierarchical representation of program
* structured by recursive rules

img  -----







### Body text

**This is strong**.
![Smithsonian Image]({{ site.url }}/images/3953273590_704e3899d5_m.jpg)
{: .image-right}

*This is emphasized*.
H<sub>2</sub>O.
<cite>(That’s a citation)</cite>.
<u>Underline</u>.
HTML and <abbr title="cascading stylesheets">CSS</abbr>

### Blockquotes
> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

## List Types

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### Unordered Lists

* Item one
* Item two
* Item three

## Tables

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}

## Code Snippets

Syntax highlighting via Rouge

```css
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
```

Non Pygments code example

    <div id="awesome">
        <p>This is great isn't it?</p>
    </div>

## Buttons

Make any link standout more when applying the `.btn` class.

```html
<a href="#" class="btn btn-success">Success Button</a>
```

<div markdown="0"><a href="#" class="btn">Primary Button</a></div>
<div markdown="0"><a href="#" class="btn btn-success">Success Button</a></div>
<div markdown="0"><a href="#" class="btn btn-warning">Warning Button</a></div>
<div markdown="0"><a href="#" class="btn btn-danger">Danger Button</a></div>
<div markdown="0"><a href="#" class="btn btn-info">Info Button</a></div>
