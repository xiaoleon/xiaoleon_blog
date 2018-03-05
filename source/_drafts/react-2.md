---
title: React(2) 设计高质量的React组件
categories:
  - React抄书笔记
tags:
  - JavaScript
  - React
date: 2018-03-04 21:29:25
---

作为一个合格的开发者，不能只满足于编写出可以运行的代码，而要了解代码背后的工作原理；不能只满足于自己编写的程序能够运行，还要让自己的代码可读而且易于维护。这样才能开发出高质量的软件。

<!-- More -->

本文中，我们将深入介绍构建高质量React组件的原则和方法，包括以下内容

* 划分组件边界的原则

* React组件的数据种类

* React组件的生命周期

### 一、易于维护组件的设计要素

任何一个复杂的应用，都是由一个简单的应用发展而来的，当应用还很简单的时候，因为功能很少，可能只有一个组件就足够了，但是，随着功能的增加，把越来越多的功能放在一个组件里就会显得臃肿和难以管理。

就和一个人最好一次只专注做一件事一样，也应该尽量保持一个组件只做一件事。当开发者发现一个组件功能太多代码量太大的时候，就要考虑拆分这个组件，用多个小的组件来代替。每个小的组件只关注实现单个功能，但是这些功能组合起来，也能满足复杂的实际需求。

这就是“分而治之”的策略，把问题分解为多个小问题，这样极容易解决也方便维护，虽然“分而治之”是一个好策略，但是不要滥用，只有必要的时候才去拆分组件，不然可能得不偿失。

拆分组件最关键的就是确定组件的边界，每个组件都应该是可以独立存在的，如果两个组件逻辑太紧密，无法清晰定义各自的责任，那也许这两个组件本身就不该被拆开，作为同一个组件也许更合理。

虽然组件是应该独立存在的，但是并不是说组件就是孤岛一样的存在，不同组件之间总会有通信交流，这样才可能组合起来完成更大的功能。

作为软件设计的通则，组件的划分要满足**高内聚（High Cohesion）**和**低耦合（Low Coupling）**的原则。

**高内聚**指的是把逻辑紧密相关的内容放在一个组件中。用户界面无外乎内容、交互行为和样式。传统上，内容由HTML表示，交互行为放在JavaScript代码文件中，样式放在CSS文件中定义。这虽然满足一个功能模块的需要，却要放在三个不同的文件中，这其实不满足高内聚的原则，React却不是这样，展示内容的JSX、定义行为的JavaScript，甚至定义样式的CSS，都可以放在一个JavaScript文件中，因为它们本来就是为了实现一个目的而存在的，所以说React天生具有高内聚的特点。

**低耦合**指的是不同组件之间的依赖关系要尽量弱化，也就是每个组件要尽量独立。保持整个系统的低耦合度，需要对系统中的功能有充分的认识，然后根据功能点划分模块，让不同的组件去实现不同的功能，这个功夫还在开发者身上，不过，React组件的对外结构非常规范，方便开发者设计低耦合的系统。

---

### 二、React组件的数据

毫无疑问，如何组织数据是程序的最重要问题。

React组件的数据分为两种，prop和state，无论prop或者state的改变，都可能引发组件的重新渲染，那么，设计一个组件的时候，什么时候选择用prop什么时候选择用state呢？其实原则很简单，prop是组件的对外接口，state是组件的内部状态，对外用prop，内部用state。

为了演示属性的使用，我们构造一个应用包含两个组件，Counter组件和ControlPanel组件，其中ControlPanel组件是父组件，包含若干个Counter组件。

![ControlPanel应用效果图](/images/react-2/1.png)

可以看到三个Counter组件有了不同的初始计数值，点击网页中的“`+`”按钮可以看到对应一行的计数增加，点击“`-`”按钮可以看到对应一行的计数减少。

#### 1. React的prop

在React中，prop（property的简写）是从外部传递给组件的数据，一个React组件通过定义自己能够接受的prop就定义了自己的对外公共接口。

每个React组件都是独立存在的模块，组件之外的一切都是外部世界，外部世界就是通过prop来和组件对话的。

* 1) 给prop赋值

我们先从外部世界来看，prop是如何使用的，在下面的JSX代码片段中，就使用了prop

```jsx
<SampleButton id="sample" borderWidgh={2} onClick={onButtonClick} style={{color: "red"}} />
```

