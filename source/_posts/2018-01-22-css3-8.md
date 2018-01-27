---
title: CSS3(8) 布局相关样式
date: 2018-01-22 22:00:00
categories:
- CSS3抄书笔记
tags:
- layout
- 多栏布局
- 盒布局
---

Web页面中的布局，是指在页面中如何对标题、导航栏、主要内容、脚注、表单等各种构成要素进行一个合理的编排。在css3之前，主要使用`float`属性或`position`属性进行页面中的简单布局，但是存在很多缺点，譬如两栏或多栏中如果元素的内容高度不一致则由底部很难对齐的问题。

<!-- More -->

### 一、多栏布局

我们首先回顾以下css3之前是如何使用`float`属性或`position`属性进行页面中的简单布局的。

```css
div {
    width: 20em;
    float: left;
}

div#div1 {
    margin-right: 2em;
}
```

```html
<body>
    <div id="div1"></div>
    <div id="div2"></div>
</body>
```

使用`float`属性或`position`属性进行页面布局时有一个比较明显的缺点，就是两个`div`元素是相互独立的，因此如果在第一个`div`元素中加入一些内容的话，将会使得两个元素的底部不能对其，导致页面中多出一块空白区域。

针对`float`属性或`position`属性的缺点，css3中加入了多栏布局方式，使用多栏布局可以将一个元素中的内容分为两栏或多栏显示，并且确保各栏中内容的底部对齐。

```css
div {
    width: 50em;
    column-count: 2; 
}
```

通过`column-count`属性来使用多栏布局方式，该属性的含义是讲一个元素中的内容分为多栏进行显示。使用多栏布局的时候，需要将元素的宽度设置成多个栏目的总宽度，它与使用`float`属性和`position`属性时的区别是：使用两个属性时只需单独设定每个元素的宽度即可，而使用多栏布局时需要设定元素中多个栏目相加后的总的宽度。

我们也可以使用`column-width`属性单独设置每一栏的宽度而不设定元素的宽度。

```css
div {
    column-count: 2;
    column-width: 20em;
}
```

可以使用`column-gap`属性来设定多栏之间的间隔距离。

```css
div {
    column-count: 2;
    column-gap: 2em;
}
```

可以使用`column-rule`属性在栏与栏之间增加一条间隔线，并且设定该间隔线的宽度、颜色等，该属性的属性值的指定方法与css中`border`属性的属性值指定方法相同。

```css
div {
    column-count: 2;
    column-rule: 1px solid red;
}
```

---

### 二、盒布局

在css3中，除了多栏布局之外，还可以使用盒布局解决前面所说的使用`float`属性或`position`属性时左右两栏或多栏中底部不能对齐的问题。

接下来我们看一个示例，示例中有三个`div`元素，简单展示了网页中的左侧边栏、中间内容和右侧边栏。

```css
#left {
    float: left;
    width: 200px;
    padding: 20px;
    background-color: orange;
}

#middle {
    float: left;
    width: 300px;
    padding: 20px;
    background-color: yellow;
}

#right {
    float: left;
    width: 200px;
    padding: 20px;
    background-color: limegreen;
}

#left, #middle, #right {
    box-sizing: border-box;
}
```

```html
<div id="container">
    <div id="left">aaaa</div>
    <div id="middle">bbbb</div>
    <div id="right">cccc</div>
</div>
```

示例中如果`div`的内容变化时，`div`元素的底部会出现无法对齐的问题。

在css3中，我们可以通过`box`属性来使用盒布局，修改代码如下

```css
#container {
    display: -webkit-box;
    display: -moz-box;
}

#left {
    width: 200px;
    padding: 20px;
    background-color: orange;
}

#middle {
    width: 300px;
    padding: 20px;
    background-color: yellow;
}

#right {
    width: 200px;
    padding: 20px;
    background-color: limegreen;
}

#left, #middle, #right {
    box-sizing: border-box;
}
```

* 盒布局与多栏布局的区别

    使用多栏布局时，各栏宽度必须是相等的，在指定每栏宽度时，也只能为所有栏指定一个统一的宽度，栏与栏之间的宽度不可能是不一样的。另外，使用多栏布局时，也不可能具体指定什么栏中显示什么内容，因此比较适合使用在显示文章内容的时候，不适合用于安排整个网页中由个元素组成的网页结构时。


* 盒布局标准

    可以采用最新的`display: flex`属性，`-webkit-box`属于2009年的提案。

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()

2. [《Flex 布局教程：语法篇》（阮一峰）](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
