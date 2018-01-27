---
title: H5(9) 离线Web应用程序
date: 2018-01-21 22:00:12
categories:
- HTML5抄书笔记
tags:
- 本地缓存
- 离线Web应用程序
---

Web应用程序已经变得越来越复杂，越来越成熟了，很多领域都在利用着Web应用程序。但是，它有一个致命的缺点：如果用户没有和Internet建立连接，他就不能利用这个Web应用程序了。

<!-- More -->

因此，HTML5中新增了一个API，它使用一个本地缓存机制很好地解决了这个问题，为离线Web应用程序的开发提供了可能性。

为了让Web应用程序在离线状态时候也能正常工作，就必须要把所有构成Web应用程序的资源文件，诸如HTML文件、CSS文件、Javascript脚本文件等放在本地缓存中，当服务器没有和Internet建立连接的时候，也可以利用本地缓存中的资源文件来正常运行Web应用程序。

### 一、本地缓存与浏览器网页缓存的区别

本地缓存是为整个Web应用程序服务的，而浏览器的网页缓存只服务于单个网页。任何网页都具有网页缓存，而本地缓存只缓存那些你指定缓存的网页。

网页缓存是不安全、不可靠的，因为我们不知道在网站中到底缓存了哪些网页，以及缓存了网页上的哪些资源。而本地缓存是可靠的，我们可以控制对哪些内容进行缓存，不对哪些内容进行缓存，开发人员还可以用编程的手段来控制缓存的更新，利用缓存对象的各种属性、状态和事件来开发出更为强大的离线应用程序。

---

### 二、manifest文件

Web应用程序的本地缓存是通过每个页面的`manifest`文件来管理的。`manifest`文件是一个简单文本文件，在该文件中以清单的形式列举了需要被缓存或不需要被缓存的资源文件的文件名称，以及这些资源文件的访问路径。

可以为每一个页面单独指定一个`manifest`文件，也可以对整个Web应用程序指定一个总的`manifest`文件。

```manifest
CACHE MANIFEST
#version 7
CACHE:
other.html
hello.js
images/myphoto.jpg
NETWORK:
http://lulingniu/NotOffline
NotOffline.asp
*
FALLBACK:
online.js locale.js
CACHE:
newhello.html
newhello.js
```

在manifest文件中，第一行必须是“`CACHE MANIFEST`”文字，以把本文件的作用告知给浏览器，即对本地缓存中的资源文件进行具体设置。同时，真正运行或测试离线Web应用程序的时候，需要对服务器进行配置，让服务器支持`text/cache-manifest`这个MIME类型。

例如对Apache服务器进行配置的时候，需要找到`{apache_home}/conf/mime.types`这个文件，并在文件最后添加代码`text/cache-manifest   manifest`

在指定资源文件的时候，可以把资源文件分为三类，分别是`CACHE`、`NETWORK`、`FALLBACK`：

* `CACHE`类别中指定需要被缓存在本地的资源文件。为某个页面指定需要本地缓存的资源文件时，不需要把这个页面本身指定在`CACHE`类别中，因为如果一个页面具有`manifest`文件，浏览器会自动对这个页面进行本地缓存

* `NETWORK`类别为显式指定不进行本地缓存的资源文件，这些资源文件只有当客户端与服务器端建立连接的时候才能访问。本示例中该类别的`“*”`为通配符，表示没有在本`manifest`文件中指定的资源文件都不进行本地缓存

* `FALLBACK`类别中的每行中指定两个资源文件，第一个资源文件为能够在线访问时使用的资源文件，第二个资源文件为不能在线访问时使用的备用资源文件。

每个类别都是可选的，但是如果文件开头没有指定类别而直接书写资源文件时，浏览器把这些资源文件视为`CACHE`类别，直到看见文件中第一个被书写出来的类别为止。

为了让浏览器能够正常阅读该文本文件，需要在Web应用程序页面上的html标签的`manifest`属性中指定`manifest`文件的url地址。

```html
<!-- 可以为每个页面单独指定一个manifest文件 -->
<html manifest="hello.manifest">
    ...
</html>

<!-- 也可以为整个Web应用程序指定一个总的manifest文件 -->
<html manifest="global.manifest">
    ...
</html>
```

通过这些步骤，将资源文件保存到本地缓存去的基本操作就完成了。当需要对本地缓存去的内容进行修改时，只需要修改`manifest`文件就可以了。文件被修改后，浏览器可以自动检查`manifest`文件，并自动更新本地缓存区的内容。

---

### 三、浏览器与服务器的交互过程

当使用离线Web应用程序进行工作的时候，有必要理解一下浏览器与服务器的交互过程。