在上面的例子中，创建了名为SampleButton的组件实例，使用了名字分别为id、borderWidth、onClick和style的prop，看起来，React组件的prop很像是HTML元素的属性，不过，HTML组件属性的值都是字符串类型，即使是内嵌JavaScript，也依然是字符串形式表示代码。React组件的prop所能支持的类型则丰富得多，除了字符串，可以是任何一种JavaScript语言支持的数据类型。

比如在上面的SampleButton中，borderWidth就是数字类型，onClick是函数类型，style的值是一个包含color字段的对象，当prop的类型不是字符串类型时，在JSX中必须用花括号`{}`把prop值包住，所以style的值有两层花括号，外层花括号代表的是JSX的语法，内层的花括号代表这是一个对象常量。

当外部世界要传递一些数据给React组件，一个最直接的方式就是通过prop；同样，React组件要反馈数据给外部世界，也可以用prop，因为prop的类型不限于纯数据，也可以是函数，函数类型的prop等于让父组件交给了子组件一个回调函数，子组件在恰当的时机调用函数类型的prop，可以带上必要的参数，这样就可以反过来把信息传递给外部世界。

对于Counter组件，父组件的ControlPanel就是外部世界，我们看ControlPanel是如何用prop传递信息给Counter的，代码如下

```js
class ControlPanel extends Component {
  render() {
    return (
      <div>
        <Counter caption="First" initValue={0} />
        <Counter caption="Second" initValue={10} />
        <Counter caption="Third" initValue={20} />
      </div>
    )
  }
}
```

ControlPanel组件包含三个Counter组件实例，在ControlPanel的render函数中将这三个子组件实例用div包起来，因为React要求render函数只能返回一个元素。

在每个Counter组件实例中，都使用了caption和initValue这两个prop。通过名为caption的prop，ControlPanel传递给Counter组件实例说明文字。通过initValue的prop传递给Counter组件一个初始的计数值。

* 2) 读取prop值

我们再来看Counter组件内部是如何接收传入的prop的，首先是构造函数，代码如下

```js
class Counter extends Component {
  constructor(props) {
    super(props);

    this.onClickIncrementButton = this.onClickIncrementButton.bind(this);
    this.onClickDecrementButton = this.onClickDecrementButton.bind(this);

    this.state = {
      count: props.initValue || 0;
    }
  }
}
```

如果一个组件需要定义自己的构造函数，一定要记得在构造函数的第一行通过super调用父类也就是React.Component的构造函数。如果在构造函数中没有调用super(props)，那么组件实例被构造之后，类实例的所有成员函数就无法通过this.props访问到父组件传递过来的props值。很明显，给this.props赋值是React.Component构造函数的工作之一。

在Counter的构造函数中还给两个成员函数绑定了当前this的执行环境，因为ES6方法创造的React组建类并不自动给我们绑定this到当前实例对象。

在构造函数的最后，我们可以看到读取传入prop的方法，在构造函数中可以通过参数props获得传入的prop值，在其他函数中则可以通过this.props访问传入的prop的值，比如在Counter组件的render函数中，我们就是通过this.props获得传入的caption，render代码如下

```js
render() {
  const { caption } = this.props;

  return (
    <div>
      <button style={buttonStyle} onClick={this.onClickIncrementButton}>+</button>
      <button style={buttonStyle} onClick={this.onClickDecrementButton}>-</button>
      <span>{caption} count: {this.state.count}</span>
    </div>
  );
}
```

在上面的代码中，我们使用了ES6的解构赋值（destructuring assignment）语法从this.props中获得了名为caption的prop值。

* 3) propTypes检查

既然prop是组件的对外接口，那么就应该有某种方式让组件声明自己的接口规范。简单说，一个组件应该可以规范以下这些方面：

  * 这个组件支持哪些prop

  * 每个prop应该是什么样的格式

React通过propTypes来支持这些功能。

在ES6方法定义的组件类中，可以通过增加类的propTypes属性来定义prop规格，这不只是声明，而且是一种限制，在运行时和静态代码检查时，都可以根据propTypes判断外部世界是否正确地使用了组件的属性。

比如，对于Counter组件的propTypes定义代码如下：

```js
Counter.propTypes = {
  caption: PropTypes.string.isRequired,
  initValue: PropTypes.number
};
```

其中要求caption必须是string类型，initValue必须是number类型。可以看到，两者除了类型不同之外，还有一个区别：caption带上了isRequired，这表示使用Counter组件必须要指定caption；而initValue因为没有isRequired，则表示如果没有也没关系。

