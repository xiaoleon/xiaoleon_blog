---
title: CSS3(5) 背景边框相关样式
date: 2018-01-22 21:49:17
tags:
- 读书笔记
- 前端开发
- CSS3
---

在css3中，追加了一些与背景相关的属性，如下表所示

| 属性 | 功能
| - | -
| background-clip | 指定背景的显示范围
| background-origin | 指定绘制背景图像时的起点
| background-size | 指定背景中图像的尺寸
| background-break | 指定内联元素的背景图片进行平铺时的循环方式

 <!-- More -->

### 一、指定背景的显示范围

在HTML页面中，一个具有背景的元素通常由元素的内容、内部补白（`padding`）、边框、外部补白（`margin`）构成。

元素背景的显示范围在css2、css2.1、css3中并不相同：

1. css2中背景的显示范围是指内部补白之内的范围，不包括边框

2. css2.1至css3中，背景的显示范围是指包括边框在内的范围

3. css3中，可以使用`background-clip`来修改背景的显示范围，如果将`background-clip`设定为`border-box`，则背景范围包括边框区域，如果设定为`padding-box`或`content-box`，则不包括边框区域。

---

### 二、指定绘制背景图像的绘制起点

在绘制背景图像时，默认是从内部补白（`padding`）区域的左上角开始，但是可以利用`background-origin`属性来指定绘制时从边框的左上角开始，或者从内容的左上角开始。

> 在firefox浏览器中，需要书写成“`-moz-background-origin`”形式；在safari浏览器或chrome浏览器中指定绘制起点时，需要书写成“`-webkit-background-origin`”形式。

`background-origin`属性为`border-box`、`padding-box`、`content-box`，分别代表从边框的左上角、内容补白区域的左上角或内容的左上角开始绘制。

---

### 三、指定背景图像的尺寸

在css3中，可以使用`background-size`属性来指定背景图像的尺寸。

```css
background-size: 40px 20px;
```

其中第一个参数为图像的宽度，第二个参数为图像的高度，中间用半角空格进行分隔。如果要维持图像横纵比的话，可以将另一个参数设定为`auto`。

---

### 四、指定内联元素背景图片进行平铺时的循环方式

在css3中，可以使用`background-break`属性来指定平铺内联元素背景图像时的循环方式，可以指定`bounding-box`、`each-box`、`continuous`这三种循环方式。

将`background-break`属性指定为`bounding-box`时，背景图像在整个内联元素中进行平铺。指定为`each-box`时，背景图像在每一行中进行平铺。指定`continuous`的时候，下一行中的图像紧接着上一行中的图像继续平铺。

---

### 五、在一个元素中显示多个背景图像

在css3中可以在一个元素里显示多个背景图像，还可以将多个背景图像进行重叠显示，从而使得背景图像中所用素材的调整变得更加容易。

```css
div {
    background-image: url(flower-red.png), url(flower-green.png), url(sky.jpg);
    background-repeat: no-repeat, repeat-x, no-repeat;
    background-position: 3% 98%, 85%, center center, top;
}
```

使用`background-image`属性指定图像文件的时候，时按在浏览器中显示时图像叠放的顺序从上往下指定的，第一个图像文件是放在最上面的，最后制定的文件是放在最下面的。

允许多重制定并配合着多个图像文件一起利用的属性有如下几个：

```
background-image  background-repeat  background-position  
background-clip  background-origin  background-size
```

---

### 六、border-radius属性

在css3中，使用`border-radius`属性指定好圆角的半径，就可以绘制圆角边框了。

```css
div {
    border-radius: 40px 20px;
}
```

如果使用了`border-radius`属性但是把边框设定为不显示的时候，浏览器将把背景的四个角绘制为圆角。

使用了`border-radius`属性后，不管边框是什么种类，都会将边框沿着圆角曲线进行绘制。

---

### 七、border-image属性

css3中增加了一个`border-image`属性，可以让处于随时变化状态的元素的长或款的边框统一使用一个图像文件来绘制。使用`border-image`属性，会让浏览器在显示图像边框时，自动将所使用到的图像分割为9部分进行处理，这样就不需要页面再另外进行人工处理了。

```css
div {
    border-image: url(borderimage.png) 20 20 20 20 / 20px;
}
```

`border-image`属性值中至少必须指定五个参数，其中第一个参数为边框所使用的图片文件的路径，后面四个参数表示当浏览器自动把边框所使用到的图像进行分隔时的上边距、右边距、下边距及左边距。

#### 1. 使用border-image属性来指定边框宽度

在css3中，除了可以使用`border`属性或`border-width`属性来指定边框的宽度外，使用`border-image`属性同样可以指定边框的宽度。

```css
border-image: url(图像文件的路径) A B C D / border-width
```

下面代码中，指定了上边距、右边距、下边距和左边距为18px，4条边分别为5px、10px、15px、20px。

```css 
div {
    border: solid;
    border-image: url(borderimage.png) 18 / 5px 10px 15px 20px;
}
```

#### 2. 中央图像的自动拉伸

浏览器将边框所用图像自动分割为9部分后，除了将`border-top-image`、`border-left-image`、`border-right-image`、`border-bottom-image`这四部分自动分配为四条边所用的图像之外，将位于中间部分的图像分配给元素边框所包围的中间区域，随着`div`元素内容变化的同时，或者在样式代码中修改div元素的宽度或高度的同时，中间部分的图像也会自动进行伸缩，以填满该中间区域。

#### 3. 指定四条边中图像的显示方法

可以在`border-image`属性中指定元素四条边中的图像是以拉伸的方式显示，还是以平铺的方式显示

```css
border-image: url(文件路径) A B C D / border-width topbottom leftright
```

其中，`topbottom`表示元素的上下两条边中图像的显示方法，`leftright`表示元素的左右两条边中的显示方法，显示方法中可以指定的值为`repeat`、`stretch`与`round`三种。

* `repeat`：图像以平铺的方式进行显示

* `stretch`：图像以拉伸的方式进行显示

* `repeat+stretch`：将上下两条边中的图像的显示方式指定为平铺显示，左右两条边中的图像的显示方式指定为拉伸显示，或者上下两条边中的图像的显示方式指定为拉伸显示，左右两条边中的图像的显示方式指定为平铺显示。

* `round`：与`repeat`类似，都是将图像进行平铺显示，区别在于如果最后显示的衣服图像不能被完全显示，且能够现实的部分不到图像的一半，就不显示最后的图像，然后扩大前面的图像，使显示区域正好完整平铺全部图像；如果能够显示的部分超过图像的一半，就显示最后的图像，但是将全部显示的图像缩小，使显示区域正好完整平铺全部图像。