例如一个`http://lulingniu`网站，以`index.html`为主页，`index.manifest`文件为`manifest`文件，在该文件中请求本地缓存`index.html`、`hello.js`、`hello1.jpg`、`hello2.jpg`这几个资源文件。

**交互过程**：

#### 1. 首次访问

* 1) 浏览器请求访问`http://lulingniu`

* 2) 服务器返回`index.html`网页

* 3) 浏览器解析`index.html`网页，请求页面上所有资源文件，包括HTML文件、图像文件、CSS文件、JS文件，以及`manifest`文件

* 4) 服务器返回所有资源文件

* 5) 浏览器处理`manifest`文件，请求`manifest`中所有要求本地缓存的文件，包括`index.html`。如果要求本地缓存所有文件，这将是一个比较大的重复的请求过程

* 6) 服务器返回所有要求本地缓存的文件

* 7) 浏览器对本地缓存进行更新，存入包括页面本身在内的所有要求本地缓存的资源文件，并且触发一个事件，通知本地缓存被更新

#### 2. 再次访问（manifest文件未更新）

* 1) 浏览器再次请求访问`http://lulingniu`

* 2) 浏览器发现这个页面被本地缓存，于是使用本地缓存中的`index.html`页面

* 3) 浏览器解析`index.html`页面，使用所有本地缓存中的资源文件

* 4) 浏览器向服务器请求`manifest`文件

* 5) 服务器返回一个304代码，通知浏览器`manifest`没有发生变化

#### 3. 再次访问（manifest文件已更新）

* 1) 浏览器再次请求访问`http://lulingniu`

* 2) 浏览器发现这个页面被本地缓存，于是使用本地缓存中的`index.html`页面

* 3) 浏览器解析`index.html`页面，使用所有本地缓存中的资源文件

* 4) 浏览器向服务器请求`manifest`文件

* 5) 服务器返回更新过的`manifest`文件

* 6) 浏览器处理`manifest`文件，发现该文件已被更新，于是请求所有要求进行本地缓存的资源文件，包括`index.html`页面本身

* 7) 浏览器返回要求进行本地缓存的资源文件

* 8) 浏览器对本地缓存进行更新，存入所有新的资源文件。并且触发一个事件，通知本地缓存被更新。

需要注意的时，即使资源文件被修改过了，上面的第3步中已装入的资源文件是不会发生变化的，譬如图片不会突然变成新的图片，脚本文件也不会突然使用新的脚本文件，也就是说，这时更新的本地缓存中的内容还不能被使用，只有重新打开这个页面的时候才会使用更新过后的资源文件。

另外，如果不想修改`manifest`文件中对于资源文件的设置，但是对服务器上请求缓存的资源文件进行了修改，那么可以通过修改版本号的方式让浏览器认为`manifest`文件已经被更新过了，以便重新下载修改过的资源文件。

---

### 四、applicationCache对象

`applicationCache`对象代表了本地缓存，可以用它来通知本地缓存中已经被更新，也允许用户手工更新本地缓存。

当浏览器对本地缓存进行更新，装入新的资源文件时，会触发`applicationCache`对象的`updateready`事件，通知本地缓存已被更新。我们可以利用这个事件告诉用户本地缓存已经被更新，用户需要手工刷新页面来得到最新版本的应用程序。

```javascript
applicationCache.onUpdateReady = function() {
    // 本地缓存已被更新，通知用户
    alert('本地缓存已被更新，您可以刷新页面来得到本程序的最新版本。');
}
```

#### 1. swapCache方法

`swapCache`方法用来手工执行本地缓存的更新，它只能在`applicationCache`对象的`updateReady`事件被触发时调用，`updateReady`事件只有在服务器的`manifest`文件被更新，并且把`manifest`文件中所要求的资源文件下载到本地后触发。顾名思义，这个事件的含义是“本地缓存准备被更新”。

当这个事件被触发后，我们可以用`swapCache`方法来手工进行本地缓存的更新。

例如，如果本地缓存的容量非常大（譬如超过100M），本地缓存的更新工作将需要相对较长的时间，而且还会把浏览器给锁住。这时最好有一个提示，告诉用户正在进行本地缓存的更新。

```js
applicationCache.onUpdateReady = function() {
    // 本地缓存已被更新，通知用户
    alert('正在更新本地缓存...');
    applicationCache.swapCache();
    alert('本地缓存已被更新，您可以刷新页面来得到本程序的最新版本。');
}
```

那么，如果我们不调用`swapCache`方法会怎么样？本地缓存就不会被更新了吗？答案时否定的，但是更新的时间不一样。如果不调用`swapCache`方法，本地缓存将在下一次打开本页面时被更新；如果调用`swapCache`方法的话，本地缓存将会立刻更新。因此，我们可以使用`confirm`方法让用户自己选择更新的时机——是立刻更新，还是在下次打开画面时再更新，特别是当他们有可能正在页面上执行一个较大的操作的时候。

