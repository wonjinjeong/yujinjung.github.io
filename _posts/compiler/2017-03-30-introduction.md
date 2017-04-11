---
layout: post
title: Introduction of Compiler
description:
modified: 2017-03-30
tags: [Compiler]
image:
  feature:  background/Desk/1.jpg
  credit: unsplash
---
##### ** 개인적으로 정리하는 것이기 때문에 틀린 내용이 들어가 있을 수 있습니다.

## The Structure of a Compiler

![structure](/images/compiler/structureCompiler.png)
{: .image-center }

### Analysis part
> #### lexical analyzer - 단어들을 분석 (어떻게 연결되는 지는 관심이 없다)
> #### syntax analyzer - 문법을 분석 (형태가 어떻게 되어있는 지에 대해)
> #### semantic analyzer - 의미를 분석 (자세하게는 하지 않기 때문에 semi-semantic이라고도 한다)

### Synthesis part
> #### intermediate code generator ~ machine dependent code optimizer

#### Optimizer는 두군데에 다 속한다.
<br />

## Lexical Analysis

![lexical](/images/compiler/lexical.png)
{: .image-center }

#### input String을 의미있는 단위(lexemes)로 나누고 token을 생성한다.(lexeme -> token)
#### 공백이나 주석은 lexical analysis 동안에 삭제된다.
#### 또한 Symbol table & Literal table을 관리한다.

* *Literal*
    * number - 3.141592
    * string - "Hello world"

## Syntax Analysis

#### syntax analyser 또는 parser가 수행한다.
#### 문법적인 form(grammatical phrase)에 맞는지 chk한다.
#### parse tree | syntax tree 를 생성한다.
#### syntax tree is compressed by parse tree.

### Parse tree
> #### hierarchical representation of program
> #### structured by recursive rules

![parse & syntax tree](/images/compiler/parse_syntax_tree.png)

## Code Optimization
### 속도를 향상시킨다.
### 기법들
> #### folding
> #### common subexpression elimination
> #### dead code elimination
> #### loop optimization
