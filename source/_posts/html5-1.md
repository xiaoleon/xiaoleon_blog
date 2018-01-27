---
title: H5(1) 概述
date: 2018-01-21 20:51:34
categories:
- HTML5抄书笔记
tags:
- HTML5意义
- HTML5变化
---

自从2010年HTML5正式推出以来，它立刻受到了世界各大浏览器的热烈欢迎与支持。根据世界上各大IT界知名媒体评论，新的Web时代，HTML5的时代马上就要到来。本文主要介绍HTML5的一些优势和能够解决什么问题。

<!-- More -->

### 一、迎接新的Web时代

在全面介绍HTML5的相关知识之前，我们先来认识一下HTML5中的部分代码，对HTML5有个初步的了解。首先我们看一段HTML4中常见的JavaScript代码：

```html
<form>
    <p><lable>Username:<input name=search type="text" id="search"></label></p>
    <script type="text/javascript">
        document.getElementById('search').focus();
    </script>
</form>
```

在HTML5中，这段代码将会以怎样的形式出现呢？具体代码如下：

```html
<form>
    <p><label>Search:<input name=search autofocus></label></p>
</form>
```

HTML5的目标是为了能够创建更简单的Web程序，书写出更简洁的HTML代码。例如，为了使Web应用程序的开发变得更容易，提供了很多API，为了使HTML变得更简洁，开发出了新的属性、新的元素等等。总体来说，为下一代Web平台提供了许许多多新的功能。

### 二、HTML5可以放心使用的三个理由：

Web开发者最担心的是新技术推出时由于其不成熟所产生的问题。如果能够实现互联网通用标准，可以避免各浏览器之间的不同意，这一点已经被明确了，但是在朝着这方面前进的过程中会不会出现什么周折是令人担心的。HTML5很高兴的告诉我们，它就像以前CSS刚开始普及时一样不会存在什么问题。

有以下三个理由证明可以放心使用HTML5：

* 兼容性：HTML5在老版本的浏览器上也可以正常运行。

* 实用性：HTML5内部并没有封装什么很复杂的、不切实际的功能，而只是封装了简单实用的功能。

* 非革命性的发展：HTML5的内部功能不是革命性的，只是发展性的。

以上三点就是所谓的“HTML设计原则”，HTML5也是以该设计原则为基本原则而开发出来的，各主流浏览器使用HTML5的前提也就是要求HTML5能够符合这些原则，今后将以其为前提来支持HTML5。

---

### 三、HTML5要解决的三个问题：

HTML5的出现，对于Web来说意义是非常重大的。因为它的意图是想要把目前Web上存在的各种问题一并解决掉，它是一个企图心比较强的HTML版本。

那么，到底Web上存在哪些问题，HTML5又打算怎么解决呢？

* Web浏览器之间的兼容性很低。

    首先要提到的就是，Web浏览器之间的兼容性是非常低的。在某个Web浏览器上可以正常运行的`HTML/CSS/JavaScript`等Web程序，在另一个Web浏览器上就不正常了的事情是非常多的。

    在HTML5中，这个问题将得到解决。HTML5的使命是详细分析各Web浏览器所具有的功能，然后以此为基础，要求这些浏览器所有内部功能都要符合一个通用标准。

* 文档结构不够明确。

    第二个问题是，在之前的HTML版本中，文档的结构不够清晰、明确。例如，为了要表示“标题”、“正文”，之前一般都是用`<div>`元素。但是，严格来说，`<div>`不是一个能把文档结构表达得很清楚的元素。而且，对于搜索引擎或者屏幕阅读器来说，过多使用了`<div>`元素，那么这些程序就连“从哪到哪算是重要的正文”等都不知道。

    在HTML5中，为了解决这个问题，追加了很多跟结构相关的元素。不仅如此，还结合了包括微格式、无障碍应用在内的各种各样的周边技术。

* Web应用程序的功能受到了限制。

    最后一个问题是，HTML与Web应用程序的关系十分薄弱。Web应用程序的特征是先从网络下载，然后忠实运行，因此应该对会威胁到用户安全的功能进行限制。

    为了弥补这方面的不足，HTML5已经开始提供各种各样Web应用上的API，各浏览器也在快速地封装着这些API，HTML5已经使富Web应用的实现变成了可能。

---

### 四、HTML5语法的变化

