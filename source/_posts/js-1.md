---
title: JavaScript(1) 实现继承
date: 2018-01-22 22:20:10
tags:
- 读书笔记
- 前端开发
- JavaScript
- 原生JS
---

JS作为面向对象的弱类型语言，继承也是其非常强大的特性之一。那么如何在JS中实现继承呢？让我们拭目以待。

<!-- More -->

---

### 一、类式继承

类式继承的主要思路是：**采用构造函数实例化对象，通过原型链将实例对象关联起来**。

#### 1. 原型链继承

JavaScript使用原型链作为实现继承的主要方法，实现的本质是重写原型对象，代之以一个新类型的实例。下面的代码中，原来存在于`Super`的实例对象中的属性和方法，现在也存在于`Sub.prototype`中了

```javascript
function Super() {
    this.value = true;
}

Super.prototype.getValue = function() {
    return this.value;
};

function Sub() {}

// Sub继承自Super
Sub.prototype = new Super();
Sub.prototype.constructor = Sub;

var instance = new Sub();
console.log(instance.getValue()); // true
```

原型链最主要的问题在于包含引用类型值的原型属性会被所有实例共享，而这也是为什么要在构造函数中，而不是在原型对象中定义属性的原因。通过原型来实现继承时，原型实际上会变成另一个类型的实例。于是，原先的实例属性也就顺理成章的变成了现在的原型属性了。

```javascript
function Super() {
    this.colors = ['red', 'blue', 'green'];
}

function Sub() {};

Sub.prototype = new Super();
Sub.prototype.constructor = Sub;

var instance1 = new Sub();
instance1.colors.push('black');
console.log(instance1.colors); // 'red, blue, green, black'

var instance2 = new Sub();
console.log(instance2.colors); // 'red, blue, green, black'
```

原型链的第二个问题是，在创建子类型的实例时，不能向超类型的构造函数中传递参数。实际上，应该说是没有办法在不影响所有实例的情况下，给超类型的构造函数传递参数。再加上包含引用类型值的原型属性会被所有实例共享的问题，在实践中很少会单独使用原型链继承。

#### 2. 借用构造函数继承

借用构造函数的技术（有时候也叫做伪类继承或经典继承）。基本思想比较简单，即在子类型构造函数的内部调用超类型构造函数，通过使用`apply()`和`call()`方法在新创建的对象上执行构造函数。

```javascript
function Super() {
    this.colors = ['red', 'blue', 'green'];
}

function Sub() {
    Super.call(this); // 继承了Super
}

var instance1 = new Sub();
instance1.colors.push('black');
console.log(instance1.colors); // 'red, blue, green, black'

var instance2 = new Sub();
console.log(instance2.colors); // 'red, blue, green'
```

相对于原型链而言，借用构造函数有一个很大的优势，即可以在子类型中向超类型构造函数传递参数。

```javascript
function Super(name) {
    this.name = name;
}

function Sub() {
    Super.call(this, 'xiaozhang'); // 继承父类，同时传递参数
    this.age = '28'; // 实例属性
}

var instance = new Sub();
console.log(instance.name); // 'xiaozhang'
console.log(instance.age); // '28'
```

但是，如果仅仅是借用构造函数，那么也将无法避免构造函数模式存在的问题——方法都在构造函数中定义，因此函数复用就无从谈起了。

#### 3. 组合继承

组合集成有时也叫伪经典继承，指的是将原型链和借用构造函数的技术组合到一块，从而发挥二者之长的一种继承模式。其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样，既通过在原型上定义方法实现了函数复用，又能够保证每个实例都有它自己的属性。

