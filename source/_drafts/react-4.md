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




---

### 三、

<!-- Todo -->


---

### 四、

<!-- Todo -->

### 参考文献

1. [《Todo》]()