---
title: H5(1) 概述
date: 2018-01-21 20:51:34
categories:
- HTML5
---

自从2010年HTML5正式推出以来，它立刻受到了世界各大浏览器的热烈欢迎与支持。根据世界上各大IT界知名媒体评论，新的Web时代，HTML5的时代马上就要到来。本文主要介绍HTML5的一些优势和能够解决什么问题。

<!-- More -->

### 一、HTML5可以放心使用的三个理由：

* 兼容性：HTML5在老版本的浏览器上也可以正常运行。

* 实用性：HTML5内部并没有封装什么很复杂的、不切实际的功能，而只是封装了简单实用的功能。

* 非革命性的发展：HTML5的内部功能不是革命性的，只是发展性的。

以上三点就是所谓的“HTML设计原则”，HTML5也是以该设计原则为基本原则而开发出来的。

---

### 二、HTML5要解决的三个问题：

* Web浏览器之间的兼容性很低。

* 文档结构不够明确。

* Web应用程序的功能受到了限制。

---

### 三、HTML元素规范

#### 1. 省略元素标记

HTML5中，元素的标记可以省略，具体来说，元素的标记分为“不允许写结束标记”、“可以省略结束标记”、“开始标记和结束标记全部可以省略”三种类型。

* 不允许写结束标记的元素有：`area、base、br、col、command、embed、hr、img、input、keygen、link、meta、param、source、track、wbr`。

* 可以省略结束标记的元素有：`li、dt、dd、p、rt、rp、optgroup、option、colgroup、thead、tbody、tfoot、tr、td、th`。

* 可以省略全部标记的元素有：`html、head、body、colgroup、tbody`。

> **说明**：
> “不允许写结束标记的元素”是指，不允许使用开发标记与结束标记将元素括起来的形式，只允许使用`“<元素/>”`的形式进行书写。例如`“<br>...</br>”`的书写方式是错误的，正确的书写方式为`“<br/>”`。当然，HTML5之前的版本中`<br>`这种写法可以被沿用。
> “可以省略全部标记的元素”是指，该元素可以完全被省略。请注意，即使标记被省略了，该元素还是以隐式的方式存在的。例如将body元素省略不写时，但它在文档结构中还是存在的，可以使用document.body进行访问。

#### 2. 省略boolean元素属性

对于具有boolean值的属性，例如`disabled`与`readonly`等，当只写属性而不指定属性值时，表示属性值为`true`；如果想要将该属性值设为`false`，可以不使用该属性。另外，要想将属性值设置为`true`时，也可以将属性名设定为属性值，或将空字符串设定为属性值。

参考代码示例：

```html
// 只写属性不屑属性值代表属性为true
<input type="checkbox" checked>
// 不写属性代表属性为false
<input type="checkbox">
// 属性值=属性名，代表属性为true
<input type="checkbox" checked="checked">
// 属性值=空字符串，代表属性为true
<input type="checkbox" checked>
```

#### 3. 省略引号

HTML5元素标记中，当属性值不包括空字符串、`“<”`、`“>”`、`“=”`、单引号、双引号等字符时，属性值两边的引号可以省略。

参考代码示例：

```html
<input type="text" />
<input type='text' />
<input type=text />
```

#### 4. 新增的input元素的类型

HTML5中新增了很多input元素的类型，举例如下：

* 1) `email`. 表示必须输入Email地址的文本输入框

* 2) `url`. 表示必须输入url地址的文本输入框

* 3) `number`. 表示必须输入数值的文本输入框

* 4) `range`. 表示必须输入一定范围内数字值的文本输入框

* 5) `Date Picker`. HTML5拥有多个可供选取日期和时间的新型输入文本框

    * `date`. 选取日、月、年

    * `month`. 选取月、年

    * `week`. 选取周和年

    * `time`. 选取时间（小时和分钟）

    * `datetime`. 选取时间、日、月、年（UTC时间）

    * `datetime-local`. 选取时间、日、月、年（本地时间）


#### 5. 全局属性

* 1) contentEditable属性。

    该属性的主要功能是允许用户编辑元素中的内容，所以该元素必须是可以获得鼠标焦点的元素，而且在点击鼠标后要向用户提供一个插入符号，提示用户该元素中的内容允许编辑。`contentEditable`属性是一个布尔值属性，可以被指定为`true`或`false`。
    
    元素还具有一个`isContentEditable`属性，当元素可编辑时，该属性为`true`；当元素不可编辑时，该属性为`false`。

* 2） designMode属性。

    `designMode`属性用来指定整个页面是否可编辑，当页面可编辑时，页面中任何支持上文所述的`contentEditable`属性的元素都变成了可编辑状态。`designMode`属性只能在Javascript脚本里被编辑修改，包含两个属性值——`“on”`、`“off”`。

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()