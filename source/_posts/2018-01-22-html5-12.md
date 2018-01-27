---
title: H5(12) Web Workers
date: 2018-01-22 21:19:17
categories:
- HTML5抄书笔记
tags:
- Web Workers
---

`Web Workers`是HTML5中新增的，用来在Web应用程序中实现后台处理的一项技术。

在使用HTML4与Javascript创建出来的Web程序中，因为所有的处理都是在单线程内执行的，所以如果花费的事件比较长的话，程序界面会处于长时间没有响应的状态。最恶劣的是，当时间长到一定程度的话，浏览器还会跳出一个提示脚本运行时间过长的提示框，使用户不得不中断正在执行的处理。

<!-- More -->

### 一、Web Worker介绍

为了解决这个问题，HTML5新增了一个`Web Workers API`。使用这个API，用户可以很容易地创建在后台运行的线程（在HTML5中被称为`worker`），如果将可能耗费较长时间的处理交给后台去执行的话，对用户在前台页面中执行的操作就完全没有影响了。

创建后台线程的步骤十分简单。只要在`Worker`类的构造器中，将需要在后台线程中执行的脚本文件的url地址作为参数，然后创建`Worker`对象就可以了。

```javascript
var worker = new Worker('worker.js');
```

> 要注意在后台线程中是不能访问页面或窗口对象的。如果在后台线程的脚本文件中使用到`window`对象或`document`对象，则会引起错误的发生。

另外，可以通过发送和接收消息来与后台线程互相传递数据。通过对`Worker`对象的`onmessage`事件句柄的获取可以在后台线程之中接收消息。

```javascript
worker.onmessage = function(event) {
    // 处理收到的消息
}
```

使用`Worker`对象的`postMessage`方法来对后台线程发送消息。

```javascript
worker.postMessage(message);
```

另外，同样可以通过获取`Worker`对象的`onmessage`事件句柄及`Worker`对象的`postMessage`方法在后台线程内部进行消息的接收和发送。


使用示例

```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <h1>从1到给定数值的求和示例</h1>
    输入数值：
    <input type="number" id="num">
    <button onclick="calculate()">计算</button>
    <script>
        var worker = new Worker('calculate.js');
        worker.onmessage = function (event) {
            alert(`合计值：${event.data}`);
        };

        function calculate() {
            var num = parseInt(document.getElementById('num').value, 10);
            worker.postMessage(num);
        }
    </script>
</body>

</html>
```

```javascript
// calculate.js

onmessage = function (event) {
    var num = event.data;
    var result = 0;
    for (var i = 0; i <= num; i++) {
        result += i;
    }
    // 向线程创建源送回消息
    postMessage(result);
}
```

---

### 二、线程嵌套

线程中可以嵌套子线程，这样的话我们可以把一个较大的后台线程切分成几个子线程，在每个子线程中各自完成相对独立的一部分工作。

```javascript
// worker1.js

onmessage = function(event) {
    var intArray = new Array(100);
    for (var i = 0; i < 100; i++) {
        intArray[i] = parseInt(Math.random() * 100);
    }
    var worker;
    worker = new Worker('worker2.js');
    worker.postMessage(JSON.stringify(intArray));
    worker.onmessage = function(event) {
        postMessage(event.data);
    }
}
```

```javascript
// worker2.js

onmessage = function(event) {
    var intArray = JSON.parse(event.data);
    var returnStr = '';
    ...
    postMessage(returnStr);
    close();
}
```

> **注意**：在子线程中向发送源发送回消息后，最好使用`close`语句关闭子线程，如果该子线程不再使用的话。

---

### 三、线程中可用的变量、函数与类

我们总体看一下在线程用的Javascript中所有可用的变量、函数与类

#### 1. self

`self`关键词用来表示本线程范围内的作用域

#### 2. postMessage(message)

向创建线程的源窗口发送消息

#### 3. onmessage

获取接收消息的事件句柄

#### 4. importScripts(urls)

导入其他Javascript文件，参数为该文件的url地址，可以导入多个文件

```javascript
importScripts('script1.js', 'scripts\script2.js', 'script\script3.js');
```

> 导入的文件必须与使用该线程的文件的页面在同一个域中，并在同一个端口中

#### 5. navigator对象

与`window.navigator`对象相似，具有`appName`、`platform`、`userAgent`、`appVersion`属性

#### 6. sessionStorage/localStorage

可以在线程中使用`Web Storage`

#### 7. XMLHttpRequest

可以在线程中处理`Ajax`请求

#### 8. Web Workers

可以在线程中嵌套线程

#### 9. setTimeout()/setInterval()

可以在线程中实现定时处理

#### 10. close

可以结束本线程

#### 11. eval()、isNaN()、escape()等

可以使用所有的Javascript核心函数

#### 12. object

可以创建和使用本地对象

#### 13. WebSockets

可以使用`WebSockets API`来向服务器发送和接收消息

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()