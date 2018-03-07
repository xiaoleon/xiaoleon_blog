---
title: React(3) 从Flux到Redux
categories:
  - React抄书笔记
tags:
  - JavaScript
  - React
date: 2018-03-06 23:05:43
---

在前一章中我们已经感受到完全用React来管理应用数据的麻烦，在这一章中，我们将介绍Redux这种管理应用状态的框架

<!-- More -->

本章包含以下内容：

* 单向数据流框架的始祖Flux

* Flux理念的一个更强实现Redux

* 结合React和Redux

### 一、Flux

要了解Redux，首先要从Flux说起，可以认为Redux是Flux思想的另一种实现方式，通过了解Flux，我们可以知道Flux一族框架（其中就包括Redux）贯彻的最重要的观点——单向数据流，更重要的是，我们可以发现Flux框架的缺点，从而深刻地认识到Redux相对于Flux的改进之处。

让我们来看看Flux的历史，实际上，Flux是和React同时面世的，在2013年，Facebook公司让React亮相的同时，也推出了Flux框架，React和Flux相辅相成，Facebook认为两者结合在一起才能构建大型的JavaScript应用。

做一个容易理解的对比，React是用来替换jQuery的，那么Flux就是以替换Backbone.js、Ember.js等MVC一族框架为目的。

在MVC（Model-View-Controller）的世界里，React相当于V（View）的部分，只涉及页面的渲染，一旦涉及应用的数据管理部分，还是交给Model和Controller，不过，Flux并不是一个MVC框架，事实上，Flux认为MVC框架存在很大问题，它推翻了MVC框架，并用一个新的思维来管理数据流转。

#### 1. MVC框架的缺陷

MVC框架是业界广泛接受的一种前端应用框架类型，这种框架把应用分成三个部分：

* Model（模型）：负责管理数据，大部分业务逻辑也应该放在Model中

* View（视图）：负责渲染用户界面，应该避免在View中涉及业务逻辑

* Controller（控制器）：负责接受用户输入，根据用户输入调用对应的Model部分逻辑，把产生的数据结果交给View部分，让View渲染出必要的输出。

MVC框架的几个组成部分和请求的关系如图所示

![MVC框架](/images/react-3/1.png)

这样的逻辑划分，实质上与把以一个应用划分为多个组件一样，就是“分而治之”。毫无疑问，相比把业务逻辑和界面渲染逻辑混在一起，MVC框架要先进得多。这种方式得到了广泛的认可，连Facebook最初也是用这种框架。

但是，Facebook的工程部门逐渐发现，对于非常巨大的代码库和庞大的组织，按照他们的原话说就是“MVC真的很快就变得非常复杂”。每当工程师想要增加一个新的功能时，对代码的修改很容易引入新的bug，因为不同模块之间的依赖关系让系统变得“脆弱而且不可预测”。对于刚刚加入团队的新手，更是举步维艰，因为不知道修改代码会造成什么样的后果。如果要保险，就会发现寸步难移；如果放手去干，就可能引发很多bug。

一句话，MVC根本不适合Facebook的需求。

为何被业界普遍认可的MVC框架在Facebook眼里却沦落到如此地步呢？

下图是Facebook描述的MVC框架，在图中我们可以看到，Model和View之间缠绕着蜘蛛网一样复杂的依赖关系，根据箭头的方向，我们知道有的是Model调用了View，有的是View调用了Model，很乱。

![MVC的缺点](/images/react-3/2.png)

MVC框架提出的数据流很理想，用户请求先到达Controller，由Controller调用Model获得数据，然后把数据交给View，但是，在实际框架实现中，总是允许View和Model可以直接通信，从而出现上图的情况。越来越多的同行发现，在MVC中让View和Model直接对话就是灾难。

当我向以前没接触过Flux的朋友介绍Flux的时候，发现了一个有意思的现象。凡是只在服务器端使用过MVC框架的朋友，就很容易理解和接受Flux。而对于已经有很多浏览器端MVC框架经验的朋友，往往还要费一点劲才能明白MVC和Flux的差异。

