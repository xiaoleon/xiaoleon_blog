---
title: H5(6) 拖放API
date: 2018-01-21 21:37:05
tags: 
- 读书笔记
- 前端开发
- HTML5
---

HTML5中，提供了直接支持拖放操作的API。虽然HTML5之前已经可以使用`mousedown`、`mousemove`、`mouseup`来实现拖放操作，但是这只支持浏览器内部的拖放，而在HTML5中，已经支持在浏览器与其他应用程序之间的数据互相拖动，同时也大大简化了有关于拖放方面的代码。

<!-- More -->

实现拖放的步骤:

* #### 1. 将想要拖放的对象元素的`draggable`属性设为`true`。这样才能将该元素进行拖放。另外，`img`元素与`a`元素（必须指定`href`）默认允许拖放。

* #### 2.编写与拖放有关的事件处理代码。

| 事件 | 产生事件的元素 | 描述
| - | - | -
| dragstart | 被拖放的元素 | 开始拖放操作
| drag | 被拖放的元素 | 拖放过程中
| dragenter | 拖放过程中鼠标经过的元素 | 被拖放的元素开始进入本元素的范围内
| dragover | 拖放过程中鼠标经过的元素 | 被拖放的元素正在本元素范围内移动
| dragleave | 拖放过程中鼠标经过的元素 | 被拖放的元素离开本元素的范围
| drop | 拖放的目标元素 | 有其他元素被拖放到了本元素中
| dragend | 拖放的对象元素 | 拖放操作结束

* #### 3.DataTransfer对象的属性和方法

`DataTransfer`对象的属性和方法可以实现定制拖放图标，让它只支持特定拖放（譬如拷贝/移动/）等，甚至可以实现更复杂的拖放操作

* #### 4.设定拖放时的视觉效果

`dropEffect`属性与`effectAllowed`属性结合起来可以设定拖放时的视觉效果。

`effectAllowed`属性表示当一个元素被拖动时所允许的视觉效果，一般在`ondragstart`事件中设定，允许设定的值为`none`、`copy`、`copyLink`、`copyMove`、`link`、`linkMove`、`move`、`all`、`uninitialize`。

dropEffect属性表示实际拖放时的视觉效果，一般在`ondragover`事件中指定，允许设定的值为`none`、`copy`、`link`、`move`。`dropEffect`属性所表示的实际视觉效果必须在`effectAllowed`属性所表示的允许的视觉效果范围内。

> * 1) 如果`effectAllowed`属性设定为`none`，则不允许拖放元素
> * 2) 如果`dropEffect`属性设定为`none`，则不允许被拖放到目标元素中
> * 3) 如果`effectAllowed`属性设定为`all`或不设定，则`dropEffect`属性允许被设定为任何值，并且按指定的视觉效果进行显示
> * 4) 如果`effectAllowed`属性设定为具体效果（不为`none`、`all`），`dropEffect`属性也设定了具体视觉效果，则两个具体效果值必须完全相等，否则不允许将被拖放元素拖放到目标元素中。

```js
source.addEventListener('dragstart', function(ev) {
    var dt = ev.dataTransfer;
    dt.effectAllowed = 'copy';
    dt.setData('text/plain', 'hello world');
}, false);

dest.addEventListener('dragover', function(ev) {
    var dt = ev.dataTransfer;
    dt.dropEffect = 'copy';
    ev.preventDefault();
}, false);
```

* #### 5.自定义拖放图标

HTML5中允许自定义拖放图标——指的是在用鼠标拖动元素的过程中，位于鼠标指针下部的小图标。

`DataTransfer`对象有一个`setDragImage`方法，该方法由三个参数，第一个参数`image`设定为拖放图标的图标元素，第二个参数x为拖放图标离鼠标指针的x轴方向的位移量，第三个参数y为拖放图标离鼠标指针的y轴方向的位移量。

```javascript
var dragIcon = document.createElement('img');
dragIcon.src = 'http://twivatar.org/twitter/mini';
source.addEventListener('dragstart', function(ev {
    var dt = ev.dataTransfer;

    dt.setDragImage(dragIcon, -10, -10);
    dt.setData("text/plain", "aaa");
}), false);
```