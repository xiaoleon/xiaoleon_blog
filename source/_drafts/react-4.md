---
title: React(3) 模块化React和Redux应用
categories:
  - React抄书笔记
tags:
  - JavaScript
  - React
date: 2018-03-13 23:02:05
---

在第一部分中，我们已经了解了React的基本工作方式，也知道了Redux在组合React组件中的作用，但是更多的只是了解其基本原理和使用方法，上手练习的也是一个简单的例子。

<!-- More -->

实际工作中我们要创建的应用无论结构和大小都要复杂得多，在这一章中，我们要介绍创建一个复杂一点的应用应该如何做，包含以下内容：

* 模块化应用的要点

* 代码文件的组织方式

* 状态树的设计

* 开发辅助工具

### 一、模块化应用要点

在本书中，我们探讨的是如何用React和Redux来构建前端网页应用，这两者都奉行这样一个公式`UI=render(state)`来产生用户界面。React适合于视图层面的东西，但是不能指望靠React来管理应用的状态，Redux才适合担当应用状态的管理工作。

从架构出发，当我们开始一个新的应用的时候，有几件事情是一定要考虑清楚的：

* 代码文件的组织结构

* 确定模块的边界

* Store的状态树设计

这三件事情，是构建一个应用的基础。如果我们在一开始深入思考这三件事，并做出合乎需要的判断，可以在后面的路上省去很多麻烦。

从本章开始，我们将构造一个“待办事项”（Todo）应用，逐步完善这个应用，增加新的功能，在这个Todo应用的进化过程中来学习各个层次的知识。

在这个各种JavaScript框架层出不穷的时代，Todo应用几乎就代替了传统Hello World应用的作用，每个框架问世的时候都会用一个Todo应用来展示自己的不同，不要小看了这样一个Todo应用，它非常适合用于做技术展示，首先，这个应用的复杂度刚刚好，没有复杂到可能要很多篇幅才能解释清楚做什么，也没有简单到只需要几行代码就能够搞定；其次，这样的功能非常利于理解，恰好能够考验一个JavaScript框架的表达能力。

确定了我们的应用要做什么之后，不要上来就开始写代码，磨刀不误砍柴工，先要思考上面提到的三个问题。让我们从第一个问题开始。

---

### 二、代码文件的组织方式

#### 1. 按角色组织

如果读者之前曾用MVC框架开发过应用程序，应该知道MVC框架之下，通常有这样一种代码的组织方式，文件目录列表如下：

```cmd
controllers/
  todoController.js
  filterController.js
models/
  todoModel.js
  filterModel.js
views/
  todo.js
  todoItem.js
  filter.js
```

在MVC中，应用代码分为Controller、Model和View，分别代表三种模块角色，就是把所有的Controller代码放在controllers目录下，把所有的Model代码方法在models目录下，把View代码放在views目录下。这种组织代码的方式，叫做“按角色组织”（Organized by Roles）。

我们当然不会使用MVC，在上一章中我们介绍过MVC框架的缺点。和众多前端开发者一样，我们选择Flux和Redux就是为了克服这些缺点的，但是因为MVC框架的影响非常深远，一些风格依然影响了前端开发人员的思维方式。

因为MVC这种“按角色组织”代码文件的影响，在Redux应用的构建中，就有这样一种代码组织方法，文件目录列表如下：

```cmd
reducers/
  todoReducer.js
  filterReducer.js
actions/
  todoActions.js
  filterActions.js
components/
  todoList.js
  todoItem.js
  filter.js
containers/
  todoListContainer.js
  todoItemContainer.js
  filterContainer.js
```

和MVC的代码组织方式不同，只不过是把controller、models和views目录换成了reducers、actions、components和containers，各个目录下代码文件的角色如下：

* reducer目录包含所有Redux的reducer

* actions目录包含所有action构造函数

* components目录包含所有的傻瓜组件

* containers目录包含所有的容器组件

这种组织方式看起来还不错，把一个类型的代码文件放在了一个目录下，至少比把所有代码全放在一个目录下要有道理。

实际上，在前面章节的所有ControlPanel例子中，我们采用的也是类似的方法，当我们发现代码文件变多，全都直接放在一个src目录下不合理时，首先想到的就是建一个views目录，把所有视图相关的目录移到views目录里面去。我们没有移动action相关和reducer相关的文件，只因为ControlPanel应用实在太简单，因为只有一个组件Counter可能发出动作，所以只有一个Action文件，也只有一个对应的Reducer文件，所以到最后我们都没有觉得有必要把它们移动到代表各自角色的目录里面去。