造成这种认知差别的主要原因，就是服务器端MVC框架往往就是每个请求就只在Controller-Model-View三者之间走一圈，结果就返回给浏览器去渲染或者其他处理了，然后这个请求生命周期的Controller-Model-View就可以回收销毁了，这是一个严格意义的单向数据流；对于浏览器端MVC框架，存在用户的交互处理，界面渲染出来之后，Model和View依然处在于浏览器中，这时候就会诱惑开发者为了简便，让现存的Model和View直接对话。

对于MVC框架，为了让数据流可控，Controller应该是中心，当View要传递消息给Model时，应该调用Controller的方法，同样，当Model要更新View时，也应该通过Controller引发新的渲染。

当Facebook推出Flux时，招致了很多质疑。很多人都说，Flux只不过是一个对数据流管理更加严格的MVC框架而已。这种说法不完全准确，但是一定意义上也说明了Flux的一个特点：更严格的数据流控制。

Facebook无心在MVC框架上纠缠，他们用Flux框架来代替原有的MVC框架，他们提出的Flux框架大致结构如图所示

![Flux的单向数据流](/images/react-3/3.png)

一个Flux应用包含四个部分，我们先粗略了解一下：

* Dispatcher：处理动作分发，维持Store之间的依赖关系

* Store：负责存储数据和处理数据相关逻辑

* Action：驱动Dispatcher的JavaScript对象

* View：视图部分，负责显示用户界面

如果非要把Flux和MVC做一个结构对比，那么，Flux的Dispatcher相当于MVC的Controller，Flux的Store相当于MVC的Model，Flux的View当然就对应MVC的View，至于多出来的这个Action，可以理解为对应MVC框架的用户请求。

在MVC框架中，系统能够提供什么样的服务，通过Controller暴露函数来实现。每增加一个功能，Controller往往就要增加一个函数；在Flux的世界里，新增加功能并不需要Dispatcher增加新的函数，实际上，Dispatcher自始至终只需要暴露一个函数Dispatch，当需要增加新的功能时，要做的是增加一种新的Action类型，Dispatcher的对外接口并不用改变。

当需要扩充应用所能处理的“请求”时，MVC方法就需要增加新的Controller，而对于Flux则只是增加新的Action。

下面我们看看怎么用Flux改进我们的React应用。

**** 2. Flux应用

让我们改进一下前面创造的ControlPanel应用，Flux提供了一些辅助工具类和函数，能够帮助创建Flux应用，但是需要一些学习曲线。在这里，我们只用Facebook官方的基本功能，目的是为了更清晰地看一看Flux的工作原理。

首先通过命令行在项目目录下安装Flux。

```cmd
npm install --save flux
```

利用Flux实现ControlPanel应用后的界面效果与前面创造的应用完全一样，通过同一界面不同实现方式的比对，我们可以体会每个方式的优劣。

* 1) Dispatcher

首先，我们要创造一个Dispatcher，几乎所有应用都只需要拥有一个Dispatcher，对于我们这个简单的应用更不例外。在src/AppDispatcher.js中，我们创造这个唯一的Dispatcher对象，代码如下：

```js
import {Dispatcher} from 'flux';

export default new Dispatcher();
```

非常简单，我们引入flux库中的Dispatcher类，然后创造一个新的对象作为这个文件的默认输出就足够了。在其他代码中，将会引用这个全局唯一的Dispatcher对象。

Dispatcher存在的作用，就是用来派发action，接下来我们就来定义应用中涉及的action。

* 2) action

action顾名思义代表一个“动作”，不过这个动作只是一个普通的JavaScript对象，代表一个动作的纯数据，类似于DOM API中的事件（event）。甚至，和事件相比，action其实还是更加纯粹的数据对象，因为事件往往还包含一些方法，比如点击事件就有preventDefault方法，但是action对象不自带方法，就是纯粹的数据。

作为管理，action对象必须有一个名为type的字段，代表这个action对象的类型，为了记录日志和debug方便，这个type应该是字符串类型。

