---
title: React(1) 新的前端思维方式
categories:
  - React抄书笔记
tags:
  - JavaScript
  - React
date: 2018-03-04 09:51:31
---

我们先来只管认识React，对任何一种工具，只有使用才能够熟练掌握，React也不例外。通过对React快速上手，我们会解析React的工作原理，并通过与功能相同的jQuery程序对比，从而看出React的特点。

<!-- More -->

### 一、初始化一个React项目

React是一个JavaScript语言的工具库，我们需要安装Node.js，React本身并不依赖于Node.js，但是我们开发中用到的诸多工具需要Node.js的支持。

在Node.js的官网（[https://nodejs.org](https://nodejs.org)）可以找到合适的安装方式，安装Node.js的同时也就安装了npm，npm是Node.js的安装包管理工具，因为我们不可能自己开发所有功能，会大量使用现有的安装包，就需要npm的帮助。

#### 1. create-react-app工具

React技术依赖于一个很庞大的技术栈，比如，转译JavaScript代码需要使用Babel，模块打包工具又要使用Webpack，定制build过程需要grunt或者gulp，这些技术栈都需要各自的配置文件，还没有开始写一行React相关代码，我们就已经被各种技术名词淹没。

针对这种情况，React的创建者Facebook提供了一个快速开发React应用的工具，名叫`create-react-app`，这个工具的目的是将开发人员从配置工作中解脱出来，无需过早关注这些技术栈细节，通过创建一个已经完成基本配置的应用，让开发者快速开始React应用的开发。

create-react-app是一个通过npm发布的安装包，在确认Node.js和npm安装好之后，命令行中执行下面的命令安装create-react-app：

```cmd
npm install -g create-react-app
```

安装结束后，我们可以通过如下命令创建react项目：

```cmd
create-react-app first_react_app
```

这个命令会在当前目录创建一个名为first_react_app的目录，在这个目录中会自动添加一个应用的框架，随后我们只需要在这个框架的基础上修改文件就可以开发React应用，避免了大量的手工配置工作。

在create-react-app命令一大段文字输出之后，根据提示，输入下面的命令：

```cmd
cd first_react_app
npm start
```

这个命令会启动一个开发模式的服务器，同时也会让浏览器自动打开一个网页，指向本机地址`http://localhost:3000`。

![第一个React应用](/images/react-1/1.png)

接下来，我们会用React开发一个简单的功能。

---

### 二、增加一个新的React组件

React的首要思想是通过组件（Component）来开发应用。所谓组件，简单说，指的是能完成某个特定功能的独立的、可重用的代码。

基于组件的应用开发是广泛使用的软件开发模式，用分而治之的方法，把一个大的应用分解成若干小的组件，每个组件只关注于某个小范围的特定功能，但是把组件组合起来，就能构成一个功能庞大的应用。如果分解功能的过程足够巧妙，那么每个组件可以在不同场景下重用，那么不光可以构建庞大的应用，还可以构建出灵活的应用。打个比方，每个组件是一块砖，而一个应用是一座楼，想要一次锻造就创建一座楼是不现实的。实际上，总是先锻造出很多砖，通过排列组合这些砖，才能构建伟大的建筑。

我们先看看create-react-app给我们自动产生的代码，在first_react_app目录下包含如下文件和目录。

![文件目录](/images/react-1/2.png)

在开发过程中，我们主要关注src目录中的内容，这个目录中是所有的源代码。

create-react-app所创建的应用的入口是`src/index.js`文件，我们看看中间的内容，代码如下：

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

这个应用所做的事情，只是渲染一个名叫App的组件，App组件在同目录下的App.js文件中定义，渲染出来的效果就是在上图中看到的界面。

我们要定义一个新的能够计算点击数组件，名叫`ClickCounter`，所以我们修改index.js文件如下：

```js
import React from 'react';
import ReactDOM from 'react-dom';
import ClickCounter from './ClickCounter';
import './index.css';

ReactDOM.render(
  <ClickCounter />,
  document.getElementById('root')
);
```

接下来我们会介绍代码的含义。现在我们先来看看如何添加一个新组建，在src目录下添加一个新的代码文件`ClickCounter.js`，代码如下：

```js
import React, { Component } from 'react';
class ClickCounter extends Component {
  constructor(props) {
    super(props);
    this.onClickButton = this.onClickButton.bind(this);
    this.state = {
      count: 0
    };
  }

  onClickButton() {
    this.setState({
      count: this.state.count + 1
    });
  }

  render() {
    return (
      <div>
        <button onClick={this.onClickButton}>Click Me</button>
        <div>
          Click Count: { this.state.count }
        </div>
      </div>
    )
  }
}
export default ClickCounter;
```

我们可以在网页中看到，其中内容已经发生改变，如下图所示

![ClickCounter组件界面效果](/images/react-1/3.png)

点击“Click Me”按钮，可以看到“Click Count”后面的数字会随之增加，每点击一次加1.

现在让我们来逐步详细解释代码中各部分的要义。

在index.js文件中，使用import导入了ClickCounter组件，代替了之前的App组件。

```js
import ClickCounter from './ClickCounter';
```

`import`是ES6语法中导入文件模块的方式，ES6语法是一个大集合，大部分功能都被最新浏览器支持。不过这个import方法却不在广泛支持之列，这没有关系，ES6语法的JavaScript会被webpack和babel转译成所有浏览器支持的ES5语法，而这一切都无需开发人员配置，create-react-app已经替我们完成了这些工作。

在ClickCounter.js文件的第一行，我们从react库中引入了React和Component，如下所示：

```js
import React, { Component } from 'react';
```

Component作为所有组件的基类，提供了很多组建共有的功能，下面这行代码，使用的是ES6语法来创建一个叫ClickCounter的组建类，ClickCounter的父类就是Component：

```js
class ClickCounter extends Component { }
```

在React出现之初，使用的是React.createClass方式来创造组件类，这种方法已经被废弃了。在本文中，我们只使用ES6的语法来构建组件类。

虽然我们导入的Component类在ClickCounter组件定义中使用了，可是导入的React却没有被使用，难道这里引入React没有必要吗？

事实上，引入React非常必要，我们可以尝试删掉第一行中的React，在网页中立刻会出现错误信息。

![缺失React的错误](/images/react-1/4.png)

这个错误的含义是：“在使用JSX的范围内必须要有React。”

也就是说，在使用JSX的代码文件中，即使代码中并没有直接使用React，也一定要导入React，这是因为JSX最终会被转译成依赖于React的表达式。

#### 1. JSX

所谓JSX，是JavaScript的语法扩展（eXtension），让我们在JavaScript中可以编写像HTML一样的代码。在ClickCounter.js的render函数中，就出现了类似这样的HTML代码，在index.js中，ReactDOM.render的第一个参数`<App />`也是一段JSX代码。

JSX中的这几段代码看起来和HTML几乎一模一样，都可以使用`<div>`、`<button>`之类的元素，所以只要熟悉HTML，学习JSX完全不成问题，但是，我们一定要明白两者的不同之处。

首先，在JSX中使用的“元素”不局限于HTML元素，可以是任何一个React组件，在App.js中可以看到，我们创建的ClickCounter组件被直接应用在JSX中，使用方法和其他元素一样，这一点是传统的HTML做不到的。

React判断一个元素是HTML元素还是React元素的原则就是看第一个字母是否大写，如果在JSX中我们不用ClickCounter而是用clickCounter，那就得不到我们想要的结果。

其次，在JSX中可以通过onClick的方式给一个元素添加一个事件处理函数，当然，在HTML中也可以使用onclick（注意和onClick拼写有区别），但在HTML中直接书写onclick一直就是为人诟病的写法，网页应用开发界一直倡导的是用jQuery的方法添加事件处理函数，直接写onclick会带来代码混乱的问题。

这就带来一个问题，既然长期以来一直不提倡在HTML中使用onclick，为什么在React的JSX中我们却要使用onClick这样的方式来添加事件处理函数呢？

#### 2. JSX是进步还是倒退

在React出现之初，很多人对React这样的设计非常反感，因为React把类似HTML的标记语言和JavaScript混在一起了，但是，随着时间的推移，业界逐渐认可了这种方式，因为大家都发现，以前用HTML来代表内容，CSS代笔样式，Javascript来定义交互行为，这三种语言分在三种不同的文件里面，实际上是把不同技术分开管理了，而不是逻辑上的“分而治之”。

根据做同一件事的代码应该有高耦合性的设计原则，既然我们要实现一个ClickCounter，那为什么不把实现这个功能的所有代码集中在一个文件里呢？

那么，在JSX中使用onClick添加事件处理函数，是否代表网页应用开发兜了一个大圈，最终回到了起点呢？

不是这样，JSX的onClick事件处理方式和HTML的onclick有很大不同。

即使现在，我们还是要说在HTML中直接使用onclick很不专业，原因如下：

* onclick添加的事件处理函数是在全局环境下执行的，这污染了全局环境，很容易产生意料不到的后果

* 给很多Dom元素添加click时间，可能会影响网页的性能，毕竟，网页需要的事件处理函数越多，性能就会越低

* 对于使用onclick的Dom元素，如果要动态地从DOM树中删掉的话，需要把对应的事件处理器注销，假如忘了注销，就可能造成内存泄漏，这样的bug很难被发现

上面说的这些问题，在JSX中都不存在。

首先，onClick挂载的每个函数，都可以控制在组建范围内，不会污染全局空间。

我们在JSX中看到一个组件使用了onClick，但并没有产生直接使用onclick的HTML，而是使用了事件委托（event delegation）的方式处理点击事件，无论有多少个onClick出现，其实最后都只在DOM树上添加了一个事件处理函数，挂在最顶层的DOM节点上。所有的点击事件都被这个事件处理函数捕获，然后根据具体组建分配给特定函数，使用事件委托的性能当然要比为每个onClick都挂载一个事件处理函数要高。

因为React控制了组件的生命周期，在unmount的时候自然能够清除相关的所有事件处理函数，内存泄漏也不再是一个问题。

除了在组件中定义交互行为，我们还可以在React组件中定义样式，我们可以修改ClickCounter.js中的render函数，代码如下：

```js
render() {
  const counterStyle = {
    margin: '16px'
  };
  return (
    <div style={counterStyle}>
      <button onClick={this.onClickButton}>Click Me</button>
      <div>
      Click Count: <span id="clickCount">{this.state.count}</span>
      </div>
    </div>
  )
}
```

我们在JavaScript代码中定义一个counterStyle对象，然后在JSX中赋值给顶层div的style属性，可以在网页中看到这个部分的margin真的变大了。

这样，React的组件可以把JavaScript、HTML和CSS的功能写在一个文件中，实现真正的组件封装。

---

### 三、分解React应用

前面我们提到过，React应用实际上依赖于一个很大很复杂的技术栈，我们使用create-react-app避免在一开始就费太多精力配置技术栈，不过现在是时候了解一下这个技术栈了。

我们启动React应用的命令是`npm start`，看看package.json中对start脚本的定义。

```json
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "react-scripts test --env=jsdom",
  "eject": "react-scripts eject"
}
```

可以看到，start命令实际上是调用了react-scripts命令，react-scripts是create-react-app添加的一个npm包，所有的配置文件都藏在node_modules/react-scripts目录下，我们当然可以钻进这个目录去一探究竟，但是也可以使用eject方法来看清楚背后的原理。

这个eject（弹射）命令做的事情，就是把潜藏在react-scripts中的一系列技术栈配置都“弹射”到应用的顶层，然后我们就可以研究这些配置细节了，而且可以更灵活地定制应用的配置。

> eject命令是不可逆的，就好像战斗机飞行员选择“弹射”出驾驶舱，等于是放弃了这架战斗机，是不可能再飞回驾驶舱的。所以，当你执行eject之前，最好做一下备份。

我们在命令行下执行下面的命令，完成“弹射”操作：

```cmd
npm run eject
```

这个命令会改变一些文件，也会添加一些文件。

当前目录下会增加两个目录，一个是scripts，另一个是config，同时，package.json文件中的scripts部分也发生了变化：

```json
"scripts": {
  "start": "node scripts/start.js",
  "build": "node scripts/build.js",
  "test": "node scripts/test.js --env=jsdom"
}
```

从此以后，start脚本将使用scripts目录下的start.js，而不是node_modules目录下的react-scripts，弹射成功，再也回不去了。

在config目录下的webpack.config.dev.js文件，定制的就是npm start所做的构造过程，其中有一段关于babel的定义：

```json
{
  "test": /\.(js|jsx)$/,
  "include": paths.appSrc,
  "loader": "babel",
  "query": {
    // This is a feature of 'babel-loader' for webpack (not Babel itself).
    // It enables caching results in ./node_modules/.cache/babel-loader/
    // directory for faster rebuilds.
    "cacheDirectory": true
  }
}
```

代码中的paths.appSrc的值就是src，所以这段配置的含义指的是所有以js或者jsx为扩展名的文件，都会由babel所处理。

并不是所有的浏览器都支持所有的ES6语法，但是有了babel，我们就可以不用顾忌太多，因为babel会把ES6语法的JavaScript代码转译为浏览器普遍支持的JavaScript代码，实际上，在React社区中，不使用ES6语法写代码才显得奇怪。

---

### 四、React的工作方式

在继续深入学习React的其他知识之前，我们先就这个简单的ClickCounter组件思考一下React的工作方式，要了解一样东西的特点，最好的方法当然是拿这个东西和另一样东西做比较。我们就拿React和jQuery来比较。

#### 1. jQuery如何工作

假设我们用jQuery来实现ClickCounter的功能，该怎么做呢？首先，我们要产生一个网页的HTML，写一个index.html文件如下所示：

```html
<!doctype html>
<html>
  <body>
    <div>
      <button id="ClickMe">Click Me</button>
      <div>
        Click Count: <span id="clickCount">0</span>
      </div>
    </div>
    <script src="./jquery.min.js"></script>
    <script src="./clickCounter.js"></script>
  </body>
</html>
```

实际产品中，产生这样的HTML可以用PHP、Java、Ruby或者任何一种服务器端语言和框架来做，也可以在浏览器中用Mustache、Hogan这样的模板来产生，这里我们只是把问题简化，直接书写HTML。

上面的HTML只是展示样式，并没有任何交互功能，现在我们用jQuery来实现交互功能，和jQuery的传统一样，我们把JavaScript写在一个独立的文件clickCounter.js中。

```js
$(function() {
  $('#clickMe').click(function() {
    var clickCounter = $('#clickCount');
    var count = parseInt(clickCounter.text(), 10);
    clickCounter.text(count + 1);
  })
})
```

用浏览器打开上面创造的index.html，可以看到实际效果和我们写的React应用一模一样，但是对比这两段程序可以看出差异。

在jQuery解决方案中，首先根据CSS规则找到id为clickCount的按钮，挂上一个匿名事件处理函数，在事件处理函数中，选中那个需要被修改的DOM元素，读取其中的文本值，加以修改，然后修改这个DOM元素。

选中一些DOM元素，然后对这些元素做一些操作，这是一种最容易理解的开发模式。jQuery的发明人John Resig就是发现了网页应用开发者的这种编程模式，才创造出了jQuery，其一问世就得到普遍认可，因为这种模式直观易懂。但是，对于庞大的项目，这种模式会造成代码结构复杂，难以维护，每个jQuery的使用者都会有这种体会。

#### 2. React的理念

与jQuery不同，用React开发应用是另一种体验，我们回顾一下，用React开发的ClickCounter组件好像没有像jQuery那样做“选中一些DOM元素然后做一些事情”的动作。

打一个比方，React是一个聪明的建筑工人，而jQuery是一个比较傻的建筑工人，开发者你就是一个建筑的设计师，如果是jQuery这个建筑工人为你工作，你不得不事无巨细地告诉jQuery“如何去做”，要告诉他这面墙要拆掉重建，那面墙上要新开一个窗户。反之，如果是React这个建筑工人为你工作，你所要做的就是告诉这个工人“我想要什么样子”，只要把图纸递给React这个工人，他就会替你搞定一切，当然他不会把整个建筑拆掉重建，而是很聪明地把这次的图纸和上次的图纸做一个对比，发现不同之处，然后只去做适当的修改就完成任务了。

显而易见，React的工作方式把开发者从繁琐的操作中解放出来，开发者只需要着重“我想要显示什么”，而不用操心“怎样去做”。

这种新的思维方式，对于一个简单的例子也要编写不少代码，感觉像是用高射炮打蚊子，但是对于一个大型的项目，这种方式编写的代码会更容易管理，因为整个React应用要做的就是渲染，开发者关注的是渲染成什么样子，而不用关心如何实现增量渲染。

React的理念，归结为一个公式，就像下面这样：

```js
UI = render(data)
```

让我们来看看这个公示表达的含义，用户看到的界面（UI），应该是一个函数（在这里叫render）的执行结果，只接受数据（data）作为参数。这个函数是一个纯函数，所谓纯函数，指的是没有任何副作用，输出完全依赖于输入的函数，两次函数调用如果输入相同，得到的结果也绝对相同。如此一来，最终的用户界面，在render函数确定的情况下完全取决于输入数据。

对于开发者来说，重要的是区分开哪些属于data，哪些属于render，想要更新用户界面，要做的就是更新data，用户界面自然会做出响应，所以React实践的也是“响应式编程”（Reactive Programming）的思想，这也就是React为什么叫做React的原因。

#### 3. Virtual DOM

既然React应用就是通过重复渲染实现用户交互，我们可能会有一个疑虑：这样的重复渲染会不会效率太低了呢？毕竟，在jQuery的实现方式中，我们可以清楚地看到每次只有需要变化的那一个DOM元素被修改了；可是，在React的实现方式中，看起来每次render函数被调用，都要把整个组件重新绘制一次，这样看起来有点浪费。

事实并不是这样，React利用Virtual DOM，让每次渲染都重新渲染最少的DOM元素。

要了解Virtual DOM，就要先了解DOM，DOM是结构化文本的抽象表达形式，特定于Web环境中，这个结构化文本就是HTML文本，HTML中的每个元素都对应DOM中的某个节点，这样，因为HTML元素的逐级包含关系，DOM节点自然就构成了一个树形结构，称为DOM树。

浏览器为了渲染HTML格式的网页，会先将HTML文本解析以构建DOM树，然后根据DOM树渲染出用户看到的界面，当要改变界面内容的时候，就去改变DOM树上的节点。

Web前端开发关于性能优化有一个原则：尽量减少DOM操作。虽然DOM操作也只是一些简单的JavaScript语句，但是DOM操作会引起浏览器对网页进行重新布局，重新绘制，这就是一个比JavaScript语句执行慢很多的过程。

如果使用mustache或者hogan这样的模板工具，那就是生成HTML字符串塞到网页中，浏览器又要做一次解析产生新的DOM节点，然后替换DOM树上对应的子树部分，这个过程肯定效率不高。虽然JSX看起来很像是一个模板，但是最终会被Babel解析为一条条创建React组件或者HTML元素的语句，神奇之处在于，React并不是通过这些语句直接构建DOM树，而是首先构建Virtual DOM。

既然DOM树是对HTML的抽象，那Virtual DOM就是对DOM树的抽象。Virtual DOM不会触及浏览器的部分，只是存在于JavaScript空间的树形结构，每次自上而下渲染React组件时，会对比这一次产生的Virtual DOM和上一次渲染的Virtual DOM，对比就会发现差别，然后修改真正的DOM树时就只需要触及差别中的部分就行。

以ClickCounter为例，一开始点击计数为0，用户点击按钮让点击计数变成1，这一次重新渲染，React通过Virtual DOM的对比发现其实只是id为clickCounter的span元素中内容从0变成了1而已：

```html
<span id="clickCounter">{this.state.count}</span>
```

React发现这次渲染要做的事情只是更换span元素的内容而已，其他DOM元素都不需要触及，于是执行类似下面的语句，就完成了任务：

```js
document.getElementById('clickCounter').innerHTML = "1";
```

#### 4. React工作方式的优点

毫无疑问，jQuery的方式直观易懂，对于初学者十分适用，但是当项目逐渐变得庞大时，用jQuery写出的代码往往互相纠缠，形成类似下图的状况，难以维护。

![jQuery方式造成的纠缠代码结构](/images/react-1/5.png)

使用React的方式，就可以避免构建这样复杂的程序结构，无论何种事件，引发的都是React组件的重新渲染，至于如何只修改必要的DOM部分，则完全交给React去操作，开发者并不需要关心，程序的流程简化为如下方式。

![React的程序流程](/images/react-1/6.png)

React利用函数式编程的思维来解决用户界面渲染的问题，最大的优势是开发者的效率会大大提高，开发出来的代码可维护性和可阅读性也大大增强。

React等于强制所有组件都按照这种由数据驱动渲染的模式来工作，无论应用的规模多大，都能让程序处于可控范围内。

---

### 五、本文小结

在本文中，我们用create-react-app创造了一个简单的React应用，在一开始，我们就按照组件的思想来开发应用，React的主要理念之一就是基于组件来开发应用。

通过和同样功能的jQuery实现方式对比，我们了解了React的工作方式，React利用声明式的语法，让开发者专注于描述用户界面“显示成什么样子”，而不是重复思考“如何去显示”，这样可以大大提高开发效率，也让代码更加容易管理。

虽然React是通过重复渲染来实现动态更新效果，但是借助Virtual DOM技术，实际上这个过程并不牵涉太多的DOM操作，所以渲染效率很高。

---

### 参考文献

1. [《深入浅出React和Redux —— 程墨》](/resources/深入浅出React和Redux.pdf)