有过MVC框架开发经历的朋友可以回忆一下，当你需要对一个功能进行修改，虽然这个功能只是针对某一个具体的应用模块，但是却牵扯到MVC中的三个角色Controller、Model和View，不管你用的是什么样的编辑器，你都得费点劲才能在这三个目录之间跳转，或者需要滚动文件列表跳过无关的分发器文件才能找到你想要修改的那一个分发器文件。

如果说MVC框架下，因为三个角色之间的交叉关系，也只能默默接受，那么在Redux框架下，我们已经有机会实现严格模块化的思想，就应该想一想更好的组织文件的方式。

#### 2. 按功能组织

Redux应用适合于“按功能组织”（Organzied by Feature），也就是完成同一应用功能的代码放在一个目录下，一个应用功能包含多个角色的代码。在Redux中，不同的角色就是reducer、actions和视图，而应用功能对应的就是用户界面上的交互模块。

拿Todo应用为例子，这个应用的两个基本功能就是TodoList和Filter，所以代码就这样组织，文件目录列表如下：

```cmd
todoList/
  actions.js
  actionTypes.js
  index.js
  reducer.js
  views/
    component.js
    container.js
filter/
  actions.js
  actionTypes.js
  index.js
  reducer.js
  views/
    component.js
    container.js
```

每个基本功能对应的其实就是一个模块，每个功能模块对应一个目录，这个例子中分别是todoList和filter，每个目录下包含同样名字的角色文件：

* actionTypes.js 定义action类型

* actions.js 定义action构造函数，决定了这个功能模块可以接受的动作

* reducer.js 定义这个功能模块如何响应actions.js中定义的动作

* views目录，包含这个功能模块中所有的React组件，包括傻瓜组件和容器组件

* index.js 这个文件把所有的角色导入，然后统一导出

在这种组织方式下，当我们要修改某个功能模块的代码的时候，只要关注对应的目录就行了，所有需要修改的代码文件都能在这个目录下找到。

表面上看，“按照角色组织”还是“按照功能组织”只是一个审美的问题，也许你觉得自己已经习惯了MVC世界的“按照角色组织”方式，也许你已经有一套很厉害的代码编辑器可以完美解决在不同目录下寻找代码文件困难的问题。但是，开发Redux应用你依然应该用“按照功能组织”的方式，为什么呢？我们看看下一条“确定模块的边界”就明白了。

---

### 三、模块接口

不同功能模块之间的依赖关系应该简单而且清晰，也就是所谓的保持模块之间低耦合性；一个模块应该把自己的功能封装得很好，让外界不要太依赖与自己内部的结构，这样不会因为内部变化而影响外部模块的功能，这就是所谓高内聚性。

React组件本身应该具有低耦合性和高内聚性的特点，不过在Redux的游乐场中，React组件扮演的就是一个视图的角色，还有reducer、actions这些角色参与这个游戏。对于整个Redux应用而言，整体由模块构成，但是模块不再是React组件，而是由React组件加上关于reducer和actions构成的小整体。

以我们将要实现的Todo应用为例，功能模块就是todoList和filter，这两个功能模块分别用各自的React组件、reducer和action定义。

可以预期每个模块之间会有依赖关系，比如filter模块想要使用todoList的action构造函数和视图，那么我们希望对方如何导入呢？一种写法是像下面的代码这样：

```js
import * as actions from '../todoList/actions';
import container as TodoList from '../todoList/views/container';
```

这种写法当然能够完成功能，但是却非常不合理，因为这让filter模块依赖于todoList模块的内部结构，而且直接伸手到todoList内部去导入想要的部分。

虽然我们在上面的例子中，todoList和filter中的文件名几乎一样，但是这毕竟是模块内部的事情，不应该硬性要求，更不应该假设所有的模块都应该按照这样的文件命名。在我们的例子中，存储视图代码文件的目录叫做views，但是有的开发者习惯把这个目录叫做components；我们把包含容器组件的文件名叫做container.js，根据开发者个人习惯也可能叫做TodoList，这些都没有必要而且也不应该有硬性规定。

现在既然我们把一个目录看作一个模块，那么我们要做的是明确这个模块对外的接口，而这个接口应该实现把内部封装起来。

