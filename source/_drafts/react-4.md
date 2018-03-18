---
title: React(4) 模块化React和Redux应用
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

冗余数据是一致性的大敌，如果在Store上存储冗余数据，那么维持不同部分数据一致就是一个大问题。

传统的关系型数据库中，对数据结构的各种“范式化”，其实就是在去除数据的冗余。而近年风生水起的NoSQL运动，提倡的就是在数据存储中“去范式化”，对数据结构的处理和关系型数据库正好相反，利用数据冗余来减少读取数据库时的数据关联工作。

面向用户的应用处于性能的考虑，倾向于直接使用“去范式化”的应用。但是带来的问题就是维持数据一致性就会困难。

不同的应用当然应该从自己的需要出发，在选择数据库的问题上，选择SQL关系型数据库或者NoSQL类型的数据库要根据应用特点，这个问题不是我们要在本书中讨论的。但是要强调的是，不管服务器端数据库用的是“范式化”还是“去范式化”的数据存储方式，在前段Redux的Store中，一定要避免数据冗余的出现。

并不是说Redux应用不需要考虑性能，而是相对于性能问题，数据一致性的问题才更加重要。

在后面的章节中我们会介绍，即使使用“范式化”的无冗余数据结构，我们借助reselector等工具一样可以获得很高的性能。

#### 3. 树形结构扁平

理论上，一个树形结构可以有很深的层次，但是我们在设计Redux Store的状态树时，要尽量保持树形结构的扁平。

如果树形结构层次很深，往往意味着树形很复杂，一个很复杂的状态树是难以管理的，如果你曾不幸开发过Windows操作系统中依赖于“注册表”的应用，就一定深有体会，Windows中的注册表就是一个庞大而且层次很深的属性结果，看起来很灵活，实际上总让软件开发陷入麻烦的泥沼。

从代码的角度出发，深层次树形状态结构会让代码冗长。

假设，一个树形从上往下依次有A、B、C、D四个节点，为了访问节点D，就只能通过上面三层逐级访问，不过，谁也不敢保证A、B、C三个节点真的存在，为了防止运行时出错，代码就要考虑到所有的可能，最后为了访问D，代码不得不写成这样：

```js
const d = state.A && state.A.B && state.A.B.C &&& state.A.B.C.D
```

相信没有开发者会愿意写很多类似上面这样的代码。

---

### 五、Todo应用实例

了解上述创建应用的原则之后，我们现在终于可以开始构建Todo应用了。

Todo应用从界面上看应该由三部分组成：

* 待办事项的列表

* 增加新待办事项的输入框和按钮

* 待办事项过滤器，可以选择过滤不同状态的待办事项

看起来需要三个功能模块，但是第一部分和第二部分的关系密切，可以放在一个模块中，所以最后我们确定有两个功能模块todos和filter，其中todos包含第一部分和第二部分的功能。

我们遵循“按照功能组织”的原则来设计代码，创建三个目录来容纳各自的代码文件，每个目录下都有一个index.js文件，这是模块的边界。各个模块之间只能假设其他模块包含index.js文件，要引用模块只能导入index.js，不能够直接去导入其他文件，文件目录如下：

```cmd
todos/
  index.js
filter/
  index.js
```

#### 1. Todo状态设计

至于Todo应用状态，从界面上看，应用中可以有很多待办事项，并有先后顺序的关系，明显用一个数组很合适。所以，我们的状态树上应该有一个表示待办事项的数组。

至于每个待办事项，应该用一个对象代表，这个对象肯定要包含文字，记录待办事项的内容，因为我们可以把一个待办事项标记为“已完成”，所以还要有一个布尔字段记录是否完成的状态，当我们把一个待办事项标记位“已完成”或者“未完成”时，必须要能唯一确定一个待办事项对象，没有规则说一个待办事项的文字必须唯一，所以我们需要一个字段来唯一标识一个待办事项，所以一个待办事项的对象格式如下：

```json
{
  id: //唯一标识
  text: //待办事项内容
  completed: //布尔值，标识待办事项是否已完成
}
```

过滤器选项设定界面上显示什么样状态的代表事项，我们已知过滤器有三种选择：

* 全部待办事项

* 已完成待办事项

* 未完成待办事项

看起来就是一个列举类型的结构，不过JavaScript里面并没有原生的enum类型支持，所以我们只能用类似常量标识符的方式来定义三种状态。在代码中，可以分别用体现语义的ALL、COMPLETED和UNCOMPLETED代表这三种状态，但是这三个标识符的实际值的选择，也值得商榷。

最简单的方式，就是让这三个状态标识符的值是整型，比如这样的代码形式：

```js
const ALL = 0;
const COMPLETED = 1;
const UNCOMPLETED = 2;
```

