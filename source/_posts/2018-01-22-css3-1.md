---
title: CSS3(1) 选择器
date: 2018-01-22 21:29:23
categories:
- CSS3抄书笔记
tags:
- CSS选择器
---

选择器是CSS3中的一个重要内容，使用它可以大幅度提高开发人员书写或修改样式表时的工作效率。

<!-- More -->

通常我们会在元素上定义`class`属性，然后根据元素的`class`属性在css文件中定义相关的样式。使用`class`属性有两个缺点：

* `class`属性本身没有语义，它纯粹时用来为css样式服务的，属于多余属性。

* 使用`class`属性的话，并没有把样式与元素绑定起来，针对同一个`class`属性，文本框也可以使用，下拉框也可以使用，甚至按钮也可以使用，这样其实是非常混乱的，修改样式的时候也很不方便。

所以在CSS3中，提倡使用选择器来将样式与元素直接绑定起来，这样的话，在样式表中什么样式与什么元素相匹配变得一目了然，修改起来也很方便。不仅如此，通过选择器，我们还可以实现各种复杂的指定，同时也能大量减少样式表的代码书写量，最终书写出来的样式表也会变得简洁明了。

### 一、属性选择器

html中，我们可以通过各种各样的属性，给元素增加很多附加信息。例如通过`width`属性，我们可以指定`div`元素的宽度；通过`id`属性，我们可以将不同的`div`元素进行区分。

css3中，增加了如下所示三种属性选择器，使得属性选择器有了通配符的概念

* [att*=val]

    如果元素用`att`表示的属性之属性值包含用`val`指定的字符的话，则该元素使用这个样式。

```css
[id*=section] {
    background-color: yellow;
}

<div id="section1"></div>
<div id="section1-1"></div>
<div id="hs-section"></div>
<div id="hs-section-1"></div>
```

* [att^=val]

    如果元素用`att`表示的属性之属性值的开头字符为用`val`指定的字符的话，则该元素使用这个样式。

```css
[id^=section] {
    background-color: yellow;
}

<div id="section1"></div>
<div id="section1-1"></div>
```

* [att$=val]

    如果元素用`att`表示的属性之属性值的结尾字符为用`val`指定的字符的话，则该元素使用这个样式。

```css
[id$=section] {
    background-color: yellow;
}

<div id="hssection"></div>
<div id="hs-section"></div>
```

**使用示例**: 根据`a`标签的`href`中不同的文件扩展符，使用不同的样式，在超链接地址的末尾添加不同的文字

```css
a[href$=\/]:after, a[href$=htm]:after, a[href$=html]:after {
    content: 'web网页';
    color: red;
}

a[href$=jpg]:after {
    content: 'jpg图片';
    color: green;
}
```

---

### 二、伪类选择器

所谓伪类选择器，是指并不是针对真正的元素使用的选择器，而针对CSS中已经定义号的伪元素使用的选择器。CSS中主要有四个伪元素选择器：

* `first-line`伪元素选择器

    用于为某个元素中的第一行文字使用样式

* `first-letter`伪元素选择器

    用于为某个元素中的文字的首字母或第一个字使用样式

* `before`伪元素选择器

    用于在某个元素之前插入一些内容

* `after`伪元素选择器

    用于在某个元素之后插入一些内容

---

### 三、root、not、empty、target

#### 1. root选择器

`root`选择器将样式绑定到页面的根元素中。所谓根元素，是指位于文档树中最顶层结构的元素，在HTML页面中就是指包含着整个页面的`“<html>”`部分。

```css
:root {
    background-color: yellow;
}
body {
    background-color: limegreen;
}
```

#### 2. not

如果相对某个结构元素使用样式，但是想排除这个结构元素下面的子结构元素，让它不使用这个样式时，可以使用`not`选择器。

```css
body *:not(h1) {
    background-color: yellow;
}
```

#### 3. empty

使用`empty`选择器来指定当元素内容为空白时使用的样式。

```css
:empty {
    background-color: yellow
}
```

#### 4. target

使用`target`选择器来对页面中的某个`target`元素（该元素的`id`被当作页面中的超链接来使用）指定样式，该样式只在用户点击了页面中的超链接，并且跳转到`target`元素后起作用。