```javascript
function Super(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Super.prototype.getName() {
    return this.name;
}

function Sub(name, age) {
    Super.call(this, name);
    this.age = age;
}

Sub.prototype = new Super();
Sub.prototype.constructor = Sub;
Sub.prototype.getAge = function() {
    return this.age;
}

var instance1 = new Sub('xiaozhang', 28);
instance1.colors.push('black');
console.log(instance1.colors); // 'red, blue, green, black'
console.log(instance1.getName()); // 'xiaozhang'
console.log(instance1.getAge()); // 28

var instance2 = new Sub('leon', 18);
console.log(instance2.colors); // 'red, blue, green'
console.log(instance2.getName()); // 'leon'
console.log(instance2.getAge()); // 18
```

组合继承有它自己的问题。那就是无论什么情况下，都会调用两次父类型构造函数：一次是在创建子类型原型的时候，另一次是在子类型构造函数内部。子类型最终会包含父类型对象的全部实例属性，但不得不在调用子类型构造函数时重写这些属性。


```javascript
function Super(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Super.prototype.getName() {
    return this.name;
}

function Sub(name, age) {
    // 第二次调用Super()，Sub.prototype再次获得name和colors两个属性，并对前一次的属性值进行覆盖
    Super.call(this, name);
    this.age = age;
}

// 第一次调用Super()，Sub.prototype获得name和colors两个属性
Sub.prototype = new Super();
Sub.prototype.constructor = Sub;
Sub.prototype.getAge = function() {
    return this.age;
}
```

#### 4. 寄生组合继承

解决两次调用父类型构造函数的方法是使用寄生组合式继承。计生组合式继承与组合继承相似，都是通过借用构造函数来继承不可共享的属性，通过原型链的混成形式来继承方法和可共享的属性。只不过把原型继承的形式变成了寄生式继承。使用寄生组合式继承可以不必为了指定子类型的原型而调用父类型的构造函数，从而寄生式继承只继承了父类型的原型属性，而父类型的实例属性是通过借用构造函数的方式来得到的。

```javascript
function Super(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Super.prototype.getName = function() {
    return this.name;
}

function Sub(name, age) {
    Super.call(this, name);
    this.age = age;
}

if (!Object.create) {
    Object.create = function(proto) {
        function F(){};
        F.prototype = proto;
        return new F;
    }
}

Sub.prototype = Object.create(Super.prototype);
Sub.prototype.constructor = Sub;
Sub.prototype.getAge = function() {
    return this.age;
}

var instance1 = new Sub('xiaozhang', 28);
instance1.colors.push('black');
console.log(instance1.colors); // 'red, blue, green, black'
console.log(instance1.getName()); // 'xiaozhang'
console.log(instance1.getAge()); // 28

var instance2 = new Sub('leon', 18);
console.log(instance2.colors); // 'red, blue, green'
console.log(instance2.getName()); // 'leon'
console.log(instance2.getAge()); // 18
```

这个例子的高效率体现在它只调用了一次`Super`构造函数，并且因此避免了在`Sub.prototype`上面创建不必要的、多余的属性。与此同时，原型链还保持不变。因此，开发人员普遍认为寄生组合式继承是引用类型最理想的继承方式。

#### 5. ES6中的class

采用ES6中的`class`语法糖，则上面代码修改如下：

```javascript
class Super {
    constructor(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    },

    getName() {
        return this.name;
    }
}

class Sub extends Super {
    constructor(name, age) {
        super(name);
        this.age = age;
    },

    getAge() {
        return this.age;
    }
}

var instance1 = new Sub('xiaozhang', 28);
instance1.colors.push('black');
console.log(instance1.colors); // 'red, blue, green, black'
console.log(instance1.getName()); // 'xiaozhang'
console.log(instance1.getAge()); // 28

var instance2 = new Sub('leon', 18);
console.log(instance2.colors); // 'red, blue, green'
console.log(instance2.getName()); // 'leon'
console.log(instance2.getAge()); // 18
```

ES6的`class`语法糖隐藏了许多技术细节，在实现同样功能的前提下，代码却优雅不少。

---

### 二、原型继承

#### 1. 原型继承