但是，考虑到将来无论是debug还是产生log，一个数字在开发人员眼里不容易看出来代表什么意思，最后还需要对照代码才知道0代表ALL、1代表COMPLETED，这样很不方便。所以，开发中一个惯常的方法，就是把这些枚举型的常量定义为字符串，比如这样的代码：

```js
const ALL = 'all';
const COMPLETED = 'completed';
const UNCOMPLETED = 'uncompleted';
```

综合起来看，我们知道Todo应用的Store状态树大概是这样一个样子，JavaScript对象的表示形式如下：

```js
{
  todos: [
    {
      text: 'First todo',
      completed: false,
      id: 0
    },
    {
      text: 'Second todo',
      completed: false,
      id: 1
    }
  ],
  filter: 'all'
}
```

每当增加一个待办事项，就在数组类型的todos中增加一个元素，当药标记一个待办事项为“已完成”或者“未完成”，就更新对应待办事项的complete字段值，而哪些待办事项应该显示出来，则要根据todos和filter共同决定。

在应用的入口文件src/index.js中，我们和ControlPanel一样，用Provider包住最顶层的TodoApp模块，这样让store可以被所有组件访问到，代码如下：

```js
ReactDOM.render(
  <Provider store={store}>
    <TodoApp />
  </Provider>
  document.getElementById('root')
);
```

而在顶层模块src/TodoApp.js中，所要做的只是把两个关键视图显示出来，代码如下：

```js
import React from 'react';
import {view as Todos} from './todos/';
import {view as Filter} from './filter/';

function TodoApp() {
  return (
    <div>
      <Todos />
      <Filter />
    </div>
  );
}

export default TodoApp;
```

#### 2. action构造函数

确定好状态树的结构之后，接下来就可以写action构造函数了。

在todos和filter目录下，我们都要分别创造actionTypes.js和actions.js文件，这两个文件几乎每个功能模块都需要，文件如此命名是大家普遍接受的习惯。

在src/todos/actionTypes.js中，我们定义的是todos支持的action类型。在Todo应用中，支持对待办事项的增加、反转和删除三种action类型，代码如下：

```js
export const ADD_TODO = 'TODO/ADD';
export const TOGGLE_TODO = 'TODO/TOGGLE';
export const REMOVE_TODO = 'TODO/REMOVE';
```

和index.js中使用命名式导出而不用默认导出一样，在actionTypes中我们也使用命名式导出，这样，使用actionTypes的文件可以这样写：

```js
import {ADD_TODO, TOGGLE_TODO, REMOVE_TODO} from './actionTypes.js';
```

也同样是为了便于debug和输出到log里面查看清晰，所有的action类型的值都是字符串，字符串还有一个好处就是可以直接通过===来比较是否相等，而其他对象用===则要求必须引用同一个对象。

> 提示：严格来说，使用Symbol来代替字符串表示这样的枚举值更合适，但是有的浏览器并不支持Symbol，我们在这里不作深入探讨。

考虑到应用可以无限扩展，每个组件也要避免明明冲突。所以，最好是每个组件的action类型字符串都有一个唯一的前缀。在我们的例子中，所有todos的action类型字符串都有共同前缀“TODO/”，所有filter的action类型字符串前缀“FILTER/”。

在src/todos/actions.js中，我们定义todos相关的action构造函数，代码如下：

```js
import {ADD_TODO, TOGGLE_TODO, REMOVE_TODO} from './actionTypes.js';

let nextTodoId = 0;

export const addTodo = (text) => ({
  type: ADD_TODO,
  completed: false,
  id: nextTodoId++,
  text: text
});

export const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  id: id
});

export const removeTodo = (id) => ({
  type: REMOVE_TODO,
  id: id
});
```

在上一章中我们已经知道，Redux的action构造函数就是创造action对象的函数，返回的action对象中必须有一个type字段代表action类型，还可能有其他字段代表这个动作承载的数据。

在src/todos/actions.js文件中定义了一个文件级别的变量nextTodoId，每调用一次addTodo action构造函数就加一，实现为每个产生的待办事项赋予一个唯一id的目的。当然，这种方法非常简陋，我们在后面的章节中会改进唯一id的生成方法。

读者可能会注意到我们用了一种新的写法，虽然action构造函数应该是一个返回action对象的方法，我们却看不见return的字样。对于只return一个对象的函数体，ES6允许简写为省去return，直接用圆括号把返回的对象包起来就行，比如上面的toggleTodo对象构造器，实际上是下面方法的简写：

```js
export const toggleTodo = (id) => {
  return {
    type: TOGGLE_TODO,
    id: id
  }
};
```

#### 3. 组合reducer

在todos和filter目录下，各有一个reducer.js文件定义的两个功能模块reducer。

对于reducer我们并不陌生，在前面的ControlPanel例子中我们就创建过reducer。但是在那个例子中，整个应用只有一个reducer。而在Todo应用中，两个功能模块都有自己的reducer，而Redux的createStore函数只能接受一个reducer，那么怎么办？