另外，尽管使用`swapCache`方法立刻更新了本地缓存，但是并不意味着我们页面上的图像和脚本文件也会立刻更新，它们都是在重新打开本页面时才会生效。

#### 2. update方法

`applicationCache`的`update`方法，作用是检查服务器上的`manifest`文件是否有更新，如果有更新，浏览器会自动下载`manifest`文件中所有请求本地缓存的资源文件，当这些资源文件下载完毕时，会触发`updateReady`事件。

```html
<!-- html --> 

<!doctype HTML>
<html manifest="swapCache.manifest">
    <head>
        <meta charset='utf-8'>
        <title>swapCache方法实例</title>
        <script src="script.js"></script>
    </head>
    <body onload="init()">
        <p>swapCache方法示例</p>
    </body>
</html>

```

```javascript
// script.js

function  init() {
    setInterval(function() {
        // 手工检查manifest是否有更新
        applicationCache.update();
    }, 5000);

    applicationCache.addEventListener('updateready', function() {
        if (confirm('本地缓存已被更新，需要刷新画面来获取应用程序最新版本，是否刷新？')) {
            applicationCache.swapCache();
            location.reload();
        }
    }, true);
}
```

```manifest
# swapCache.manifest

CACHE MANIFEST
#version 1.20
CACHE:
script.js
```

---

### 五、applicationCache对象的事件

#### 1. 首次访问

* 1) 浏览器请求访问`http://lilingniu`

* 2) 服务器返回`index.html`网页

* 3) 浏览器发现该网页具有`manifest`属性，触发`checking`事件，检查`manifest`文件是否存在。不存在时，触发`error`事件，表示`manifest`文件未找到，不执行步骤6开始的交互过程

* 4) 浏览器解析`index.html`网页，请求页面上的所有资源文件

* 5) 服务器返回所有资源文件

* 6) 浏览器处理`manifest`文件，请求`manifest`中所有要求本地缓存的文件，包括`index.html`页面本身，即使刚才已经请求过该文件。如果要求本地缓存所有文件，这将是一个比较大的重复的请求过程

* 7) 服务器返回所有要求本地缓存的文件

* 8) 浏览器触发`downloading`事件，然后开始下载这些资源。在下载的同时，周期性地触发`progress`事件，开发人员可以用编程的手段获取多少文件已被下载，多少文件仍然处于下载队列等信息

* 9) 下载结束后触发`cached`事件，表示首次缓存成功，存入所有要求本地缓存的资源文件

#### 2. 再次访问

* 1) 步骤1-5同上，步骤5执行完之后，浏览器将核对`manifest`文件是否被更新。

* 2) 若没有被更新，触发`noupdate`事件，步骤6开始的交互过程不会被执行。

* 3) 若发生了更新，将继续执行后面的步骤，在步骤9中不触发`cached`事件，而是触发`updateready`事件，这表示下载结束，可以通过刷新页面来使用更新后的本地缓存，或调用`swapCache`方法来立刻使用更新后的本地缓存。

* 4) 另外，在访问缓存名单时如果返回一个HTTP 404错误，或者410错误，则触发`obsolete`事件。

* 5) 在整个过程中，如果任何与本地缓存有关的处理中发生错误的话，都会触发`error`事件。

```manifest
CACHE MANIFEST
#version 1.0
CACHE:
index.html
```

```html
<html lang="en" manifest="applicationCacheEvent.manifest">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script>
        function init() {
            var msg = document.getElementById('msg');
            applicationCache.addEventListener('checking', function () {
                msg.innerHTML += 'checking <br>';
            }, true);
            applicationCache.addEventListener('noupdate', function () {
                msg.innerHTML += 'noupdate <br>';
            }, true);
            applicationCache.addEventListener('downloading', function () {
                msg.innerHTML += 'downloading <br>';
            }, true);
            applicationCache.addEventListener('progress', function () {
                msg.innerHTML += 'progress <br>';
            }, true);
            applicationCache.addEventListener('updateready', function () {
                msg.innerHTML += 'updateready <br>';
            }, true);
            applicationCache.addEventListener('cached', function () {
                msg.innerHTML += 'cached <br>';
            }, true);
            applicationCache.addEventListener('error', function () {
                msg.innerHTML += 'error <br>';
            }, true);
        }
    </script>
</head>
<body onload="init()">
    <h1>applicationCache事件流程示例</h1>
    <p id="msg"></p>
</body>
</html>
```

---

### 参考文献

1. [《HTML5与CSS3权威指南》]()