定义action通常需要两个文件，一个定义action的类型，一个定义action的构造函数（也称为action creator）。分成两个文件的主要原因是Store中会根据action类型做不同操作，也就有单独导入action类型的需要。

在src/ActionTypes.js中，我们定义action的类型，代码如下：

```js
export const INCREMENT = 'increment';
export const DECREMENT = 'decrement';
```

在这个例子中，用户只能做两个动作，一个是点击“`+`”按钮，一个是点击“`-`”按钮，所以我们只有两个action类型INCREMENT和DECREMENT。

现在我们在src/Actions.js文件中定义action构造函数：

```js
import * as ActionTypes from './ActionTypes.js';
import AppDispatcher from './AppDispatcher.js';

export const increment = (counterCaption) => {
  AppDispatcher.dispatch({
    type: ActionTypes.INCREMENT,
    counterCaption: counterCaption
  });
};

export const decrement = (counterCaption) => {
  AppDispatcher.dispatch({
    type: ActionTypes.DECREMENT,
    counterCaption: counterCaption
  });
};
```

虽然出于业界习惯，这个文件被命名为Actions.js，但是要注意里面定义的并不是action对象本身，而是能够产生并派发action对象的函数。

在Actions.js文件中，引入了ActionTypes和AppDispatcher，看得出来是要直接使用Dispatcher。

这个Actions.js导出了两个action构造函数increment和decrement，当这两个函数被调用的时候，创造了对应的action对象，并立即通过AppDispatcher.dispatch函数派发出去。

派发出去的action对象最后怎么样了呢？下面关于Store的部分可以看到。

* 3) Store

一个Store也是一个对象，这个对象存储应用状态，同时还要接受Dispatcher派发的动作，根据动作来决定是否要更新应用状态。

接下来我们创造Store相关的代码，因为使用Flux之后代码文件数量会增多，再把所有源代码文件都放在src目录下就不容易管理了。所以我们在src下创建一个子目录stores，在这个子目录里放置所有的Store代码。

在前面章节的ControlPanel应用例子里，有三个Counter组件，还有一个统计三个Counter计数值之和的功能，我们遇到的麻烦就是这两者之间的状态如何同步的问题，现在，我们创造两个Store，一个是为Counter组件服务的CounterStore，另一个就是为总数服务的SummaryStore。

我们首先添加CounterStore，放在src/stores/CounterStore.js文件中。

```js
const counterValues = {
  'First': 0,
  'Second': 10,
  'Third': 30
};

const CounterStore = Object.assign({}, EventEmitter.prototype, {
  getCounterValues: function() {
    return counterValues;
  },

  emitChange: function() {
    this.emit(CHANGE_EVENT);
  },

  addChangeListener: function(callback) {
    this.on(CHANGE_EVENT, callback);
  },

  removeChangeListener: function(callback) {
    this.removeListener(CHANGE_EVENT, callback);
  }
});
```

当Store的状态发生变化的时候，需要通知应用的其它部分做必要的响应。在我们的应用中，做出响应的部分当然就是View部分，但是我们不应该硬编码这种联系，应该用消息的方式建立Store和View的联系。这就是为什么我们让CounterStore扩展了EventEmitter.prototype，等于让CounterStore成了EventEmitter对象，一个EventEmitter实例对象支持下列相关函数。

* emit函数，可以广播一个特定事件，第一个参数是字符串类型的事件名称

* on函数，可以增加一个挂在这个EventEmitter对象特定事件上的处理函数，第一个参数是字符串类型的事件名称，第二个参数是处理函数

* removeListener函数，和on函数做的事情相反，删除挂在这个EventEmitter对象特定事件上的处理函数，和on函数一样，第一个参数是事件名称，第二个参数是处理函数。要注意，如果要调用removeListener函数，就一定要保留对处理函数的引用

对于CounterStore对象，emitChange、addChangeListener和removeChangeListener函数就是利用EventEmitter上述的三个函数完成对CounterStore状态更新的广播、添加监听函数和删除监听函数等操作。