这是Redux最有意思的部分，虽然Redux的createStore只接受一个reducer，却可以把多个reducer组合起来，成为一体，然后就可以被createStore函数接受。

在src/Store.js文件中，我们完成了reducer的组合，代码如下：

```js
import {createStore, combineReducer} from 'redux';

import {reducer as todoReducer} from './todos';
import {reducer as filterReducer} from './filter';

const reducer = combineReducer({
  todos: todoReducer,
  filter: filterReducer
});

export default createStore(reducer);
```

我们使用了Redux提供的一个函数combineReducers来把多个reducer函数合成为一个reducer函数。

combineReducers函数接受一个对象作为参数，参数对象的每个字段名对应了state状态上的字段名（在上面的例子中分别是todoReducer和filterReducer），combineReducers函数返回一个新的reducer函数，当这个新的reducer函数被执行时，会把传入的state参数对象拆开处理，todo字段下的子状态交给todoReducer，filter字段下的子状态交给filterReducer，然后再把这两个调用的返回结果合并成一个新的state，作为整体reducer函数的返回结果。

假设，当前State上的状态可以用currentState代表，这时候给Store派发一个action对象，combineReducers产生的这个reducer函数就会被调用，调用参数state就是currentState。这个reducer函数会分别调用todoReducer和filterReducer，不过传递过去的state参数有些变化，调用todoReducer的参数state值是currentStore.todos，调用filterReducer的state是currentState.filter，当todoReducer和filterReducer这两个函数返回结果之后，combineReducers产生的reducer函数就用这两个结果分别去更新Store上的todos和filter字段。

所以，现在我们来看功能模块的reducer，会发现state的值不是Redux上那个完整的状态，而是状态上对应自己的那一部分。

在src/todos/reducer.js中可以看到，state参数对应的是Store上todos字段的值，默认值是一个数组，reducer函数往往就是一个以action.type为条件的switch语句构成，代表模式如下：

```js
import {ADD_TODO, TOGGLE_TODO, REMOVE_TODO} from './actionTypes.js';

export default (state = [], action) => {
  switch (action.type) {
    // 针对action.type所有可能值的case语句

  }
};
```

先看对于ADD_TODO这种action类型的处理，代码如下：

```js
case ADD_TODO: {
  return [
    {
      id: action.id,
      text: action.text,
      completed: false
    },
    ...state
  ]
}
```

这里，我们使用了ES6的扩展操作符来简化reducer的代码，扩展操作符可以用来扩展一个对象，也可以用来扩展一个数组。

现在state是一个数组，我们想要返回一个增加了一个对象的数组，就这样写：

```js
return [newObject, ...state];
```

为什么我们不直接使用熟悉的数组push或者unshift操作呢？

绝对不能，因为push和unshift会改变原来的那个数组，还记得吗？reducer必须是一个纯函数，纯函数不能有任何副作用，包括不能修改参数对象。

对于TOGGLE_TODO这种action累i系那个的处理，代码如下：

```js
case TOGGLE_TODO: {
    return state.map((todoItem) => {
      if (todoItem.id === action.id) {
        return {...todoItem, complated: !todoItem.completed};
      } else {
        return todoItem;
      }
    });
}
```

扩展操作符可以在一对{}符号中把一个对象展开，这样，在{}中后面的部分的字段值，可以覆盖展开的部分：

```js
return {...todoItem, completed: !todoItem.completed};
```

像上面的代码中，返回了一个新的对象，所有字段都和todoItem一样，只是completed字段和todoItem中的completed布尔类型值正好相反。

对于REMOVE_TODO这种action类型的处理，代码如下：

```js
case REMOVE_TODO: {
  return state.filter((todoItem) => {
    return todoItem.id !== action.id;
  });
}
```

对于删除操作，我们使用数组的filter方法，将id匹配的待办事项过滤掉，产生了一个新的数组。

最后，reducer中的switch语句一定不要漏掉了default的条件，代码如下：

```js
default: {
  return state;
}  
```

因为reducer函数会接收到任意action，包括它根本不感兴趣的action，这样就会执行default中的语句，应该将state原样返回，表示不需要更改state。

在src/filter/reducer.js中定义了filter模块的reducer，代码如下：

```js
import {SET_FILTER} from './actionTypes.js';
import {FilterTypes} from '../constants.js';

export default (state = FilterTypes.ALL, action) = {
  switch (action.type) {
    case SET_FILTER: {
      return action.filter;
    }
    default:
      return state;
  }
}
```

这个reducer更加简单，所做的就是把Redux Store上filter字段的值设为action对象上的filter值。

