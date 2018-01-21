---
title: H5(4) 新增元素
date: 2018-01-21 21:17:20
tags: 
- 读书笔记
- 前端开发
- HTML5
---

<!-- More -->

#### 1. figure

figure元素所表示的内容通常是图片、统计图或代码示例，但并不仅限于此，它同样可以用来表示音频插件、视频插件或统计表格等。

```html
<figure>
    <img src="1.jpg" alt=''>
    <img src="2.jpg" alt=''>
    <img src="3.jpg" alt=''>
    <figcaption>hello world</figcaption>
</figure>
```

#### 2. details

details元素提供了一种替代Javascript的、将画面上局部区域进行展开或收缩的方法。

```html
<details>
    <summary>This is title</summary>
    <p>This is content</p>
</details>
```

#### 3. mark

mark元素表示页面中需要突出显示或高亮显示的，对于当前用户具有参考作用的一段文字。它通常是用于引用原文的时候，目的是引起读者的注意。

```html
<p><mark>HTML5</mark>是近十年来Web开发标准最巨大的飞跃</p>
```

#### 4. progress

progress元素代表一个任务的完成进度，这个进度可以是不确定的，只是表示进度正在进行，但不清楚还有多少工作量没有完成，也可以用0到某个最大数字（例如100）之间的数字来表示准确的进度完成情况（例如进度百分比）

```html
<progress id="p" max=100 value=60></progress>
```

#### 5. meter

meter元素表示规定范围内的数量值。包含如下六个属性：

1. value：在元素中特地表示出来的实际值
2. min：指定规定的范围时允许使用的最小值，默认为0
3. max：指定规定的范围时允许使用的最大值，默认为1
4. low：规定范围的下限值，必须小于或等于high属性的值
5. high：规定范围的上限值，必须大于或等于low属性的值
6. optimum：最佳值，属性值必须在min和max之间

```html
<p>
    磁盘使用量：
    <meter value="40" min="0" max="160">40/160</meter>GB
</p>
```

#### 6. menu与command

menu和command这两个元素是HTML5中新增的用于Web应用程序的元素，它用于菜单、工具条及弹出菜单。其中menu元素相当于菜单，而command元素相当于菜单项。

#### 7. ol列表

ol元素中添加了start属性和reversed属性

1. start属性。如果不想ol元素所代表的列表编号从1开始，那么可以使用start属性来自定义编号的初始值

```html
<ol start=5>
    <li>列表内容5</li>
    <li>列表内容6</li>
    <li>列表内容7</li>
    <li>列表内容8</li>
</ol>
```

2. reversed属性。如果相对列表进行反向排序，可以使用ol的reversed属性。

```html
<ol reversed>
```

#### 8. cite

cite元素表示作品（例如一本书、一篇文章、一首歌曲）的标题，HTML5中明确规定了不能用cite元素表示包括作者在内的任何人名，因为人的名字不是标题。

```html
<p>我最喜欢的电影是由甄子丹主演的<cite>精武风云</cite></p>
```

#### 9. small

HTML5中，对small进行了重新定义，使其由原来的通用展示性元素变为更具体的、专门用来标识所谓“小子印刷体”的元素，通常用在诸如免责声明、注意事项、法律规定、与版权相关的法律性声明文字中，同时不允许被应用在页面主内容中，只允许被当作辅助信息用inline方式内嵌在页面上使用。同时，small元素也不意味着元素中内容字体会变小，如果需要将字体变小，需要配合着css样式表来使用。