```css
:target {
    background-color: yellow;
}
```

---

### 四、first-child、last-child、nth-child、nth-last-child

利用这几个选择器，能够特殊针对父元素的第一个子元素、最后一个子元素、指定序号的子元素、甚至第偶数个、第奇数个子元素进行样式的指定。

```css
li:first-child {
    background-color: yellow;
}

li:last-child {
    background-color: skyblue;
}

li:nth-child(n) {
    /* css中的n从1开始计数 */
    background-color: red;
}

li:nth-child(even) {
    /* 所有正数下来的第偶数个子元素 */
}

li:nth-child(odd) {
    /* 所有正数下来的第奇数个子元素 */
}

li:nth-last-child(odd) {
    /* 所有倒数上去的第奇数个子元素 */
}
```

---

### 五、nth-of-type、nth-last-of-type

在css3中，使用`nth-of-type`选择器与`nth-last-of-type`选择器可以避免父元素下不同类型子元素的序号选择问题。使用这两个选择器的时候，css3在计算子元素是第奇数个还是第偶数个的时候，就只针对同类型的子元素进行计算。

```css
h2:nth-of-type(odd) {
    background-color: yellow;
}

h2:nth-of-type(even) {
    background-color: skyblue;
}
```

---

### 六、循环使用样式

如果我们有100个列表项目，需要重复使用前四种样式的设置，可以采用循环指定的方式，只要在 `nth-child(n)` 语句中，把参数 `n` 改成可循环的 `an + b` 的形式就可以，`a` 表示每次循环中共包括几种样式， `b` 表示指定的样式在循环中的位置。

```css
li:nth-child(4n+1) {
    background-color: yellow;
}

li:nth-child(4n+2) {
    background-color: limegreen;
}

li:nth-child(4n+3) {
    background-color: red;
}

li:nth-child(4n) {
    background-color: white;
}
```

---

### 七、only-child

如果采用如下琐事的方法结合运用`nth-child`选择器与`nth-last-child`选择器的话，可以指定当某个父元素中只有一个子元素时才使用的样式。

```css
div:nth-child(1):nth-last-child(1) {
    background-color: red;
}
```

另外也可以使用`only-child`选择器来替代上面的实现方法

```css
div:only-child {
    background-color: red;
}
```

---

### 八、UI元素状态伪类选择器

在CSS3的选择器中，除了结构性伪类选择器外，还有一种UI元素状态伪类选择器。这些选择器的共同特征是：指定的样式只有当元素处于某种状态下时才起作用，在默认状态下不起作用。

在CSS3中，共有11种UI元素状态伪类选择器，分别是

`E:hover  E:active  E:focus  E:enabled  E:disabled  E:read-only  E:read-write  E:checked  E:default  E:indeterminate  E::selection `

#### 1. E:hover

指定当鼠标指针移动到元素上面时元素所使用的样式

#### 2. E:active

指定元素被激活（鼠标在元素上按下还没有松开）时使用的样式

#### 3. E:focus

指定元素获得光标焦点时使用的样式，主要是在文本框控件获得焦点并进行文字输入的时候使用

#### 4. E:enabled

指定当元素处于可用状态时的样式

#### 5. E:disabled

指定当元素处于不可用状态时的样式

#### 6. E:read-only

指定当元素处于只读状态时的样式

#### 7. E:read-write

指定当元素处于非只读状态时的样式

#### 8. E:checked

指定当表单中的`radio`或`checkbox`处于选取状态时的样式，在firefox浏览器中，需要写成 `-moz-checked` 的形式

#### 9. E:default

指定当页面打开默认处于选取状态的单选框或复选框控件的样式

#### 10. E:indeterminate

指定当页面打开时，如果一组单选框中任何一个单选框都没有被设定为选取状态时整组单选框的样式，如果用户选取了对其中任何一个单选框，则该样式被取消指定

#### 11. E::selection

指定当元素处于选中状态时的样式

---

### 九、通用兄弟元素选择器

通用兄弟元素选择器，用来指定位于同一个父元素之中的某个元素之后的所有其他某个种类的兄弟元素所使用的样式

```css
<子元素>~<子元素之后的同级兄弟元素> {
    // 指定样式
}
```

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()