我们来总结一下Redux的组合reducer功能，利用combineReducers可以把多个只针对局部状态的“小的”reducer合并为一个操纵整个状态树的“大的”reducer，更妙的事，没有两个“小的”reducer会发生冲突，因为无论怎么组合，状态树上一个子状态都只会被一个reducer处理，Redux就是用这种方法隔绝了各个模块。

很明显，无论我们有多少“小的”reducer，也无论如何组合，都不用在乎它们被因为调用的顺序，因为调用顺序和结果没有关系。

#### 4. Todo视图

对于一个功能模块，定义action类型、action构造函数和reducer基本上各用一个文件就行，约定俗成地分别放在模块目录下（actionTypes.js、actions.js和reducer.js文件中）。但是，一个模块涉及的视图文件往往包含多个，因为对于充当视图的React组件，我们往往会让一个React组件的功能精良、小，导致视图分布在多个文件之中。

既然有多个文件，那么也就没有太大必要保持统一的文件名，反正模块导出的只是一个view字段，在模块内部只要文件名能够表达视图的含义就行。

* 1) todos视图

对于todos模块，在index.js中被导出的view存在src/todos/views/todos.js中，代码如下：

```js
import React from 'react';
import AddTodo from './addTodo.js';
import TodoList from './todoList.js';

export default () => {
  return (
    <div className="todos">
      <AddTodo />
      <TodoList />
    </div>
  )
}
```

很简单，就是把AddTodo组件和TodoList两个组件摆放在一个div中，这样的简单组合自然不需要什么状态，所以用一个函数表示成无状态组件就可以了。

在Todos的顶层div上添加了className属性，值为字符串todos，最后产生的DOM元素上就会有CSS类todos。这个类是为了将来定制样式而使用的，并不影响功能。

对于AddTodo组件，涉及处理用户的输入。当用户点击“增加”按钮或者在输入栏input中直接回车键的时候，要让JavaScript读取到input这个DOM元素的value值，React为了支持这种方法，提供了一个叫ref的功能。

在src/todos/views/addTodo.js中有对AddTodo组件的定义，我们先来看其中render函数对ref的用法，代码如下：

```js
render() {
  return (
    <div className="add-todo">
      <form onSubmit={this.onSubmit}>
        <input className="new-todo" ref={this.refInput} />
        <button className="add-btn" type="submit">
        添加
        </button>
      </form>
    </div>
  )
};
```

当一个包含ref属性的组件完成装载（mount）过程的时候，会看一看ref属性是不是一个函数，如果是，就会调用这个函数，参数就是这个组件代表的DOM元素，注意，是DOM元素，不是Virtual DOM元素，通过这种方法，我们的代码可以访问到实际的DOM元素。

AddTodo的render渲染出来了一个form，通过onSubmit属性把form被提交的事件挂在AddTodo组件的onSubmit函数上。

在上面的例子中，input元素的ref属性是AddTodo组件的一个成员函数refInput，所以当这个input元素完成装载之后，refInput会被调用。

refInput的函数代码如下：

```js
refInput(node) {
  this.input = node;
}
```

当refInput被调用时，参数node就是那个input元素，refInput把这个node记录在成员变量input中。

于是，当组件完成mount之后，就可以通过this.input来访问那个input元素。这是一个DOM元素，可以使用任何DOM API访问元素内容，通过访问this.input.value就可以直接读取当前的用户输入。

使用this.input的onSubmit函数代码如下：

```js
onSubmit(ev) {
  ev.preventDefault();

  const input = this.input;
  if (!input.value.trim()) {
    return;
  }

  this.props.onAdd(input.value);
  input.value = '';
}
```

在HTML中，一个form表单被提交的默认行为会引发网页跳转，在React应用中当然不希望网页跳转发生，所以我们首先通过调用参数ev的preventDefault函数取消掉浏览器的默认提交行为。

通过this.input.value，可以判断出当前输入框里的文字内容。如果是空白，就应该忽略，因为创建一个空白的待办事项没有意义。否则，就调用通过属性onAdd传递进来的函数。最后，我们把input清空，让用户知道创建成功，而且方便创建下一条待办事项。

文件中的AddTodo组件是一个内部组件，按说应该是一个傻瓜组件，但是和我们之前例子中的傻瓜组件不一样，AddTodo不是一个只有一个render函数的组件，AddTodo甚至有一个逻辑比较复杂的onSubmit函数，为什么不把这部分逻辑提取到外面的容器组件呢？

其实我们可以把onSubmit的逻辑提取到mapDispatchToProps中。但是，让AddTodo组件外面的mapDispatchToProps访问到AddTodo组件里面的ref很困难，得不偿失。

使用ref实际上就是直接触及了DOM元素，与我们想远离DOM是非之地的想法相悖，虽然React提供了这个功能，但是还是要谨慎使用，如果要用，我们进尽量让ref不要跨越组件的边界。

所以，我们把通过ref访问input.value放在内部的AddTodo中，但是把调用dispatch派发action对象的逻辑放在mapDispatchToProps中，两者一个主内一个主外，各司其职，不要混淆。

