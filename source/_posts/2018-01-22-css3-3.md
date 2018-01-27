---
title: CSS3(3) 文字与字体相关样式
date: 2018-01-22 21:40:32
categories:
- CSS3抄书笔记
tags:
- 文字样式
- 服务器端字体
---

本章针对css3中与文字、字体相关的一些属性做详细介绍，其中包括`text-shadow`属性、`word-break`属性、`word-wrap`属性、`Web Font`和`@font-face`属性，以及`font-size-adjust`属性。

<!-- More -->

### 一、文字添加阴影

`text-shadow`属性用于给页面上的文字添加阴影效果

```css
text-shadow: length length length color
```

其中，前三个`length`分别指阴影离开文字的 **横方向距离**、**纵方向距离**、**模糊半径**，color指阴影的 **颜色**。

**指定多个阴影**

可以使用`text-shadow`属性给文字指定多个阴影，并且针对每个阴影使用不同颜色，指定多个阴影时使用逗号将多个阴影进行分隔。

```css
div {
    text-shadow: 10px 10px 5px #f39800,
                40px 35px 5px #fff100,
                70px 60px 5px #c0ff00;
}
```

---

### 二、文本自动换行

在css3中，使用`word-break`属性来让文字自动换行

浏览器显示文本的时候，会让文本在浏览器或div元素的右端自动实现换行。对于西方文字来说，浏览器会在半角空格或连字符的地方自动换行，而不会在单词的当中突然换行。对于中文来说，可以在任何一个中文字后面进行换行。如果中文当中含有西方文字，浏览器也会在半角空格或连字符的地方进行换行，而不会在单词中强制换行。

当中文当中含有标点符号的时候，浏览器总是不可能让标点符号位于一行文字的行首，通常将标点符号以及它前面的一个文字作为一个整体来统一换行。

#### 1. 指定自动换行

在css3中，可以使用`word-break`属性来自己决定自动换行的处理方法。通过`word-break`属性的指定，不仅仅可以让浏览器实现半角空格或连字符后面的换行，而且可以让浏览器实现任意位置的换行。

```css
div {
    word-break: keep-all;
}
```

| 值 | 换行规则
| - | -
| normal | 使用浏览器默认换行规则
| keep-all | 只能在半角空格或连字符处换行
| break-all | 允许在单词内换行

#### 2. 长单词与URL地址自动换行

对于西方文字来说，浏览器在半角空格或连字符的地方进行换行。因此，浏览器不能给较长的单词自动换行。当浏览器窗口比较窄的时候，文字会超出浏览器的窗口，浏览器下部出现滚动条，让用户通过拖动滚动条的方法来查看没有在当前窗口显示的文字。

在css3中，使用`word-wrap`属性来实现长单词与url地址的自动换行

```css
div {
    word-wrap: break-word;
}
```

`word-wrap`属性可以使用的属性值为`normal`与`break-word`两个。使用`normal`属性值时浏览器保持默认处理，只在半角空格或连字符的地方进行换行。使用`break-word`时浏览器可以在长单词或url地址内部进行换行。

---

### 三、使用服务器端字体

在css3之前，页面文字所使用的字体必须已经在客户端中被安装才能正常显示，在样式表中允许指定当前字体不能正常显示时使用的替代字体，但是如果这个替代字体在客户端中也没有被安装时，使用这个字体的文字就不能正常显示了。

在css3中，使用`@font-face`属性来利用服务器端字体

```css
@font-face {
    font-family: WebFont;
    src: url('font/Fontin_Sans_R_45b.otf') format('opentype');
    font-weight: normal;
}
```

`font-family`属性值用`WebFont`来声明使用服务器端的字体。`src`属性值指定服务器端字体的字体文件所在的路径。

在针对元素使用这个服务器端字体的时候，还需要在元素样式中将`font-family`属性值指定为`WebFont`

```css
h1 {
    font-family: WebFont;
}
```

`@font-face`属性不仅可以用于显示服务器端的字体，也可以用来显示客户端本地的字体。

```css
@font-face {
    font-family: Arial;
    src: local('Arial');
}
```

使用`@font-face`属性显示客户端本地字体的好处是可以让浏览器在对字体进行显示时首先在客户端本地寻找是否存在该字体，当客户端寻找不到时在可使用服务器端的字体。

```css
@font-face {
    font-family: MyHelvetica;
    src: local('Helvetica Neue') url(MgOpenModernaRegular.ttf);
}
```

---

### 四、修改字体种类而保持字体尺寸不变

如果改变了字体的种类，则页面中所有使用改字体的文字大小都可能发生变化，从而使得原来安排好的页面布局产生混乱。

因此，在css3中增加了`font-size-adjust`属性，可以在保持文字大小不发生变化的情况下改变字体的种类。

`font-size-adjust`属性的使用方法很简单，但是它需要使用每个字体种类自带的一个`aspect`值（比例值）。

```css
div {
    font-size: 16px;
    font-family: Times New Roman;
    font-size-adjust: 0.46;
}
```

`aspect`值可以用来在将字体修改为其他字体时保持字体大小基本不变，这个`aspect`值的计算方法为`x-height`值除以该字体的尺寸，`x-height`值是指使用这个字体书写出来的小写`x`的高度（像素为单位）。如果某个字体的尺寸为100px时，`x-height`值为58px，则该字体的`aspect`为0.58，因为字体的`x-height`值总是随着字体的尺寸一起改变的，所以字体的`aspect`值都是一个常数。

**常用的西方字体aspect值**

| 字体种类 | aspect值
| - | -
| Verdana | 0.58
| Comic Sans MS | 0.54
| Trebuchet MS | 0.53
| Georgia | 0.5
| Myriad Web | 0.48
| Minion Web | 0.47
| Times New Roman | 0.46
| Gill Sans | 0.46
| Bernhard Modern | 0.4
| Caflisch Script Web | 0.37
| Fjemish Script | 0.28

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()