CounterStore函数还提供一个getCounterValues函数，用于让应用中其他模块可以读取当前的计数值，当前的计数值存储在文件模块级的变量counterValues中。

上面实现的Store只有注册到Dispatcher实例上才能真正发挥作用，所以还需要增加下列代码

```js
import AppDispatcher from '../AppDispatcher.js';

CounterStore.dispatchToken = AppDispatcher.register((action) => {
  if (action.type === ActionTypes.INCREMENT) {
    counterValues[action.counterCaption]++;
    CounterStore.emitChange();
  } else if (action.type === ActionTypes.DECREMENT) {
    counterValues[action.counterCaption]--;
    CounterStore.emitChange();
  }
})
```

这是最重要的一个步骤，要把CounterStore注册到全局唯一的Dispatcher上去。Dispatcher有一个函数叫做register，接受一个回调函数作为参数。返回值是一个token，这个token可以用于Store之间的同步，我们在CounterStore中还用不上这个返回值，在稍后的SummaryStore中会用到，现在我们只是把register函数的返回值保存在CounterStore对象的dispatchToken字段上，待会就会用得到。

现在我们来仔细看看register接受的这个回调函数参数，这是Flux流程中最核心的部分，当通过register函数把一个回调函数注册到Dispatcher之后，所有派发给Dispatcher的action对象，都会传递到这个回调函数中来。

比如通过Dispatcher派发一个动作，代码如下：

```js
AppDispatcher.dispatch({
  type: ActionTypes.INCREMENT,
  counterCaption: 'First'
});
```

那在CounterStore注册的回调函数就会被调用，唯一的一个参数就是那个action对象，回调函数要做的，就是根据action对象来决定如何更新自己的状态。

作为一个普遍接受的传统，action对象中必须要有一个type字段，类型是字符串，用于表示这个action对象是什么类型，比如上面派发的action对象，type为“increment”，表示是一个计数器“加一”的动作；如果有必要，一个action对象还可以包含其他的字段。上面的action对象中还有一个counterCaption字段值为“First”，标识名字为“First”的计数器。

在我们的例子中，action对象的type和counterCaption字段结合在一起，可以确定是哪个计数器应该做加一或者减一的动作，上面例子中的动作含义就是：“名字为First的计数器要做加一动作。”

根据不同的type，会有不同的操作，所以注册的回调函数很自然有一个模式，就是函数体是一串if-else条件语句或者switch条件语句，而条件语句的跳转条件，都是针对参数action对象的type字段:

```js
CounterStore.dispatchToken = AppDispatcher.register((action) => {
  if (action.type === ActionTypes.INCREMENT) {
    ...
  } else if (action.type === ActionTypes.DECREMENT) {
    ...
  }
});
```

无论是加一还是减一，最后都要调用counterStore.emitChange函数，假如有调用者通过Counter.addChangeListener关注了CounterStore的状态变化，这个emitChange函数调用就会引发监听函数的执行。

目前，CounterStore只关注INCREMENT和DECREMENT动作，所以if-else判断也只关注了这两种类型的动作，除此之外，其他action对象一律忽略。

接下来，我们再来看看另一个Store，也就是代表所有计数器计数值综合的Store，在src/stores/SummaryStore.js中。

SummaryStore也有emitChange、addChangeListener还有removeChangeListener函数，功能一样也是用于通知监听者状态变化，这几个函数的代码和CounterStore中完全重复，不同点是对获取状态函数的定义，代码如下：

```js
function computeSummary(counterValues) {
  let summary = 0;
  for (const key in counterValues) {
    if (counterValues.hasOwnProperty(key)) {
      summary += counterValues[key];
    }
  }
  return summary;
}

const SummaryStore = Object.assign({}, EventEmitter.prototype, {
  getSummary: function() {
    return computeSummary(CounterStore.getCounterValues());
  }
})
```

可以注意到，SummaryStore并没有存储自己的状态，当getSummary被调用时，它是直接从CounterStore里获取状态计算的。