> 注意：并不是只有ref一种方法才能够访问input元素的value，我们在这里使用ref主要是展示一下React的这个功能，在后面的章节中我们会介绍更加可控的方法。

注意，对于AddTodo，没有任何需要从Redux Store的状态衍生的属性，所以最后一行的connect函数第一个参数mapStateToProps是null，只是用了第二个参数mapDispatchToProps。

在src/todos/views/addTodo.js中表示AddTodo标识符代表的组件和src/todos/views/todos.js中AddTodo标识符代表的组件不一样，后者是前者用react-redux包裹之后的容器组件。

接下来看看待办事项列表组件，定义在src/todos/view/todoList.js中，在渲染TodoList时，我们的todos属性是一个数组，而数组元素的个数是不确定的，这就涉及如何渲染数量不确定子组件的问题，TodoList作为无状态组件代码如下：

```js
const TodoList = ({todos, onToogleTodo, onRemoveTodo}) => {
  return (
    <ul className="todo-list">
    {
      todos.map((item) => (
        <TodoItem
          key={item.id}
          text={item.text}
          completed={item.completed}
          onToggle={() => onToggleTodo(item.id)}
          onRemove={() => onRemoveTodo(item.id)}
        />
      ))
    }
    </ul>
  )
}
```

这段代码中的TodoList真的就只是一个无状态的傻瓜组件了，对于传递进来的todos属性，预期是一个数组，通过一个数组的map函数转化为一个TodoItem组件的数组，这是我们第一次在JSX代码片段中创建动态数量的元素。

因为todos这个数组的元素数量并不确定，所以很自然想到要用一个循环来产生同样数量的TodoItem组件实例。但是，并不能在JSX中使用for或者while这样的循环语句。因为，JSX中可以使用任何形式的JavaScript表达式，只要JavaScript表达式出现在符号{}之间，但是也只能是JavaScript“表达式”，for或者while产生的是“语句”而不是“表达式”，所以不能出现for或者while。

归根到底，JSX最终会被babel转译成一个嵌套的函数调用，在这个函数调用中自然无法插入一个语句进去，所以，当我们想要在JSX中根据数组产生动态数量的组件实例，就应该用数组的map方法。

还有一点很重要，对于动态数量的子组件，每个子组件都必须要带上一个key属性，而且这个key属性的值一定要是能够唯一标示这个子组件的值。

当我们完成Todo应用之后，会回过头来再解释key值的作用。

TodoList的mapStateToProps方法需要根据Store上的filter状态决定todos状态上取哪些元素来显示，这个过程涉及对filter的switch判断。为了防止mapStateToProps方法过长，我们将这个逻辑提取到selectVisibleTodos函数中，代码如下：

```js
const selectVisibleTodos = (todos, filter) => {
  switch (filter) {
    case FilterTypes.ALL:
      return todos;
    case FilterTypes.COMPLETED:
      return todos.filter(item => item.completed);
    case FilterTypes.UNCOMPLETED:
      return todos.filter(item => !item.completed);
    default:
      throw new Error('unsupported filter');
  }
}

const mapStateToProps = (state) => {
  return {
    todos: selectVisibleTodos(state.todos, state.filter)
  };
}
```

最后，我们看TodoList的mapDispatchToProps方法，代码如下：

```js
const mapDispatchToProps = (dispatch) => {
  return {
    onToggleTodo: (id) => {
      dispatch(toggleTodo(id));
    },
    onRemoveTodo: (id) => {
      dispatch(removeTodo(id));
    }
  };
};
```

在TodoList空间中，看mapDispatchToProps函数产生的练个新属性onToggleTodo和onRemoveTodo的代码遵循一样的模式，都是把接收到的参数作为参数传递给一个action构造函数，然后用dispatch方法把产生的action对象派发出去，这看起来是重复代码。

实际上，Redux已经提供了一个bindActionCreators方法来消除这样的重复代码，显而易见很多mapDispatchToProps要做的事情只是把action构造函数和prop关联起来，所以直接以prop名为字段名，以action构造函数为对应字段值，把这样的对象传递给bindActionCreators就可以了，上面的mapDispatchToProps可以简写为这样：

```js
import {bindActionCreators} from 'redux';

const mapDispatchToProps = (dispatch) => bindActionCreators({
  onToggleTodo: toggleTodo,
  onRemoveTodo: removeTodo
}, dispatch);
```

更进一步，可以直接让mapDispatchToProps是一个prop到action构造函数的映射，这样连bindActionCreators函数都不用，代码如下：

```js
const mapDispatchToProps = {
  onToggleTodo: toggleTodo,
  onRemoveTodo: removeTodo
};
```

上面定义的mapDispatchToProps传给connect函数，产生的效果和之前的写法完全一样。

