---
title: H5(13) Geolocation
date: 2018-01-22 21:23:54
tags:
- 读书笔记
- 前端开发
- HTML5
---

在HTML5中，为`window.navigator`对象新增了一个`geolocation`属性，可以使用`Geolocation API`来对该属性进行访问。

<!-- More -->

### 一、获取当前位置

可以使用`getCurrentPosition`方法来取得用户当前的地理位置信息，该方法如下：

```javascript
void getCurrentPosition(onSuccess, onError, options);
```

第一个参数为获取当前地理位置信息成功时所执行的回调函数

第二个参数为获取当前地理位置信息失败时所执行的回调函数（可选）

第三个参数为一些可选属性的列表（可选）

`onSuccess`回调函数中，用到了一个参数`position`，它代表一个`position`对象，我们在后面对这个对象进行介绍

```javascript
navigator.geolocation.getCurrentPosition(function(position) {
    // 获取成功时的处理
})
```

`onError`回调函数中，用到了一个`error`对象，该对象具有以下两个属性：

1. `code`属性。`code`属性为以下三个值其中之一：

    * 用户拒绝了位置服务（数字值为1）

    * 获取不到位置信息（数字值为2）

    * 获取信息超时错误（数字值为3）

2. `message`属性。

    `message`属性为一个字符串，在该字符串中包含了错误信息。

```javascript
navigator.geolocation.getCurrentPosition(
    function(position) {
        var coords = position.coords;
        showMap(coords.latitude, coords.longitude, coords.accuracy);
    },
    function(error) {
        var errorTypes = {
            1: '位置服务被拒绝',
            2: '获取不到位置信息',
            3: '获取信息超时'
        };
        alert(errorTypes[error.code] + ': 不能确定你的当前地理位置');
    }
)
```

`options`是一些可选属性的列表，包含如下：

* enableHighAccuracy

是否要求高精度的地理位置信息，这个参数在很多设备上设置了都没用，因为使用在设备上时要结合设备电量、具体地理情况来综合考虑

* timeout

对地理位置信息的获取操作做一个超时限制（单位为毫秒）。如果在改时间内未获取到地理位置信息，则返回错误

* maximumAge

对地理位置信息进行缓存的有效时间（单位为毫秒）。如果该值指定为0，则无条件重新获取新的地理位置信息

```javascript
navigator.geolocation.getCurrentPosition(
    function(position) {},
    function(error) {},
    {
        maximumAge: 60 * 1000 * 2,
        timeout: 5000
    }
)
```

---

### 二、持续监视当前地理位置信息

使用`watchPosition`方法来持续获取用户当前地理位置信息，它会定期地自动获取

```javascript
int watchCurrentPosition(onSuccess, onError, options);
```

该方法与`getCurrentPosition`方法的参数说明与使用方法相同，该方法返回一个数字，这个数字的使用与`setInterval`的返回参数值类似，可以被`clearWatch`方法使用，停止对当前地理位置信息的监视。

```javascript
void clearWatch(watchId)
```

---

### 三、position对象

通过访问`position`对象，可以得到地理位置信息

* latitude

当前地理位置的纬度

* longitude

当前地理位置的精度

* altitude

当前位置的海拔高度

* accuracy

获取到的纬度或经度的精度（单位为米）

* altitudeAccurancy

获取到的海拔高度的精度（单位为米）

* heading

设备的前进方向，以面朝正北方向的顺时针旋转角度来表示

* speed

设备的前进速度（单位为米/秒）

* timestamp

获取地理位置信息时的时间