原型继承，又称为委托继承。道格拉斯·克罗克福德（Douglas Crockford）在2006年谢了一篇文章，《Javascript中的原型式继承》。在这篇文章中，他介绍了一种实现继承的方式，这种方式并没有使用严格意义上的构造函数。他的想法是借助原型可以基于已有的对象来创建新对象，同时不必因此创建自定义类型。

```javascript
function object(o) {
    function F() {};
    F.prototype = o;
    return new F();
}

var superObj = {
    init: function(value) {
        this.value = value;
    },
    getValue: function() {
        return this.value;
    }
}

var subObj = object(superObj);
subObj.init('sub');
console.log(subObj.getValue()); // 'sub'
```

在`object`函数内部，先创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的新实例。从本质上将，`object`方法对传入其中的对象执行了一次浅复制。ES5通过新增`Object.create`方法规范了原型式继承。

```javascript
var superObj = {
    init: function(value) {
        this.value = value;
    },
    getValue: function() {
        return this.value;
    }
}

var subObj = Object.create(superObj);
subObj.init('sub');
console.log(subObj.getValue()); // 'sub'
```

#### 2. 与原型链继承的关系

原型继承虽然只是看上去将原型链继承的一些程序性步骤包裹在函数里而已。但是，它们的一个重要区别是父类型的实例对象不再作为子类型的原型对象。

```javascript
// 1. 使用原型链继承
function Super() {
    this.value = 1;
}
Super.prototype.value = 0;

function Sub() {}

Sub.prototype = new Super();
Sub.prototype.constructor = Sub;

var instance = new Sub();
console.log(instance.value);
```

```javascript
// 2. 使用原型继承
function Super() {
    this.value = 1;
}
Super.prototype.value = 0;

function Sub() {}

Sub.prototype = Object.create(Super.prototype);
Sub.prototype.constructor = Sub;

var instance = new Sub();
console.log(instance.value);
```

原型继承中子类可以继承父类原型上的属性，但不可以继承父类的实例上的属性。原型继承与原型链继承都存在着子例共享父例引用类型值的问题。

```javascript
var superObj = {
    colors: ['red', 'blue', 'green']
};

var subObj1 = Object.create(superObj);
subObj1.colors.push('black');

var subObj2 = Object.create(superObj);
subObj2.colors.push('white');

console.log(superObj.colors); // 'red, blue, green, black, white'
console.log(subObj1.colors); // 'red, blue, green, black, white'
console.log(subObj2.colors); // 'red, blue, green, black, white'
```

#### 3. 寄生式继承

寄生式继承（`parasitic`）是与原型继承紧密相关的一种思路，并且同样是由道格拉斯·克罗克福德推而广之的。寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装继承过程的函数，该函数内部以某种方式来增强对象，最后再返回对象。

```javascript
function parasite(original) {
    var clone = Object.create(original); // 通过调用函数创建一个新对象
    clone.sayHi = function() {
        // 以某种方式来增强这个对象
        console.log('hi');
    };
    return clone; // 返回这个对象
}

var superObj = {
    colors: ['red', 'blue', 'green']
};

var subObj1 = parasite(superObj);
subObj1.colors.push('black');

var subObj2 = parasite(superObj);
subObj2.colors.push('white');

console.log(superObj.colors); // 'red, blue, green, black, white'
console.log(subObj1.colors); // 'red, blue, green, black, white'
console.log(subObj2.colors); // 'red, blue, green, black, white'
```

由于原型继承存在着引用类型的值被共享的问题，所以使用的并不多，只在一些简单的应用场景下使用。如果需要解决该问题，则需要借用构造函数，与原型继承的初衷相违背，相当于使用了类式继承的终极写法——寄生组合继承。

---

### 三、拷贝继承

拷贝继承又称为混入继承，`jQuery`中使用的就是拷贝继承。拷贝继承不需要改变原型链，通过拷贝函数将父例的属性和方法拷贝到子例即可。

#### 1. 拷贝函数