我们再看定义了TodoItem的src/todos/views/todoItem.js文件，代码如下：

```js
const TodoItem = ({onToggle, onRemove, completed, text}) => (
  <li 
    className="todo-item" 
    style={{
      textDecoration: completed ? 'line-through': 'none'
      }}
  >
    <input 
      className="toggle"
      type="checkbox"
      checked={completed ? "checked": ""}
      readOnly
      onClick={onToggle}
      />
    <label className="text">{text}</label>
    <button className="remove" onClick={onRemove}>X</button>
  </li>
)
```

这里的TodoItem就是一个无状态的组件，把onToggle属性挂到checkbox的点击事件上，把onRemove属性挂到删除按钮的点击事件上。

每个待办事项都包含一个checkbox元素和一段文字内容，checkbox是否勾选并不依赖用户输入，而是根据completed属性来判断。同时，对于completed状态的待办事项，文字内容中间用横线代表完成。

* 2) filter视图

对于过滤器，我们有三个功能类似的链接，很自然就会想到把链接相关的功能提取出来，放在一个叫Link的组件中。

在src/filter/views/filter.js中，我们定义被导出的filter主视图，代码如下：

```js
const Filter = () => {
  return (
    <p className="filters">
      <Link filter={FilterTypes.ALL}>{FilterTypes.ALL}</Link>
      <Link filter={FilterTypes.COMPLETED}>{FilterTypes.COMPLETED}</Link>
      <Link filter={FilterTypes.UNCOMPLETED}>{FilterTypes.UNCOMPLETED}</Link>
    </p>
  )
}
```

这个filter主视图很简单，是一个无状态的函数，列出了三个过滤器，把实际工作都交给了Link组件。

我们在src/filter/views/link.js中添加Link组件：

```js
const Link = ({active, children, onClick}) => {
  if (active) {
    return (
      <b className="filter selected">{children}</b>
      );
  } else (
    return (
      <a href="#" className="filter no-selected" onClick={(ev) => {
        ev.preventDefault();
        onClick();
      }}>
        {children}
      </a>
    )
  )
}
```

作为傻瓜组件的Link，当传入属性active为true时表示当前实例就是被选中的过滤器，不该被再次选择，所以不应该渲染超链接标签a。若否，则渲染一个超链接标签。

一个超链接标签的默认行为是跳转，即href属性是#，所以超链接标签的onClick事件处理器第一件事就是用preventDefault函数取消默认行为。实际上，我们把href设为“#”唯一的目的就是让浏览器给超链接显示出下划线，代表这是一个链接。

我们使用了一个特殊的属性children，对于任何一个React组件都可以访问这样一个属性，代表的是被包裹住的子组件，比如，对于下面的代码：

```js
<Foo>
  <Bar>WhatEver</Bar>
</Foo>
```

Foo组件实例的children就是`<Bar>WhatEver</Bar>`，而Bar组件实例的children就是`WhatEver`。在render函数中把children属性渲染出来，是惯常的组合React组件的方法。

Link的mapStateToProps和mapDispatchToProps函数都很简单，代码如下：

```js
const mapStateToProps = (state, ownProps) => {
  return {
    active: state.filter === ownProps.filter
  };
};

const mapDispatchToProps = (dispatch, ownProps) => {
  onClick: () => {
    dispatch(setFilter(ownProps.filter));
  }
}

```

#### 5. 样式

我们终于完成了Todo应用，在浏览器中可以看到最终效果。

![无定制样式的Todo应用界面](/images/react-4/1.png)

这个Todo应用功能已经完成，但是并没有定制样式，还是需要通过CSS来添加样式。我们在定义视图部分代码时，一些元素上通过className属性添加了CSS类，现在我们就利用这些类来定义CSS规则。

在src/todos/views/style.css中，我们定义了Todo空间中所有的样式，为了让定义的样式产生效果，在Todos组件的最顶层视图文件src/todos/views/todos.js中添加如下代码：

```js
import './style.css';
```

在React应用中，通常使用webpack来完成对JavaScript的打包，create-react-app产生的应用也不例外，不过webpack不知能够处理JavaScript文件，它还能够处理css、scss甚至图片文件，因为在webpack眼里，一切文件都是“模块”，通过文件中的import语句或者require函数调用就可以找到文件之间的使用关系，只要被import就会被纳入最终打包的文件中，即使被import的是一个css文件。

如下图所示，Todo应用拥有样式了。

![完成定制样式的Todo应用界面](/images/react-4/2.png)

把css文件用import语句导入，webpack默认的处理方式是将css文件的内容嵌入最终的打包文件bundle.js中，这毫无疑问会让打包文件变得更大，但是应用所有的逻辑都被包含在一个文件中了，便于部署。

当然，如果不希望将css和JavaScript混在一起，也可以在webpack中通过配置完成，在webpack的loader中使用extract-text-webpack-plugin，就可以让css文件在build时被放在独立的css文件中。

