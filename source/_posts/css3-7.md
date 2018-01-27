---
title: CSS3(7) 动画
date: 2018-01-22 21:56:19
categories:
- CSS3抄书笔记
tags:
- Transitions
- Animations
---

css3中，如果使用动画功能，可以使页面上的文字或画像具有动画效果，可以使背景色从一种颜色平滑过渡到另一种颜色。

<!-- More -->

css3中的动画功能分为`Transitions`功能与`Animations`功能，这两种功能都可以通过改变css的属性值来产生动画效果。

到目前为止，`Transitions`功能支持从一个属性值平滑过渡到另一个属性值，`Animations`功能支持通过关键帧的指定来在页面上产生更复杂的动画效果。

### 一、Transitions功能

`Transitions`功能通过将元素的某个属性从一个属性值在指定的时间内平滑过渡到另一个属性值来实现动画功能，可通过`transitions`属性来使用`Transitions`功能。

使用方法如下所示：

```css
transition: property duration timing-function
```

其中`property`表示对哪个属性进行平滑过渡，`duration`表示在多长时间内完成属性值的平滑过渡，`timing-function`表示通过什么方法来进行平滑过渡。

我们看一个使用示例，页面中有一个`div`元素，背景色为黄色，通过`hover`属性指定当鼠标指针停留在`div`元素上时的背景色为浅蓝色，通过`transitions`属性指定：当鼠标指针移动到`div`元素上时，在1秒钟内让`div`元素的背景色从黄色平滑过度到浅蓝色。

```css
div {
    background-color: #ffff00;
    -webkit-transition: background-color 1s linear;
    -moz-transition: background-color 1s linear;
    -o-transition: background-color 1s linear;
}

div:hover {
    background-color: #00ffff;
}
```

css3中，还有另外一种使用`Transitions`功能的方法，就是将`transitions`属性中的三个参数改写成`transition-property`、`transition-duration`、`transition-timing-function`三个属性，这三个属性的含义与属性值的制定方法与`transitions`属性中的三个参数的含义及指定方法完全相同。

**使用Transitions功能同时平滑过渡多个属性值**

可以使用`Transitions`功能同时对多个属性值进行平滑过渡。

```css
div {
    background-color: #ffff00;
    color: #000000;
    width: 300px;

    -webkit-transition: background-color 1s linear, color 1s linear, width 1s linear;
    -moz-transition: background-color 1s linear, color 1s linear, width 1s linear;
    -o-transition: background-color 1s linear, color 1s linear, width 1s linear;
}

div:hover {
    background-color: #003366;
    color: #ffffff;
    width: 400px;
}
```

---

### 二、Animations功能

`Animations`功能与`Transitions`功能相同，都是通过改变元素的属性值来实现动画效果的。它们的区别在于：使用`Transitions`功能时只能通过指定属性的开始值与结束值，然后在这两个属性值之间进行平滑过渡的方式来实现动画效果，因此不能实现比较复杂的动画效果；而`Animations`则通过定义多个关键帧以及定义每个关键帧中元素的属性值来实现更为复杂的动画效果。

我们看一个代码使用示例，一个`div`元素背景色为红色，当鼠标指针移动到`div`元素上时，元素的背景色将经历从红色到深蓝色，从深蓝色到黄色，从黄色回到红色这样一系列的变化。

```css
div {
    background-color: red;
}
@-webkit-keyframes mycolor {
    0% {
        background-color: red;
    }
    40% {
        background-color: darkblue;
    }
    70% {
        background-color: yellow;
    }
    100% {
        background-color: red;
    }
}
div:hover {
    /* 指定关键帧集合的名称 */
    -webkit-animation-name: mycolor;
    /* 指定完成整个动画的时间 */
    -webkit-animation-duration: 5s;
    /* 指定实现动画的方法 */
    -webkit-animation-timing-function: linear;
    /* 指定动画的播放次数 */
    -webkit-animation-iteration-count: infinite;
}
```

代码实现的动画中带有如下几个关键帧，通过这些关键帧之间的平滑过渡完成了动画的实现。

* 开始帧：0%

* 背景色为深蓝色的关键帧：40%

* 背景色为黄色的关键帧：70%

* 结束帧：100%

**实现动画的方法**

前面的使用示例中，我们只使用了一种实现动画的方法——`linear`。`linear`的含义是在动画从开始到结束时使用同样的速度进行各种属性值的改变，在一个动画中不改变各种属性值的改变速度。

| 方法 | 属性值的变化速度
| - | -
| linear | 在动画开始时到结束时以同样速度进行改变
| ease-in | 动画开始时速度很慢，然后速度沿曲线值进行加快
| ease-out | 动画开始时速度很快，然后沿曲线值进行放慢
| ease | 动画开始时速度很慢，然后速度沿曲线值进行加快，然后再沿曲线值放慢
| ease-in-out | 动画开始时速度很慢，然后沿曲线值进行加快，然后再沿曲线值放慢

示例：实现网页的淡入效果

```css
@-webkit-keyframes fadein {
    0% {
        opacity: 0;
        background-color: white;
    }
    100% {
        opacity: 1;
        background-color: white;
    }
}
body {
    -webkit-animation-name: fadein;
    -webkit-animation-duration: 5s;
    -webkit-animation-timing-function: linear;
    -webkit-animation-iteration-count: 1;
}
```

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()