```javascript
// 深拷贝函数
function extend(obj, cloneObj) {
    if (typeof obj != 'object') {
        return false;
    }
    var cloneObj = cloneObj || {};
    for (var i in obj) {
        if (typeof obj[i] === 'object') {
            cloneObj[i] = (obj[i] instanceof Array) ? [] : {};
            arguments.callee(obj[i], cloneObj[i]);
        } else {
            cloneObj[i] = obj[i];
        }
    }
    return cloneObj;
}

var obj1 = { a: 1, b: 2, c: [1, 2, 3] };
var obj2 = extend(obj1);

console.log(obj1.c); // [1,2,3]
console.log(obj2.c); // [1,2,3]

obj2.push(4);
console.log(obj2.c); // [1,2,3,4]
console.log(obj1.c); // [1,2,3]
```

#### 2. 对象间的拷贝继承

由于拷贝继承解决了引用类型值共享的问题，所以其完全可以脱离构造函数实现对象间的继承。

```javascript
function extend(obj, cloneObj) {
    if (typeof obj != 'object') {
        return false;
    }
    var cloneObj = cloneObj || {};
    for (var i in obj) {
        if (typeof obj[i] === 'object') {
            cloneObj[i] = (obj[i] instanceof Array) ? [] : {};
            arguments.callee(obj[i], cloneObj[i]);
        } else {
            cloneObj[i] = obj[i];
        }
    }
    return cloneObj;
}

var superObj = {
    arrayValue: [1, 2, 3],
    init: function(value) {
        this.value = value;
    },
    getValue: function() {
        return this.value;
    }
};

var subObj = extend(superObj);
subObj.arrayValue.push(4);

console.log(subObj.arrayValue); // [1,2,3,4]
console.log(superObj.arrayValue); // [1,2,3]
```

#### 3. 使用构造函数的拷贝组合继承

如果要使用构造函数，则属性可以使用借用构造函数的方法，而引用类型属性和方法使用拷贝继承。相当于不再通过原型链来建立对象之间的联系，而通过复制来得到对象的属性和方法。

```javascript
function extend(obj, cloneObj) {
    if (typeof obj != 'object') {
        return false;
    }
    var cloneObj = cloneObj || {};
    for (var i in obj) {
        if (typeof obj[i] === 'object') {
            cloneObj[i] = (obj[i] instanceof Array) ? [] : {};
            arguments.callee(obj[i], cloneObj[i]);
        } else {
            cloneObj[i] = obj[i];
        }
    }
    return cloneObj;
}

function Super(name) {
    this.name = name;
    this.colors = ['red', 'blue', 'green'];
}

Super.prototype.getName = function() {
    return this.name;
}

function Sub(name, age) {
    Super.call(this, name);
    this.age = age;
}

Sub.prototype = extend(Super.prototype);

var instance1 = new Sub('xiaozhang', 28);
instance1.colors.push('black');
console.log(instance1.colors); // 'red, blue, green, black'

var instance2 = new Sub('leon', 18);
console.log(instance2.colors); // 'red, blue, green'
```

---

### 四、总结

本文介绍的 **类式继承** 、**原型继承**、**拷贝继承** 三种继承方式中，类式继承用的最普遍，由于ES6中`class`语法糖，使其代码复杂度大大降低；原型继承由于无法处理引用类型值共享的问题，使用较少，但是原型继承引申出的寄生组合继承是类式继承的规范式方法；拷贝继承使用范围最广泛，不仅可以实现原型之间的继承，也可以脱离构造函数，直接实现对象间的继承。

总之，继承主要就是处理父例和子例之间的两个问题，即是否使用构造函数，及如何建立联系。

类式继承的核心就是使用构造函数，通过原型链来建立联系

原型继承不使用构造函数，通过`Object.create()`来建立联系

拷贝继承使用或者不使用构造函数都可以，通过复制来建立联系

---

### 参考文献

1. [javascript面向对象系列](http://geek.csdn.net/news/detail/246690)