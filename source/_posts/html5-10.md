---
title: H5(10) 跨文档消息传输
date: 2018-01-22 21:10:17
tags: 
- 读书笔记
- 前端开发
- HTML5
---

HTML5提供了在网页文档之间相互接收与发送信息的功能。使用这个功能，只要获取到网页所在窗口对象的实例，不仅同源（域+端口号）的Web网页之间可以互相通信，甚至可以实现跨域通信。

<!-- More -->

首先，想要接受从其他的窗口那里发过来的消息，就必须对窗口对象的`message`事件进行监视，代码如下所示：

```javascript
window.addEventListener('message', function() {
    ...
}, false);
```

使用`window`对象的`postMessage`方法向其他窗口发送消息，该方法的定义如下

```javascript
otherWindow.postMessage(message, targetOrigin);
```

otherWindow为要发送窗口对象的引用，可以通过`window.open`返回该对象，或通过对`window.frames`数组指定序号或名字的方式来返回单个frame所属的窗口对象。

第一个参数为所发送的消息文本，或JSON转换后的文本。

第二个参数为接收消息的对象窗口的url地址。可以在url地址字符串中使用通配符`“*”`指定全部地址，不过建议使用准确的url地址。

```html
<!-- parent.html  http://localhost:8100/html/parent.html -->

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Parent</title>
    <style>
        div {
            margin: 10px;
        }
    </style>
</head>

<body>
    <section>
        <h1>Parent Page</h1>
        <div id="msg">
            Message Content
        </div>
        <div>
            <input id="text" type="text">
        </div>
        <div>
            <button id="button" onclick="postMessage()">发送</button>
        </div>
    </section>
    <section>
        <h1>Child Page</h1>
        <iframe src="http://localhost:8111/html/child.html" width="100%" frameborder="0"></iframe>
    </section>
    <script>
        window.addEventListener('message', function (ev) {
            if (ev.origin != 'http://localhost:8111') {
                return;
            }
            document.getElementById('msg').innerHTML = `
                从${ev.origin}那里传过来的消息：${ev.data}
            `
        }, false);

        function postMessage() {
            var msg = document.getElementById('text').value;
            var iframe = window.frames[0];
            iframe.postMessage(msg, 'http://localhost:8111/html/child.html');
        }
    </script>
</body>

</html>
```

```html
<!-- child.html  http://localhost:8111/html/child.html -->

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Child</title>
    <style>
        div {
            margin: 10px;
        }
    </style>
</head>

<body>
    <div id="msg">
        Message Content
    </div>
    <div>
        <input id="text" type="text">
    </div>
    <div>
        <button id="button" onclick="postMessage()">发送</button>
    </div>
    <script>
        var source, origin;

        window.addEventListener('message', function (ev) {
            if (ev.origin != 'http://localhost:8100') {
                return;
            }
            source = ev.source;
            origin = ev.origin;
            document.getElementById('msg').innerHTML = `
                从${ev.origin}那里来的消息：${ev.data}
            `;
        }, false);

        function postMessage() {
            var msg = document.getElementById('text').value;
            if (source && origin) {
                source.postMessage(msg, origin);
            }
        }
    </script>
</body>

</html>
```


本示例中的几个关键之处：

1. 通过对`window`对象的`message`事件进行监听，可以接收消息。

2. 通过访问`message`事件的`origin`属性，可以获取消息的发送源。

    > 发送源与网站的url地址不是同一概念，发送源只包含域名和端口号，为了不接收从其他源恶意发送过来的消息，最好对发送源做个检查。

3. 通过访问`message`事件的`data`属性，可以获取消息内容

4. 使用`postMessage`方法发送消息

5. 通过访问`message`事件的`source`属性，可以获取消息发送源的窗口对象（准确的说，应该时窗口的代理对象）

通过这种方式，可以实现网页文档与网页文档之间、端口与端口之间、域与域之间互相传递消息。