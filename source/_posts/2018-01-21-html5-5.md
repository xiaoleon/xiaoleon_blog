---
title: H5(5) 文件API
date: 2018-01-21 21:19:14
categories:
- HTML5抄书笔记
tags:
- File对象
- Blob对象
- FileReader接口
---

HTML5中提供了一个关于文件操作的文件API，通过使用这个API，对于从Web页面上访问本地文件系统的相关处理会变得十分简单。

<!-- More -->

### 一、FileList与file对象

`FileList`对象表示用户选择的文件列表。HTML5中，通过添加`multiple`属性，file控件内允许一次放置多个文件。控件内每一个用户选择的文件都是一个file对象，而`FileList`对象则为这些file对象的列表。

file对象由两个属性，`name`属性表示文件名，不包括路径，`lastModifiedDate`属性表示文件的最后修改日期。

---

### 二、Blob对象

`Blob`表示二进制原始数据，它提供一个`slice`方法，可以通过该方法访问到字节内部的原始数据块。事实上，上面的file对象也继承了这个`Blob`对象。

`Blob`对象由两个属性，`size`属性表示一个`Blob`对象的字节长度，`type`属性表示`Blob`的`MIME`类型，如果是未知类型，则返回一个空字符串。

对于图像类型的文件，`Blob`对象的`type`属性都是以`“image/”`开头的，后跟图像类型，利用`cite`型我们可以在Javascript中判断用户选择的文件是否为图像文件。

另外，HTML5中对file控件添加了`accept`属性，这样file控件只能接受某种类型的文件。

```html
<input type="file" id="file" accept="image/*">
```

---

### 三、FileReader接口

`FileReader`接口主要用来把文件读入内存，并且读取文件中的数据。`FileReader`接口提供了一个异步API，使用该API可以在浏览器主线程中访问文件系统，读取文件中的数据。

#### 1. FileReader接口的方法

`FileReader`接口拥有4个方法。需要注意的是：无论读取成功或失败，方法并不会返回读取结果，这一结果存储在`result`属性中。

| 方法名 | 参数 | 描述  
| - | - | - 
| readAdBinaryString | file | 将文件读取为二进制码
| readAsText | file, [encoding] | 将文件读取为文本
| readAdDataURL | file | 将文件读取为DataURL
| abort | (none) | 中断读取操作


#### 2. FileReader接口的事件

`FileReader`接口包含了一套完整的事件模型，用于捕获读取文件时的状态。

| 事件 | 描述
| - | -
| onabort | 数据读取中断时触发
| onerror | 数据读取出错时触发
| onloadstart | 数据读取开始时触发
| onprogress | 数据读取中
| onload | 数据读取成功完成时触发
| onloadend | 数据读取完成时触发，无论成功或失败

```javascript
var file = document.getElementById('file').files[0];
var reader = new FileReader();
reader.readAsText(file);
reader.onload = function(f) {
    var result = document.getElementById('result');
    result.innerHTML = this.result;
}
```

> `FileReader`对象读取到的数据都保存在了`result`属性中

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()