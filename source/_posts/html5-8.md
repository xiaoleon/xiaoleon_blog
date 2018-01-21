---
title: H5(8) Web Storage与本地数据库
date: 2018-01-21 21:53:48
tags: 
- 读书笔记
- 前端开发
- HTML5
---

`Web Storage`存储机制是对HTML4中`cookies`存储机制的一个改善。由于`cookies`存储机制有很多缺点，HTML5中不再使用它，转而使用改良后的`Web Storage`存储机制。

本地数据库是HTML5中新增的一个功能，使用它可以在客户端本地建立一个数据库——原本需要保存在服务器端数据库中的内容现在可以直接保存在客户端本地了，这大大减轻了服务器端的负担，同时也加快了访问数据的速度。

<!-- More -->

### 一、Web Storage

`Cookies`存储永久数据存在以下几个问题：

* 大小：`cookies`的大小被限制在4KB

* 带宽：`cookies`是随HTTP事务一起被发送的，因此会浪费一部分发送`cookies`时使用的带宽

* 复杂性：要正确的操纵`cookies`是很困难的

`Web Storage`，顾名思义，就是在Web上存储数据的功能，这里的存储是针对客户端本地而言的，具体来说，又分为两种

* sessionStorage

将数据保存在`session`对象中。所谓`session`，是指用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览这个网站所花费的时间。`session`对象可以用来保存在这段时间内所要求保存的任何数据

* localStorage

将数据保存在客户端本地的硬件设备中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用

两者区别在于，`sessionStorage`为临时保存，而`localStorage`为永久保存。

读写数据时使用的基本方法：

* sessionStorage

保存数据：`sessionStorage.setItem(key, value)`

读取数据：`var value = sessionStorage.getItem(key)`

* localStorage

保存数据：`localStorage.setItem(key, value)`

读取数据：`var value = localStorage.getItem(key)`

---

### 二、本地数据库

HTML5中可以通过sql语句来访问本地的sqlite数据库，有两个必要的步骤

1. 创建访问数据库的对象

2. 使用事务处理

```javascript
var db = openDatabase('mydb', '1.0', 'Test DB', 2 * 1024 * 1024);
```

方法中的四个参数，第一个为数据库名，第二个为版本号，第三个为数据库的描述，第四个为数据库的大小。该方法返回创建后的数据库访问对象，如果该数据库不存在，则创建该数据库。

实际访问数据库的时候，还需要调用`transaction`方法，来执行事务处理。使用事务处理，可以防止在对数据库进行访问及执行有关操作的时候受到外界的打扰，因为在web上，同时会有许多人都在对页面进行访问。如果在访问数据库的过程中，正在操作的数据被别的用户给修改掉的话，会引起很多意想不到的后果。因此，可以使用事务来达到在操作完了之前，阻止别的用户访问数据库的目的。

transaction方法的使用如下

```javascript
db.transaction(function(tx) {
    tx.executeSql('create table if not exists logs (id unique, Log)');
})
```

`transaction`方法使用一个回调函数为参数。在这个函数中，执行访问数据库的语句。

`executeSql`方法的完整定义如下：

```javascript
transaction.executeSql(sqlquery, [], dataHandler, errorHandler);
```

* 第一个参数为需要执行的SQL语句

* 第二个参数为SQL语句中所有使用到的参数的数组

```javascript
transaction.executeSql('update people set age=? where name=?;', [age, name]);
```

* 第三个参数为执行SQL语句成功时调用的回调函数

```javascript
function dataHandler(transaction, results) {
    // 执行SQL语句成功后的处理
}
```

* 第四个参数为执行SQL语句出错时调用的回调函数

```javascript
function errorHandler(transaction, errmsg) {
    // 执行SQL语句出错时的处理
}
```

