---
title: CSS3(2) 巧用Content
date: 2018-01-22 21:36:53
categories:
- CSS3抄书笔记
tags:
- Content应用
---

`content`是`before`与`after`伪元素中的属性值，用于在伪元素中展示相关内容，我们可以利用这个属性实现一些小功能。

<!-- More -->

### 一、读取元素属性

可以将`alt`属性的值作为图像的标题来显示，如果在`content`属性中通过`“attr(属性名)”`这种形式来指定`attr`属性值，可以将某个属性的属性值显示出来。

```css
img:after {
    content: attr(alt);
    display: block;
    text-align: center;
    margin-top: 5px;
}
```

```html
<img src="sky.jpg" alt="蓝天白云">
```
---

### 二、插入项目编号

可以使用`content`属性来插入项目编号，在`content`属性中使用`counter`属性来针对多个项目追加连续编号。

```css
div {
    counter-increment: mycounter;
}
div:before {
    content: counter(mycounter);
}
div:after {
    content: 'Part 'counter(mycounter)': ';
}
```

使用`before`选择器或`after`选择器的`content`属性，不仅可以追加数字编号，还可以追加字母编号或罗马数字编号

```css
div:before {
    content: counter(mycounter, upper-alpha)'. ';
    color: blue;
    font-size: 42px;
}
```

使用`list-style-type`属性来指定编号的种类，例如，指定大写字母编号时，使用`upper-alpha`属性，指定大写罗马字母时，使用`upper-roman`属性。

#### 1. 编号嵌套

```css
h1:before {
    content: counter(mycounter)'.';
}
h1 {
    counter-increment: mycounter;
}
h2:before {
    content: counter(subcounter)'.';
}
h2 {
    counter-increment: subcounter;
    margin-left: 40;
}
```

如果需要对编号进行重置，则需要使用 `counter-reset` 属性

```css
h1 {
    counter-increment: mycounter;
    counter-reset: subcounter;
}
```

#### 2. 大编号嵌套中编号

```css
h2:before {
    content: counter(mycounter) '-' counter(subcounter) '. ';
}
```

---

### 三、添加嵌套文字符号

可以使用`content`属性的`open-quote`属性值与`close-quote`属性值在字符串两边添加诸如括号、单引号、双引号之类的嵌套文字符号。

另外，在元素的样式中使用`quotes`属性来指定使用什么嵌套文字符号。

```css
h1:before {
    content: open-quote;
}
h1:after {
    content: close-quote;
}
h1 {
    quotes: "(" ")";
}
```

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()