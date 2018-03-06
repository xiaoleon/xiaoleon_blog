---
title: React(3) 从Flux到Redux
categories:
  - React抄书笔记
tags:
  - JavaScript
  - React
date: 2018-03-06 23:05:43
---

在前一章中我们已经感受到完全用React来管理应用数据的麻烦，在这一章中，我们将介绍Redux这种管理应用状态的框架

<!-- More -->

本章包含以下内容：

* 单向数据流框架的始祖Flux

* Flux理念的一个更强实现Redux

* 结合React和Redux

### 一、Flux

要了解Redux，首先要从Flux说起，可以认为Redux是Flux思想的另一种实现方式，通过了解Flux，我们可以知道Flux一族框架（其中就包括Redux）贯彻的最重要的观点——单向数据流，更重要的是，我们可以发现Flux框架的缺点，从而深刻地认识到Redux相对于Flux的改进之处。

让我们来看看Flux的历史，实际上，Flux是和React同时面世的，在2013年，Facebook公司让React亮相的同时，也推出了Flux框架，React和Flux相辅相成，Facebook认为两者结合在一起才能构建大型的JavaScript应用。

做一个容易理解的对比，React是用来替换jQuery的，那么Flux就是以替换Backbone.js、Ember.js等MVC一族框架为目的。

在MVC（Model-View-Controller）的世界里，React相当于V（View）的部分，只涉及页面的渲染，一旦涉及应用的数据管理部分，还是交给Model和Controller，不过，Flux并不是一个MVC框架，事实上，Flux认为MVC框架存在很大问题，它推翻了MVC框架，并用一个新的思维来管理数据流转。

#### 1. MVC框架的缺陷




---

### 二、

<!-- Todo -->


---

### 三、

<!-- Todo -->


---

### 四、

<!-- Todo -->

### 参考文献

1. [《深入浅出React和Redux —— 程墨》]()