---
title: CSS3(9) Media Queries相关样式
date: 2018-01-22 22:02:35
categories:
- CSS3抄书笔记
tags:
- 响应式布局
- 媒体查询
---

在css3的众多模块中，有一个与各种媒体相关的重要模块——`Media Queries`，该模块中允许添加媒体查询表达式，用以指定媒体类型，然后根据媒体类型来选择应该使用的样式。换句话说，允许我们在不改变内容的情况下在样式中选择一种页面的布局以精确地适应不同的设备，从而改善用户体验。

<!-- More -->

### 一、Media Queries使用方法

`Media Queries`的使用方法如下：

```
@media 设备类型 and ( 设备特性 ) { 样式代码 } 
```

在代码的开头必须要书写`“@media”`，然后指定设备类型，也可以称之为媒体类型。css定义了10种设备类型，在此处可以指定的值与该值所代表的设备类型如下所示。

| 可以指定的值 | 设备类型
| - | -
| all | 所有设备
| screen | 电脑显示器
| print | 打印用纸或打印预览视图
| handheld | 便携设备
| tv | 电视机类型的设备
| speech | 语音和音频合成器
| braille | 忙人用点字法触觉回馈设备
| embossed | 盲文打印机
| projection | 各种投影设备
| tty | 使用固定密度字母栅格的媒介，比如电传打字机和终端

设备特性的书写方式与样式的书写方式很相似，分为两个部分，当中由冒号分割，冒号前书写设备的某种特性，冒号后书写该特性的具体值，如需要指定浏览器的窗口宽度大于400px时所使用的样式，书写方法如下：`( min-width: 400px )`

css中的设备特性共有13种，是一个类似于css属性的集合，但与css属性不同的是，大部分设备特性的指定值接受`min/max`的前缀，用来表示大于等于或小于的逻辑，以此避免使用`<`盒`>`这些字符。

| 特性 | 可指定值 | 是否允许使用min/max前缀 | 特性说明
| - | - | - | -
| width | 带单位的长度数值，例如：400px | 允许 | 浏览器窗口的宽度
| height | 带单位的长度数值，例如：200px | 允许 |浏览器窗口的高度
| device-width | 带单位的长度数值，例如：400px | 允许 | 设备屏幕分辨率的宽度
| device-height | 带单位的宽度数值，例如：200px | 允许 | 设备屏幕分辨率的高度
| orientation | 只能指定两个值：portrait或landscape | 不允许 | 浏览器窗口的方向是纵向还是横向。当窗口的高度值大于等于宽度值时，该特性值为portrait，否则为landscape
| aspect-ratio | 比例值，例如：16/9 | 允许 | 浏览器窗口的纵横比，比例值为浏览器窗口的宽度值/高度值
| device-aspect-ratio | 比例值，例如：16/9 | 允许 | 屏幕分辨率的纵横比，比例值为设备屏幕分辨率的宽度值/高度值
| color | 整数值 | 允许 | 设备使用多少位的颜色值，如果不是彩色设备，该值为0
| color-index | 整数值 | 允许 | 色彩表中的色彩数
| monochrome | 整数值 | 允许 | 单色帧缓冲期中每像素的字节数
| resolution | 分辨率值，例如：300dpi | 允许 | 设备的分辨率
| scan | 只能指定两个值：progressive或interlace | 不允许 | 电视机类型设备的扫描方式。progressive表示逐行扫描，interlace表示隔行扫描
| grid | 只能指定两个值：0或1 | 不允许 | 设备是基于栅格还是基于位图。基于栅格时该值为1，否则该值为0

使用`and`关键字来指定当某种设备类型的某种特性的值满足某个条件时所使用的样式

```css
@media screen and (max-width: 639px) { 样式代码 }
```

可以使用多条语句来将同一个样式应用到不同的设备类型和设备特性中，指定方式如下

```css
@media handheld and (min-width: 360px), screen and (min-width: 480px) { 样式代码 }
```

可以在表达式中加上`not`关键字或`only`关键字，`not`关键字表示对后面的表达式执行取反操作，书写方法如下

```css
/* 除便携设备之外的其他设备或非彩色便携设备 */
@media not handheld and (color) { 样式代码 }
/* 所有非彩色设备 */
@media all and (not color) { 样式代码 }
```

`only`关键字，让那些不支持`Media Queries`但是能够读取`Media Type`的设备的浏览器将表达式的样式隐藏起来。

```css
@media only screen and (color) { 样式代码 }
```

对于支持`Media Queries`的设备来说，将能够正确地应用央视，就仿佛`only`不存在一样；对于不支持`Media Queries`但能够读取`Media Type`的设备来说，由于先读取到`only`而不是`screen`，将忽略这个样式；对于不支持`Media Queries`的浏览器，无论是否有`only`，都将忽略这个样式。

css3中的`Media Queries`模块中也支持对外部样式表的引用，使用如下

```css
@import url(color.css) screen and (min-width: 1000px);

<link rel="stylesheet" type="text/css" media="screen and (min-width: 1000px)" href="style.css">
```

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()