为了验证propTypes的作用，可以尝试故意违反propTypes的规定使用Counter实例，比如在ControlPanel的render函数中增加下列的代码：

```jsx
<Counter caption={123} initValue={20} />
```

我们在Chrome浏览器中，可以看到console中的红色警告

![错误prop类型的错误提示](/images/react-2/2.png)

这段出错的含义是，caption属性预期是字符串类型，得到的却是一个数字类型。我们尝试删掉这个Counter实例的caption属性，代码如下：

```jsx
<Counter initValue={20} />
```

这是可以看到Console中依然有红色警告信息

![缺失必须存在prop的错误提示](/images/react-2/3.png)

提示的含义是，caption是Counter必需的属性，但是却没有赋值。

很明显，有了propTypes的检查，可以很容易发现对prop的不正确使用方法，可尽早发现代码中的错误。

如果组件根本没有定义propTypes会怎么样呢？可以尝试在`src/Counter.js`文件中删除掉propTypes赋值的语句，在浏览器Console中红色警告不再出现。可见，没有propTypes定义，组件依然能够正常工作，而且，即使在上面propTypes检查出错的情况下，组件依旧能够工作。也就是说propTypes检查只是一个辅助开发的功能，并不会改变组件的行为。

propTypes虽然能够在开发阶段发现代码中的问题，但是放在产品环境中就不大合适了。

首先，定义类的propTypes属性，无疑是要占用一些代码空间，而且propTypes检查也是要消耗CPU计算资源的。其次，在产品环境下做propTypes检查没有什么帮助，毕竟，propTypes产生的这些错误信息只有开发者才能看得懂，放在产品环境下，在最终用户的浏览器Console中输出这些错误信息没什么意义。

所以，最好的方式是，开发者在代码中定义propTypes，在开发过程中避免犯错，但是在发布产品代码时，用一种自动的方式将propTypes去掉，这样最终部署到产品环境的代码就会更优。现有的babel-react-optimize就具有这个功能，可以通过npm安装，但是应该确保只在发布产品时使用它。

#### 2. React的state

驱动组件渲染过程的除了prop，还有state，state代表组件的内部状态。由于React组件不能修改传入的prop，所以需要记录自身数据变化，就要使用state。

在Counter组件中，最初显示初始计数，可以通过initValue这个prop来定制，在Counter已经被显示之后，用户会点击“`+`”和“`-`”按钮改变这个计数，这个变化的数据就要Counter组件自己通过state来存储了。

* 1) 初始化state

通常在组件类的构造函数结尾处初始化state，在Counter构造函数中，通过对this.state的赋值完成了对组件state的初始化，代码如下：

```js
constructor(props) {
  ...
  this.state = {
    count: props.initValue || 0
  }
}
```

因为initValue是一个可选的props，要考虑到父组件没有指定这个props值的情况，我们优先使用传入属性的initValue，如果没有，就是用默认值0。

组件的state必须是一个JavaScript对象，不能是string或者number这样的简单数据类型，即使我们需要存储的只是一个数字类型的数据，也只能把它存作state某个字段对应的值，Counter组件里，我们的唯一数据就存在count字段里。

由于在PropType声明中没有用isRequired要求必须有值的prop，例如上面的initValue，我们需要在代码中判断所给的prop值是否存在，如果不存在，就给一个默认的初始值。不过，让这样的判断逻辑充斥在我们组件的构造函数之中并不是一件美观的事情，而且容易有遗漏。我们可以用React的defaultProps功能，让代码更加容易读懂。

给Counter组件添加defaultProps代码如下：

```js
Counter.defaultProps = {
  initValue: 0
};
```

有了这样的设定，Counter构造函数中的this.state初始化中可以省去判断条件，可以认为代码执行到这里，必有initValue属性值，代码可以简化为这样

```js
this.state = {
  count: props.initValue
}
```

以后，即使Counter的使用者没有指定initValue，在组件中就会收到一个默认的属性值0。

* 2) 读取和更新state

通过给button的onClick属性挂载点击事件处理函数，我们可以改变组件的state，以点击“`+`”按钮的响应函数为例，代码如下：

```js
onClickIncrementButton() {
  this.setState({
    count: this.state.count + 1
  });
}
```