有意思的是，选择css和JavaScript打包在一起还是分开打包，和代码怎么写没有任何关系，这就是React的妙处。代码中只需要描述“想要什么”，至于最终“怎么做”，可以通过配置webpack获得多重选择。

如果使用scss语法，可以简化上面的样式代码，但是create-react-app产生的应用默认不支持scss，有兴趣的话可以通过eject方法直接编辑webpack配置，应用上scss加载器。

#### 6. 不使用ref

在前面的例子中，代码通过React的ref功能来访问DOM元素，这种功能的需求往往来自于提交表单的操作，在提交表单的时候，需要读取当前表单中input元素的值。

毫无疑问，ref的用法非常脆弱，因为React的产生就是为了避免直接操作DOM元素，因为直接访问DOM元素很容易产生失控的情况，现在为了读取某个DOM元素的值，通过ref取得对元素的直接引用，不得不说，干的并不漂亮。

有没有更好的方法呢？

有，可以利用组件状态来同步记录DOM元素的值，这种方法可以控制住组件不使用ref。

Todo应用中的addTodo.js文件可以这样修改，首先是render函数，代码如下：

```js
render() {
  return (
    <div className="add-todo">
      <form onSubmit={this.onSubmit}>
        <input 
          className="new-todo" 
          onChange={this.onInputChange} 
          value={this.state.value} />
        <button className="add-btn" type="submit">
          添加
        </button>
      </form>
    </div>
  )
}
```

在这里我们不再使用ref来获取对input元素的直接访问，而是利用input元素上的onChange属性挂上一个事件处理函数onInputChange，这样每当input的值发生变化时，onInputChange函数就会被调用，这样我们总有机会记录下最新的input元素内容。

onInputChange函数的代码如下：

```js
onInputChange(event) {
  this.setState({
    value: event.target.value
  });
}
```

在onInputChange函数中，通过传入的事件参数event可以访问事件发生的目标，读取到当前值，并把内容存在组件状态的value字段上。这样，组件状态上总能够获取最新的input元素内容。

然后看onSubmit函数的改变，代码如下：

```js
onSubmit(ev) {
  ev.preventDefault();

  const inputValue = this.state.value;
  if (!inputValue.trim()) {
    return;
  }

  this.props.onAdd(inputValue);
  this.setState({value: ''});
}
```

当需要提交表单的时候，onSubmit函数被调用，只需要直接从组件状态上读取value字段的值就够了。

在产品开发中，应该尽量避免ref的使用，而换用这种状态绑定的方法来获取元素的值。

---

### 六、开发辅助工具

我们已经写了一个比较复杂的基于React和Redux的应用，将来还会遇到更复杂的应用，是时候引入一些开发辅助工具了，毕竟没有人能够一次把复杂代码写对，必要的工具能够大大提高我们的工作效率。

#### 1. Chrome扩展包

Chrome是网页应用开发者群体最喜爱的浏览器，因为Chrome浏览器有丰富的扩展库来帮助网页开发，这里我们介绍三款Chrome扩展包，可以说是React和Redux应用开发必备之物。

* React Devtools，可以检视React组件的树形结构

* Redux Devtools，可以检视Redux数据流，可以将Store状态跳跃到任何一个历史状态，也就是所谓的“时间旅行”功能

* React Perf，可以发现React组件渲染的性能问题

#### 2. redux-immutable-state-invariant辅助包

我们曾经反复强调过，每一个reducer函数都必须是一个纯函数，不能修改传入的参数state和action，否则会让应用重新陷入状态不可预料的境地。

禁止reducer函数修改参数，这是一个规则，规则总是会被无心违反的，但是怎么避免开发者不小心违反这个规则呢？

有一个redux-immutable-state-invariant包，提供了一个Redux的中间件，能够让Redux在每次派发动作之后做一个检查。如果发现某个reducer违反了作为一个纯函数的规定擅自修改了参数state，就会给一个大大的错误警告，从而让开发者意识到自己犯了一个错误，必须要修正。

什么是Redux的中间件？我们在后面的章节会详细介绍，在这里我们只要理解中间件是可以增强Redux的Store实例功能的一种方法就可以。

很明显，对于redux-immutable-state-invariant的这种检查，在开发环境下很有用。但是在产品环境下，这样的出错提示意义不大，只是徒耗计算资源和增大JavaScript代码提及，所以我们通常在产品环境中不启用redux-immutable-state-invariant。

#### 3. 工具应用

上面我们介绍了辅助开发的Chrome扩展包和redux-immutable-state-variant库，对于React Devtools来说，启用只是安装一个Chrome扩展包的事，但是对于其余几个工具，我们的代码需要做一些修改才能配合浏览器使用。

因为Store是Redux应用的核心，所以所有的代码修改都集中在src/Store.js中。

