---
title: CSS3(4) 盒相关样式
date: 2018-01-22 21:44:04
categories:
- CSS3抄书笔记
tags:
- 盒模型
- box-sizing
---

在css中，使用`display`属性来定义盒的类型。总体上来说，css的盒分为`block`类型与`inline`类型。

 <!-- More -->

### 一、inline-block

`inline-block`是在css2.1中追加的一个盒类型，属于`block`类型盒的一种，但是在显示时具有`inline`类型盒的特点。

默认情况下使用`inline-block`类型时并列显示的元素的垂直对齐方式是底部对其，为了将垂直对齐方式改为顶部对齐，还需要在`div`元素的样式中加入`vertical-align`属性。

---

### 二、inline-table

由于`table`元素属于`block`类型，所以不能与其他文字处于同一行中，但是如果将`table`元素修改wei`inline-table`类型，就可以让表格与其他文字处于同一行中了。

```css
table {
    display: inline-table;
    border: 1px solid #00aaff;
}
```

可以在样式中利用`vertical-align`属性显示指定表格与文字的对齐方式。

---

### 三、list-item

如果在`display`中将元素的类型设定为`list-item`类型，可以将多个元素作为列表来显示，同时在元素的开头加上列表的标记。

```css
div {
    display: list-item;
    list-style-type: circle;
    margin-left: 30px;
}
```

---

### 四、run-in、compact

将元素指定为`run-in`类型或者`compact`类型的时候，如果元素后面还有block类型的元素，`run-in`类型的元素将被包含在`block`类型的元素内部，而`compact`类型的元素将被放置在`block`类型的元素左边。

---

### 五、超出盒容纳的内容

如果在样式中指定了盒的宽度与高度，就有可能出现某些内容在盒中容纳不下的情况，可以使用`overflow`属性来指定如何显示这些内容。

* overflow: hidden

    超出容纳范围的文字被隐藏

* overflow: scroll

    元素中出现固定的水平滚动条与垂直滚动条

* overflow: auto

    文字超出div元素的容纳范围时，根据需要出现水平滚动条或垂直滚动条

* overflow: visible

    显示效果与不使用overflow属性时一样

* text-overflow

    当通过把`overflow`属性设定为`ellipsis`，将盒中容纳不下的内容隐藏起来时，如果使用`text-overflow`属性，可以在盒的末尾显示一个代表省略的符号`“...”`。但是，`text-overflow`属性只在当盒中的内容在水平方向上超出盒的容纳范围时有效。通过将`white-space`属性设定为`nowrap`，使得盒右端的内容不能换行显示。

```css
div {
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
    -webkit-text-overflow: ellipsis;
    -o-text-overflow: ellipsis;
}
```

---

### 六、对盒使用阴影

在css3中，可以使用`box-shadow`属性让盒在显示时产生阴影特效

```css
box-shadow: length length length color;
-webkit-box-shadow: length length length color; // safari
-moz-box-shadow: length length length color; // firefox
```

前面三个`length`分别指阴影离开文字的 **横向距离**、**纵向距离**、**模糊半径**，color指阴影的 **颜色**

**对第一个文字或第一行使用阴影**

可以使用`first-letter`选择器或`first-line`选择器来只让第一个文字或第一行具有阴影效果

```css
div:first-letter {
    font-size: 22px;
    box-shadow: 5px 5px 5px gray;
}

div:first-line {
    background-color: red;
}
```

---

### 七、盒的宽度和高度

在css3中，使用`box-sizing`属性来指定针对元素的宽度和高度的计算方法，是否包含元素内部的补白区域，以及边框的宽度和高度。

#### 1. content-box

元素的宽度和高度不包括内容补白区域，以及边框的宽度和高度

#### 2. border-box

元素的宽度和高度包括内部补白区域，以及边框的宽度和高度

#### 3. 为什么要使用box-sizing属性

使用`box-sizing`属性的目的是控制元素的总宽度，如果不使用该属性，样式中默认使用的是`content-box`属性值，它只对内容的宽度做了一个指定，却没有对元素的总宽度进行指定。有些场合下利用`border-box`属性值会使得页面布局更加方便。

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()