请注意我们的todoList和filter模块目录下，都有一个index.js文件，这个文件就是我们的模块接口。

比如，在todoList/index.js中，代码如下：

```js
import * as actions from './actions.js';
import reducer from './reducer.js';
import view from './views/container.js';

export {actions, reducer, view};
```

如果filter中的组件想要使用todoList中的功能，应该导入todoList这个目录，因为导入一个目录的时候，默认导入的就是这个目录下的index.js文件，index.js文件中导出的内容，就是这个模块想要公开出来的接口。

> 注意：虽然每个模块目录下都会有一个actionTypes.js文件定义action类型，但是通常不会把actionTypes中内容作为模块的接口之一导出，因为action类型只有两个部分依赖，一个是reducer，一个是action构造函数，而且只有当前模块的reducer和action构造函数才会直接使用action类型。模块之外，不会关心这个模块的action类型，如果模块之外要使用这个模块的动作，也只需要直接使用action构造函数就行。

下面就是对应的导入todoList的代码：

```js
import {actions, reducer, view as TodoList} from  '../todoList';
```

当我们想要修改todoList的内部代码结构，比如把views目录改名为components目录，或者把container.js改名为TodoListView.js时，所要做的只是修改todoList目录下的index.js内容，而这个文件export出来的内容不会有任何改变，也就是说对导入todoList的代码不用任何改变。这就是我们确定模块边界想要达到的目的。

还有一种导出模块接口的方式，是不以命名式export的方式导出模块接口，而是以export default的方式默认导出，就像这样的代码：

```js
import * as actions from './actions.js';
import reducer from './reducer.js';
import view from './views/container.js';

export default {actions, reducer, view}
```

如果像上面这样导出，那么导入时的代码会有一点区别，因为ES6语法中，export default和export两种导出方式的导入方式也会不同，代码如下：

```js
import TodoListComponent from './actions.js';

const reducer = TodoListComponent.reducer;
const actions = TodoListComponent.actions;
const TodoList = TodoListComponent.view;
```

无论使用哪种导出方式，都请在整个应用中只用一中模块导出方式，保持一致，避免混乱。

在本书中，全部使用export的方式，因为从上面的代码看得出来，如果使用export default的方式，在导入的时候不可避免要使用多行代码才能得到actions、reducer和view，而用导入命名式export只用一行就可以搞定，相对而言要更加简洁。

读者可能注意到了，上面接口代码中导入actions的语句和导入view和reducer不一样，代码如下：

```js
import * as actions from './actions.js';
```

我们预期actions.js中是按照命名式export，原因和上面陈述的一样，actions.js可能会导出很多action构造函数，命名式导出是为了导入actions方便。对于view和reducer，一个功能模块绝对只有一个根视图模块，一个功能模块也只应该有一个导出的reducer，所以它们两个在各自代码文件中是以默认方式导出的。

---

### 四、状态树的设计

上面说的“代码文件组织结构”和“确定模块的边界”更多的只是确定规矩，然后在每个应用中我们只要都遵循这个规矩就足够了，而要注意的第三点“Store上状态树的设计”，更像是一门技术，需要我们动一动脑子。

因为所有的状态都存在Store上，Store的状态树设计，直接决定了要写哪些reducer，还有action怎么写，所以是程序逻辑的源头。

我们认为状态树设计要遵循如下几个原则：

* 一个模块控制一个状态节点

* 避免冗余数据

* 树形结构扁平

#### 1. 一个状态节点只属于一个模块

这个规则与其说是规则，不如说是Redux中模块必须遵守的限制，完全无法无视这个限制。

在Redux应用中，Store上的每个state都只能通过reducer来更改，而我们每个模块都有机会导出自己的reducer，这个导出的reducer只能最多更改Redux的状态树上一个节点下的数据，因为reducer之间对状态树上的修改权是互斥的，不可能让两个reducer都可以修改同一个状态树上的节点。

比如，如果A模块的reducer负责修改状态树上a字段下的数据，那么另一个模块B的reducer就不可能有机会修改a字段下的数据。

这里所说的“拥有权”指的是“修改权”，而不是“读取权”，实际上，Redux Store上的全部状态，在任何时候，对任何模块都是开放的，通过store.getState()总能够读取当前整个状态树的数据，但是只能更新自己相关那一部分模块的数据。

#### 2. 避免冗余数据



---

### 参考文献

1. [《深入浅出React和Redux —— 程墨》](/resources/深入浅出React和Redux.pdf)