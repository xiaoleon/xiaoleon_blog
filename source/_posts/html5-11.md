---
title: H5(11) Web Sockets通信
date: 2018-01-22 21:14:50
tags:
- 读书笔记
- 前端开发
- HTML5
---

`Web Sockets`是HTML5提供的在Web应用程序客户端与服务器端之间进行的非HTTP的通信机制。它实现了用HTTP不容易实现的服务器端的数据推送等智能通信技术，因此受到了高度关注。

使用`Web Sockets API`可以在服务器与客户端之间建立一个非HTTP的双向连接。这个连接时实时的，也是永久的，除非被显式关闭。这意味着当服务器想向客户端发送数据时，可以立即将数据推送到客户端的浏览器中，无须重新建立连接。只要客户端有一个被打开的`socket`（套接字）并且与服务器建立了连接，服务器就可以把数据推送到这个`socket`上，服务器不再需要轮训客户端的请求，从被动转为了主动。

<!-- More -->

### 一、使用Web Sockets API

`Web Sockets`的API本身非常简单，将url字符串作为参数，然后调用`WebSocket`对象的构造器来建立与服务器之间的通信连接

```javascript
var webSocket = new WebSocket('ws://localhost/socket');
```

url字符串必须以`“ws”`或`“wss”`（加密通信时）文字作为开头。这个url字符串被设定好之后，在JS脚本中可以通过访问`WebSocket`对象的`url`属性来重新获取。

通信建立好之后，就可以进行客户端与服务器端的双向通信了。使用`WebSocket`对象的`send`方法对服务器发送数据，只能发送文本数据，但是可以使用JSON对象把任何Javascript对象转换成文本数据后进行发送

```javascript
webSocket.send('data');
```

通过获取`onmessage`事件的句柄来接收服务器传过来的数据

```javascript
webSocket.onmessage = function(event) {
    var data = event.data;
    ...
}
```

通过获取`onopen`事件句柄来监听`socket`的打开事件

```javascript
webSocket.onopen = function(event) {
    // 开始通信时的处理
}
```

通过获取`onclose`事件句柄来监听`socket`的关闭事件

```javascript
webSocket.onclose = function(event) {
    // 通信结束时的处理
}
```

通过`close`方法来关闭`socket`，切断通信连接

```javascript
webSocket.close();
```

另外，可以通过读取`readyState`的属性值来获取`WebSocket`对象的状态，`readyState`属性存在以下几种值：

* `CONNECTING`（数字值为0） —— 正在连接

* `OPEN`（数字值为1） —— 已建立连接

* `CLOSING`（数字值为2） —— 正在关闭连接

* `CLOSED`（数字值为3） —— 已关闭连接