在代码中，通过this.state可以读取到组件的当前state。值得注意的是，我们改变组件state必须要使用this.setState函数，而不能直接去修改this.state。

直接修改this.state的值，虽然事实上改变了组件的内部状态，但只是野蛮地修改了state，却没有驱动组件进行重新渲染，既然组件没有重新渲染，当然不会反应this.state值的变化；而this.setState()函数所做的事情，首先是改变this.state的值，然后驱动组件经历更新过程，这样才有机会让this.state里新的值出现在界面上。

#### 3. prop和state的对比

总结一下prop和state的区别：

* prop用于定义外部接口，state用于记录内部状态

* prop的赋值在外部世界使用组件时，state的赋值在组件内部

* 组件不应该改变prop的值，而state存在的目的就是让组件来改变的

组件的state，就相当于组件的记忆，其存在意义就是被修改，每一次通过this.setState函数修改state就改变了组件的状态，然后通过渲染过程把这种变化体现出来。

但是，组件是绝不应该去修改传入的props值的，我们设想一下，假如父组件包含多个子组件，然后把一个JavaScript对象作为props值传给这几个子组件，而某个子组件居然改变了这个对象的内部值，那么，接下来其他子组件读取这个对象会得到什么值呢？当时读取了修改过的值，但是其他子组件是每次渲染都读取这个props的值呢？还是只读一次以后就用那个最初值呢？一切皆有可能，完全不可预料。也就是说，一个子组件去修改props中的值，可能让程序陷入一团混乱之中，这就完全违背了React设计的初衷。

严格来说，React并没有办法阻止我们去修改传入的props对象。所以，每个开发者就把这当做一个规矩，在编码中一定不要踩这道红线，不然最后可能遇到不可预料的bug。

---

### 三、组件的生命周期

为了理解React的工作过程，我们就必须要了解React组件的生命周期，如同人有生老病死，自然界有日月更替，每个组件在网页中也会被创建、更新和删除，如同有生命的机体一样。

React严格定义了组件的生命周期，会经理如下三个过程：

* 装载过程（Mount），也就是把组件第一次在DOM树中渲染的过程

* 更新过程（Update），当组件被重新渲染的过程

* 卸载过程（Unmount），组件从DOM中删除的过程

三种不同的过程，React库会依次调用组件的一些成员函数，这些函数称为生命周期函数。所以，要定制一个React组件，实际上就是定制这些生命周期函数。

#### 1. 装载过程

我们先来看装载过程，当组件第一次被渲染的时候，依次调用的函数是如下这些：

* constructor

* getInitialState

* getDefaultProps

* componentWillMount

* render

* componentDidMount

我们逐个详细解释这些函数的功能

* 1) constructor

我们先来看constructor，也就是ES6中每个类的构造函数，要创造一个组件类的实例，当然会调用对应的构造函数。

要注意，并不是每个组件都需要定义自己的构造函数。在后文中我们可以看到，无状态的React组件往往就不需要定义构造函数，一个React组件需要构造函数，往往是为了下面的目的：

  * 初始化state，因为组件生命周期中任何函数都可能要访问state，那么整个生命周期中第一个被调用的构造函数自然是初始化state最理想的地方

  * 绑定成员函数的this环境

在ES6语法下，类的每个成员函数在执行时的this并不是和类实例自动绑定的。而在构造函数中，this就是当前组件实例，所以，为了方便将来的调用，往往在构造函数中将这个实例的特定函数绑定this为当前实例。

以Counter组件为例，我们的构造函数有这样如下的代码：

```js
this.onClickIncrementButton = this.onClickIncrementButton.bind(this);
this.onClickDecrementButton = this.onClickDecrementButton.bind(this);
```

这两句的作用，就是通过bind方法让当前实例中onClickIncrementButton和onClickDecrementButton函数被调用时，this始终是指向当前组件实例。

* 2) getInitialState和getDefaultProps

getInitialState这个函数的返回值用来初始化组件的this.state，但是，这个方法只有用React.createClass方法创造的组件类才会发生作用，本文中使用ES6语法，所以这个函数根本不会产生作用。

getDefaultProps函数的返回值可以作为props的初始值，和getInitialState一样，这个函数不会产生作用。

* 3) render

render函数无疑是React组件中最重要的函数，一个React组件可以忽略其他所有函数都不实现，但是一定要实现render函数，因为所有React组件的父类React.Component类对除render之外的生命周期函数都有默认实现。