CounterStore提供了getCounterValues函数让其他模块能够获得所有计数器的值，SummaryStore也提供了getSummary让其他模块可以获得所有计数器当前的总和。不过，既然总可以通过CounterStore.getCounterValues函数获取最新鲜的数据，SummaryStore似乎也就没有必要把计数器总和存储到某个变量里。事实上，可以看到SummaryStore并不像CounterStore一样用一个变量counterValues存储数据，SummaryStore不存储数据，而是每次对getSummary的调用，都实时读取CounterStore.getCounterValues，然后实时计算总和返回给调用者。

可见，虽然名为Store，但并不表示一个Store必须要存储什么东西，Store只是提供获取数据的方法，而Store提供的数据完全可以另一个Store计算得来。

SummaryStore在Dispatcher上注册的回调函数也和CounterStore很不一样，代码如下：

```js
SummaryStore.dispatchToken = AppDispatcher.register((action) => {
  if ((action.type === ActionTypes.INCREMENT) ||
      (action.type === ActionTypes.DECREMENT)) {
        AppDispatcher.waitFor([CounterStore.dispatchToken]);
        SummaryStore.emitChange();
      }
});
```

SummaryStore同样也通过AppDispatcher.register函数注册了一个回调函数，用于接受派发的action对象。在回调函数中，也只关注了INCREMENT和DECREMENT类型的action对象，并通过emitChange通知监听者，注意在这里使用了waitFor函数，这个函数解决的是下面描述的问题。

既然一个action对象会被派发给所有回调函数，这就产生了一个问题，到底是按照什么顺序调用各个回调函数呢？

即使Flux按照register调用的顺序去调用各个回调函数，我们也完全无法把握各个Store哪个先装载从而调用register函数。所以，可以认为Dispatcher调用回调函数的顺序完全是无法预期的，不要假设它会按照我们期望的顺序逐个调用。

怎么解决这个问题呢？这就要靠Dispatcher的waitFor函数了。在SummaryStore的回调函数中，之前在CounterStore中注册回调函数时保存下来的dispatchToken终于派上用场了。

Dispatcher的waitFor可以接受一个数组作为参数，数组中的每个元素都是一个Dispatcher.register函数的返回结果，也就是所谓的dispatchToken。这个waitFor函数告诉Dispatcher，当前的处理必须要暂停，直到dispatchToken代表的那些已注册回调函数执行结束才能继续。

我们知道，JavaScript是单线程语言，不可能有线程之间的等待这回事，这个waitFor函数当然不是用多线程实现的，只是在调用waitFor的时候，把控制权交给Dispatcher，让Dispatcher检查一下dispatchToken代表的回调函数有没有被执行，如果已经执行，那就直接继续，如果还没有执行，那就调用dispatchToken代表的回调函数之后waitFor才返回。

回到我们上面的例子，即使SummaryStore比CounterStore提前接收到了action对象，在emitChange中调用waitFor，也就能够保证emitChange函数被调用的时候，CounterStore也已经处理过这个action对象。

这里要注意一个事实，Dispatcher的register函数，只提供了一个回调函数的功能，但却不能让调用者在register时选择只监听某些action，换句话说，每个register的调用者只能这样请求：“当有任何动作被派发时，请调用我。”但不能够这么请求：“当这种类型还有那种类型的动作被派发的时候，请调用我。”

当一个动作被派发的时候，Dispatcher就是简单地把所有注册的回调函数全都调用一遍，至于这个动作是不是对方关心的，Flux的Dispatcher不关心，要求每个回调函数去鉴别。

看起来，这似乎是一种浪费，但是这个设计让Flux的Dispatcher逻辑最简单化，Dispatcher的责任越简单，就越不会出现问题。毕竟，由回调函数全权决定如何处理action对象，也是非常合理的。

* 4) View

---

### 二、

<!-- Todo -->


---

### 三、

<!-- Todo -->


---

### 四、

<!-- Todo -->

### 参考文献

1. [《深入浅出React和Redux —— 程墨》]()