首先需要从Redux引入多个函数，代码如下：

```js
import {createStore, combineReducers, applyMiddleware, compose} from 'redux';
```

为了使用React Perf插件，需要添加如下代码：

```js
import Perf from 'react-addons-perf'

const win = window;

win.Perf - Perf;
```

在这里把window赋值给模块级别变量win，是为了帮助代码缩小器（minifer），在webpack中缩小代码的插件叫UglifyJsPlugin，能够将局部变量名改成很短的变量名，这样功能不受影响但是代码的大小大大缩减。

不过，和所有的代码缩小器一样，UglifyJsPlugin不敢去改变全局变量名，因为改了就会影响程序的功能。所以当多次引用window这样的全局变量时，最好在模块开始将window赋值给一个变量，比如win，然后在代码其它部分只使用win，这样最终经过UglifyJsPlugin产生的缩小代码就只包含一个无法缩小的window变量。

我们给win上的Perf赋值了从react-addons-perf上导入的内容，这是为了帮助React Perf扩展包的使用，做了这个代码修改之后，React Perf上的Start按钮和Stop按钮才会起作用。

为了应用redux-immutable-state-invariant中间件和Redux Devtools，需要使用Redux的Store Enhancer功能，代码如下：

```js
const reducer = combineReducers({
  todos: todoReducer,
  filter: filterReducer
});

const middlewares = [];

if (process.env.NODE_ENV !== 'production') {
  middlewares.push(require('redux-immutable-state-invariant')());
}

const storeEnhancers = compose(
  applyMiddleware(...middlewares),
  (win && win.devToolsExtension) ? win.devToolExtension() : (f) => f
);

export default createStore(reducer, {}, storeEnhancers);
```

代码修改的关键在于给createStore函数增加了第三个参数storeEnhancers，这个参数代表Store Enhancer的意思，能够让createStore函数产生的Store对象具有更多更强的功能。

因为Store Enhancer可能有多个，在我们的例子中就有两个，所以Redux提供了一个compose函数，用于把多个Store Enhancer组合在一起，我们分别来看这两个Store Enhancer是什么。

第一个Store Enhancer是一个函数applyMiddleware的执行结果，这个函数接受一个数组作为参数，每个元素都是一个Redux的中间件。虽然目前我们只应用了一个中间件，但是考虑到将来会扩展更多，所以我们用数组变量middlewares来存储所有的中间件，将来要扩展新的中间件只需要往这个数组中push新的元素就可以了。

目前，往middlewares中push的唯一一个中间件就是redux-immutable-state-invariant。如同上面解释过的，redux-immutable-state-invariant只有在开发环境下使用才有意义，所以只有当process.env.NODE_ENV不等于production时才加入这个中间件。

我们一直按照ES6的语法导入模块，也就是用import关键字导入模块，但是在这里却用了Node传统风格require，是因为import语句不能够存在于条件语句之中，只能出现在模块语句的顶层位置。

第二个Store Enhancer就是Redux Devtools，因为Redux Devtools的工作原理是截获所有应用中Redux Store的动作和状态变化，所以必须通过Store Enhancer在Redux Store中加入钩子。

如果浏览器中安装了Redux Devtools，在全局window对象上就有一个devToolsExtension代表Redux Devtools。但是，我们也要让没有安装这个扩展包的浏览器也能正常使用我们的应用，所以要根据window.devToolsExtension是否存在做一个判断，如果有就用，如果没有就插入一个什么都不做的函数。

```js
(win && win.devToolsExtension) ? win.devToolsExtension() : (f) => f
```

当所有的代码修改完毕，重新启动Todo应用，在浏览器的开发者工具中就可以使用所有关于React和Redux的工具了。

---

### 七、本章小结

在这一章，我们学习了构建一个完整Redux应用的步骤和需要考虑的方面。

首先，要考虑代码文件的组织方式，对于可以高度模块化的Redux应用，使用“按功能组织”的方式要优于传统MVC框架下“按角色组织”的方式。

之后，要考虑Store上状态树的设计，因为状态树的结构直接决定了模块的划分，以及action类型、action构造函数和reducer的设计。可以说，开始写Redux应用第一行代码之前，首先要想好Store的状态树长得什么样子。

然后，我们实际构建了一个Todo应用，这个应用要比之前的ControlPanel应用复杂，利用划分模块的方法解决才是正道，从中我们也学习了React的ref功能，以及动态数量的子空间必须要包含key属性。

最后，我们了解了开发React和Redux应用必备的几样辅助工具，有了这几样工具，开发React和Redux应用就会如虎添翼。

这只是个起点，在接下来的章节中，我们会进一步深入了解React和Redux的精髓。

---

### 参考文献

1. [《深入浅出React和Redux —— 程墨》](/resources/深入浅出React和Redux.pdf)