通常一个组件要发挥作用，总是要渲染一些东西，render函数并不做实际的渲染动作，它只是返回一个JSX描述的结构，最终由React来操作渲染过程。

当然，某些特殊组件的作用不是渲染界面，或者，组件在某些情况下选择没有东西可画，那就让render函数返回一个null或者false，等于告诉React，这个组件这次不需要渲染任何DOM元素。

需要注意的时，render函数应该是一个纯函数，是完全根据this.state和this.props来决定返回的结果，而且不要产生任何副作用。在render函数中去调用this.setState毫无疑问是错误的，因为一个纯函数不应该引起状态的改变。

* 4) componentWillMount和componentDidMount

在装在过程中，componentWillMount会在调用render函数之前被调用，componentDidMount会在调用render函数之后被调用，这两个函数就像是render函数的前哨和后卫，一前一后，把render函数夹住，正好分别做render前后必要的工作。

不过，我们通常不用定义componentWillMount函数，顾名思义，componentWillMount发生在“将要装载”的时候，这个时候没有任何渲染出来的结果，即使调用this.setState修改状态也不会引发重新绘制，一切都迟了。换句话说，所有可以在这个componentWillMount中做的事情，都可以提前到constructor中去做，可以认为这个函数存在的主要目的就是为了和componentDidMount对称。

而componentDidMount作用就大了，需要注意的是，render函数被调用完之后，componentDidMount函数并不是会被立刻调用，componentDidMount被调用的时候，render函数返回的东西已经引发了渲染，组件已经被装载到了DOM树上。

我们还是以ControlPanel为例，在ControlPanel中有三个Counter组件，我们稍微修改Counter的代码，让装在过程中所有生命周期函数都用console.log输出函数名和caption的值，比如，componentWillMount函数的内容如下：

```js
componentWillMount() {
  console.log('enter componentWillMount ' + this.props.caption);
}
```

在浏览器的console里我们能够看见：

```cmd
enter constructor: First
enter componentWillMount First
enter render First
enter constructor: Second
enter componentWillMount Second
enter render Second
enter constructor: Third
enter componentWillMount Third
enter render Third
enter componentDidMount First
enter componentDidMount Second
enter componentDidMount Third
```

可以清楚的看到，虽然componentWillMount都是紧贴着自己组件的render函数之前被调用，componentDidMount可不是紧跟着render函数被调用，当所有三个组建的render函数都被调用之后，三个组件的componentDidMount才连在一起被调用。

之所以会有上面的现象，是因为render函数本身并不往DOM树上渲染或者装载内容，它只是返回一个JSX表示的对象，然后由React库来根据返回对象决定如何渲染。而React库肯定是要把所有的组件返回的结果综合起来，才能知道如何产生对应的DOM修改。所以，只有React库调用三个Counter组件的render函数之后，才有可能完成装载，这时候才会依次调用各个组件的componentDidMount函数作为装载过程的收尾。

componentWillMount和componentDidMount这对兄弟函数还有一个区别，就是componentWillMount可以在服务器端被调用，也可以在浏览器端被调用；而componentDidMount只能在浏览器端被调用，在服务器端使用React的时候不会被调用。

目前为止，我们构造的React应用例子都只是在浏览器端使用React，所以看不出区别，在后面关于“同构”应用的介绍时，我们会探讨在服务器端使用React的情况。

至于为什么只有componentDidMount仅在浏览器端执行，这是一个实现上的决定，而不是设计时刻有意而为之。不过，如果非要有个解释的话，可以这么说，既然“装载”是一个创建组件并放到DOM树上的过程，那么，真正的“装载”是不可能在服务器端完成的，因为服务器端渲染并不会产生DOM树，通过React组件产生的只是一个纯粹的字符串而已。

不管怎样，componentDidMount只在浏览器端执行，倒是给了我们开发者一个很好地位置去做只有浏览器端才做的逻辑，比如通过Ajax获取数据来填充组件的内容。

在componentDidMount被调用的时候，组件已经被装载到DOM树上了，可以放心获取渲染出来的任何DOM。

在实际开发过程中，可能会需要让React和其他UI库配合使用，比如，因为项目前期已经用jQuery开发了很多功能，需要继续使用这些基于jQuery的代码，有时候其他的UI库做某些功能比React更合适，比如d3.js已经支持了丰富的绘制图表的功能，在这些情况下，我们不得不考虑如何让React和其他UI库和平共处。