HTML的语法是在SGMLSGML（`Standard Generalized Markup Language`）语言的基础上建立起来的。但是SGML语法非常复杂，要开发能够解析SGML语法的程序也很不容易，所以很多浏览器都不包含SGML的分析器。因此，虽然HTML基本上遵从SGML的语法，但是对于HTML的执行在各浏览器之间并没有一个统一的标准。

与HTML4相比，HTML5在语法上发生了很大的变化。可能有很多人会有疑问，“之前的HTML已经相当普及了！”，“如果改变基础语法，会产生什么影响？”等。但是，HTML5中的语法变化，与其他开发语言中的语法变化在根本意义上有所不同。它的变化，正是因为在HTML5之前没有符合标准规范的Web浏览器！

接下来，让我们具体看一下在HTML5中，到底对语法进行了哪些改变。

#### 1. 内容类型（ContentType）

首先，HTML5的文件扩展符与内容类型保持不变。也就是说，扩展符仍然为“`.html`”或“`.htm`”，内容类型（`ContentType`）仍然为“`text/html`”。

#### 2. DOCTYPE声明

`DOCTYPE`声明是HTML文件中必不可少的，它位于文件第一行。在HTML4中，它的声明方法如下：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Traditional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

在HTML5中，刻意不使用版本声明，一份文档将会适用于所有版本的HTML。HTML5中的`DOCTYPE`声明方法（不区分大小写）如下：

```html
<!DOCTYPE html>
```

#### 3. 指定字符编码

在HTML4中，使用`meta`元素的形式指定文件中的字符编码，如下所示：

```html
<meta http-equiv="Content-Type" content="text/html;charset=UTF-8">
```

在HTML5中，可以使用对`meta`元素直接追加`charset`属性的方式来指定字符编码

```html
<meta charset="UTF-8">
```

#### 4. 省略元素标记

HTML5中，元素的标记可以省略，具体来说，元素的标记分为“不允许写结束标记”、“可以省略结束标记”、“开始标记和结束标记全部可以省略”三种类型。

* 不允许写结束标记的元素有：`area、base、br、col、command、embed、hr、img、input、keygen、link、meta、param、source、track、wbr`。

* 可以省略结束标记的元素有：`li、dt、dd、p、rt、rp、optgroup、option、colgroup、thead、tbody、tfoot、tr、td、th`。

* 可以省略全部标记的元素有：`html、head、body、colgroup、tbody`。

> **说明**：“不允许写结束标记的元素”是指，不允许使用开发标记与结束标记将元素括起来的形式，只允许使用`“<元素/>”`的形式进行书写。例如`“<br>...</br>”`的书写方式是错误的，正确的书写方式为`“<br/>”`。当然，HTML5之前的版本中`<br>`这种写法可以被沿用。
> “可以省略全部标记的元素”是指，该元素可以完全被省略。请注意，即使标记被省略了，该元素还是以隐式的方式存在的。例如将`body`元素省略不写时，但它在文档结构中还是存在的，可以使用`document.body`进行访问。

#### 5. 省略boolean元素属性

对于具有`boolean`值的属性，例如`disabled`与`readonly`等，当只写属性而不指定属性值时，表示属性值为`true`；如果想要将该属性值设为`false`，可以不使用该属性。另外，要想将属性值设置为`true`时，也可以将属性名设定为属性值，或将空字符串设定为属性值。

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

#### 6. 省略引号

HTML5元素标记中，当属性值不包括空字符串、`“<”`、`“>”`、`“=”`、单引号、双引号等字符时，属性值两边的引号可以省略。

参考代码示例：

```html
<input type="text" />
<input type='text' />
<input type=text />
```

#### 7. 新增的input元素的类型

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


#### 8. 全局属性

* 1) contentEditable属性。

    该属性的主要功能是允许用户编辑元素中的内容，所以该元素必须是可以获得鼠标焦点的元素，而且在点击鼠标后要向用户提供一个插入符号，提示用户该元素中的内容允许编辑。`contentEditable`属性是一个布尔值属性，可以被指定为`true`或`false`。
    
    元素还具有一个`isContentEditable`属性，当元素可编辑时，该属性为`true`；当元素不可编辑时，该属性为`false`。

* 2） designMode属性。

    `designMode`属性用来指定整个页面是否可编辑，当页面可编辑时，页面中任何支持上文所述的`contentEditable`属性的元素都变成了可编辑状态。`designMode`属性只能在Javascript脚本里被编辑修改，包含两个属性值——`“on”`、`“off”`。

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()