以和jQuery配合为例，我们知道，React是用来取代jQuery的，但如果真的要让React和jQuery配合，就需要利用componentDidMount函数，当componentDidMount被执行时，React组件对应的DOM已经存在，所有的事件处理函数也已经设置好，这时候就可以调用jQuery的代码，让jQuery在已经绘制的DOM基础上增强新的功能。

在componentDidMount中调用jQuery代码只处理了装载过程，要和jQuery完全结合，又要考虑React的更新过程，就需要使用下面要讲的componentDidUpdate函数。

#### 2. 更新过程

当组件被装载到DOM树上之后，用户在网页上可以看到组件的第一印象，但是要提供更好的交互体验，就要让该组件可以随着用户操作改变展现的内容，当props或者state修改的时候，就会引发组件的更新过程。

更新过程会依次调用下面的生命周期函数，其中render函数和装载过程一样，没有差别。

* componentWillReceiveProps

* shouldComponentUpdate

* componentWillUpdate

* render

* componentDidUpdate

有意思的是，并不是所有的更新过程都会执行全部函数，下面会介绍到各种特例。

* 1) componentWillReceiveProps(nextProps)

关于这个componentWillReceiveProps存在一些误解。在网上有些教材声称这个函数只有当组件的props发生改变的时候才会被调用，其实是不正确的。实际上，只要是父组件的render函数被调用，在render函数里面被渲染的子组件就会经历更新过程，不管父组件传给子组件的props有没有改变，都会触发子组件的componentWillReceiveProps函数。

注意，通过this.setState方法触发的更新过程不会调用这个函数，这是因为这个函数适合根据新的props值（也就是参数nextProps）来计算出是不是要更新内部状态state。更新组件内部状态的方法就是this.setState，如果this.setState的调用导致componentWillReceiveProps再一次被调用，那就是一个死循环了。

让我们对ControlPanel做一些小的改进，来体会一下上面提到的规则。

我们首先在Counter组件类里增加函数定义，让这个函数componentWillReceiveProps在console上输出一些文字，代码如下：

```js
componentWillReceiveProps(nextProps) {
  console.log('enter componentWillReceiveProps ' + this.props.caption);
}
```

在ControlPanel组件的render函数中，我们也做如下修改

```js
render() {
  console.log('enter ControlPanel render');
  return (
    <div style={style}>
      ...
      <button onClick={ () => this.forceUpdate() }>
        Click me to repaint!
      </button>
    </div>
  )
}
```

除了在ControlPanel的render函数入口处增加console输出，我们还增加了一个按钮，这个按钮的onClick事件引发了一个匿名函数，当这个函数被点击的时候，调用this.forceUpdate，每个React组件都可以通过forceUpdate函数强行引发一次重新绘制。

在网页中，我们去点击那个新增加的按钮，可以看到浏览器的console中有如下输出：

```console
enter ControlPanel render
enter componentWillReceiveProps First
enter render First
enter componentWillReceiveProps Second
enter render Second
enter componentWillReceiveProps Third
enter render Third
```

可以看到，引发forceUpdate之后，首先是ControlPanel的render函数被调用，随后第一个Counter组件的componentWillReceiveProps函数被调用，然后Counter组件的render函数被调用，随后第二个第三个组件的这两个函数也依次被调用。

然而，ControlPanel在渲染三个子组件的时候，提供的props值一直就没有变化，可见componentWillReceiveProps并不是当props值变化的时候才被调用，所以，这个函数有必要把传入参数nextProps和this.props做必要对比。nextProps代表的是这一次渲染传入的props值，this.props代表的上一次渲染时的props值，只有两者有变化的时候才有必要调用this.setState更新内部状态。

在网页中，我们再次尝试点击第一个Counter组件的“`+`”按钮，可以看到浏览器的console输出如下：

```console
enter render First
```

明显，只有第一个组件的Counter的render函数被调用，函数componentWillReceiveProps没有被调用。因为点击“`+`”按钮引发的是第一个Counter组件的this.setState函数的调用，就像上面说过的一样，this.setState不会引发这个函数componentWillReceiveProps被调用。

从这个例子我们也会发现，在React的组件组合中，完全可以只渲染一个子组件，而其它组件完全不需要渲染，这是提高React性能的重要方式。

* 2) shouldComponentUpdate(nextProps, nextState)

---

### 四、

<!-- Todo -->

### 参考文献

1. [《Todo》]()