---
layout: article
title: 01. Object-Oriented Design Principles
author: J_宋
tags: SoftwareSystemDesign 中文
mathjax: true
key: systemDesign01
---

#### 软件设计的过程

1. 了解用户的需求，明确需求是啥？ -- **需求**
2. 根据需求来确定系统外部观察到的行为是什么样的？ -- **规约**
3. 如何联系？使用什么样的技术？-- **架构**（这还是比较高层的设计）
4. 如何来完成任务？需要写哪一些代码？-- **设计**

####  面向对象软件设计

- 将实现的约束条件应用到面向对象分析所产生的概念模型的过程
- 用<span style="color:blue">方法</span>和<span style="color:blue">属性</span>来描述用于构成系统的类
- 添加不明显属于领域的类，比如<span style="color:blue">抽象类</span>和<span style="color:blue">接口</span>
- 描述类是如何构成组件的



####  OOD所处的环节

 <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 9.00.53 PM.png" alt="Screen Shot 2021-03-02 at 9.00.53 PM" style="zoom:50%;" />

1. OOA : 概念模型
   - 面向对象的问题分析
   - 问题中间有需要那些实现的功能，他们对应到那些类？
2. OOD ：面向对象的设计
   - 添加了一些额外的不是明显属于这个领域的类 (比如<span style="color:blue">抽象类</span>和<span style="color:blue">接口</span>)
   - 这个其实是真正要去实现的模型
3. OOP ：Object-oriented program
   - 直接对应到程序



#### 面向对象设计原则 (7条原则)

⚠️ 每条设计原则要讲的是什么

⚠️ 他们是如何混合在一起来完成设计任务

##### 0. 概述

- 面向对象设计主要只考虑<span style="color:red">**可维护性**，**可复用性**</span>
  - 为啥？👉 因为软件设计有一个重要的目标，软件在经后演化的过程中间降低维护成本，开发成本
- 可维护性和可复用性是，虽然他们有共性，但是是*独立的目标*
  - "它可维护了"不一定代表"这个软件一定可复用"
  - "它可复用了"不一定代表"这个软件一定可维护"
  - 但是，对于面向对象的软件系统设计来说，我们希望支持可维护性同时提高系统的可复用性
- 什么是可维护性（Maintainability）的软件？
  - 过于僵硬（Rigidity） ：ex) 所有的修改都需要面向程序的源代码来进行的时候
  - 过于脆弱（Fragility）：ex) 修改一个代码的时候，导致看起来没有关系的另外一个地方发生故障
  - 复用率底（Immobility）
  - 黏度过高（Viscosity）：主要是指架构层面上。系统进行改动的是时候，如果说需要破坏原始的意图和框架

- 什么是好的系统设计？
  - 可扩展性 💁🏻 对应关系 ： 过于僵硬
  - 灵活性 💁🏻 对应关系 ： 过于脆弱
  - 可插入行 💁🏻 对应关系 ： 黏度过高
- 为啥要在面向对象语言中间加入抽象，继承，封装，多态这些特征呢？
  - 都是为了实现更高层次的复用性，维护性
- 软件重构 ( *超出本科的范围，不展开说明* )
  - 在不改变软件现有功能的基础上，通过调整程序代码改善软件的质量、性能，使其程序的设计模式和架构更趋合理，提高软件的扩展性和维护性



##### 1. 单一职责原则

- Single Responsibility Principle, SRP

- 一个对象应该只包含单一的职责

- 应该仅有一个引起它变化的原因
  - 若我们认为这点是可能有变化的，单独有一个类来处理这个变化

- 一个类(或者大到模块，小到方法)承担的职责越多，它被复用的可能性越小
  - 为啥？👉 一个值的变化的时候，可能会影响这个类，所以这个类很难被复用。

- 那么一个类应该主要包括那些职责？
  - 数据职责 -- 通过属性来体现
  - 行为职责 -- 通过方法来体现

- 单一职责原则是实现<span style="color:red">高内聚、低耦合</span>

- ⚠️ 单一职责是从变化的原因入手的
  - 设计的时候，你需要提前能够有一个预判，那些需求是最有可能发生变化
  - 软件不断演化过程中间，对于我们发现这个东西经常变化，影响比较大的时候，我们进行重构做成一个单一的类或者单一的模块

- 实例 ：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 10.22.53 PM.png" alt="Screen Shot 2021-03-02 at 10.22.53 PM" style="zoom:50%;" />

  👆设计不太好的原因 -- Login类中间有多个职责，导致经常不能复用，维护性也很差

  - 重构之后 ：

    <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 10.28.30 PM.png" alt="Screen Shot 2021-03-02 at 10.28.30 PM" style="zoom:30%;" />

    - MainClass只负责启动系统
    - UserDAO封装了CRUD数据库操作
    - DBUtil负责数据库的链接



##### 2. 开闭原则

- Open-Closed Principle, OCP

- 一个软件实体应当<span style="color:red">对扩展开放</span>，<span style="color:red">对修改关闭</span>。
  - 增加功能的时候，或者修改原有功能的时候，我不动原有的任何代码，而是通过新的代码来实现

- 什么是好的可维护性？

  - 它是可以用新的代码来替换久的代码

- <span style="color:blue">抽象化</span>（= 用新的代码来替换久的代码）是开闭原则的关键

- 对可变性封装原则（Principle of Encapsulation of Variation, EVP）:

  - 变化的部分封装起来，封装成一个类，去它进行替换

- 实例 ：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 10.54.52 PM.png" alt="Screen Shot 2021-03-02 at 10.54.52 PM" style="zoom:60%;" />

  - 重构之后：

    <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 10.57.21 PM.png" alt="Screen Shot 2021-03-02 at 10.57.21 PM" style="zoom:30%;" />

      - 最难的地方是“识别变化” ： 什么时候要封装？



##### 3.  里氏代换原则

- Liskov Substitution Principle, LSP

- 实现开闭原则的关键

- 所有引用基类(父类)的地方必须能透明地使用其子类的对象

  - <span style="color:red">子类的所有方法都必须在父类中声明 :arrow_right: 这一点保证了我们子类是可以互相替换</span>
    - 子类不能去实现额外的方法..?
    - 如果子类实现了新的方法，会造成什么问题？:arrow_right:  子类实现了新的方法之后我们能不能很好的应用开闭原则来对子类进行一些修改啊？
  - <span style="color:red">子类可以扩展一些新的private方法</span>，这些private方法可以在子类自己的public方法中调用，这没有问题。
    - 但是，你不应该去额外增加一些新的public方法使得这个子类一旦被使用这意味着它可能直接和软件的其他部分产生耦合
  - <span style="color:red">子类不应该是作为父类功能的一个扩展</span>
    - 如果说子类要扩展父类的功能，<span style="color:darkorange">完全可以使用组合的方式来实现</span> -- 这不完全绝对
  - 里氏代换原则运用的时候，<span style="color:red">尽量把父类设计成一些它的整个接口的实现，然后子类来继承父类中定义的接口并且给出一种具体的实现</span>

- 实例 ：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 11.39.18 PM.png" alt="Screen Shot 2021-03-02 at 11.39.18 PM" style="zoom:30%;" />

  - 重构之后 ：

     <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 11.41.32 PM.png" alt="Screen Shot 2021-03-02 at 11.41.32 PM" style="zoom:30%;" />
    - 重要的是根据里氏代换原则，所有能够接受CipherA类对象的地方都可以接受CipherB类的对象。

    - 因此，我们可以简化DataOperator里，Client里的代码。

    - 如果需要添加新的CipherC类，作为CipherA类的子类，甚至你把它作为CipherB类的子类都可以。因为它都是可以替换



##### 4. 依赖倒转原则

- Dependence Inversion Principle, DIP

- 如果说开闭原则是面向对象设计的目标，那么依赖倒转原则是实现面向对象设计的主要的机制

- 高层模块不应该依赖低层模块，它们都应该依赖抽象 。抽象不应该依赖于细节，细节应该依赖于抽象

   <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-02 at 11.56.10 PM.png" alt="Screen Shot 2021-03-02 at 11.56.10 PM" style="zoom:70%;" />

  - 强调的是高层和细节都依赖于抽象
  - 为啥是这样的？👉 我们要实现开闭原则

- 实例：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-03 at 12.07.39 AM.png" alt="Screen Shot 2021-03-03 at 12.07.39 AM" style="zoom:30%;" />

  - 重构之后 ：

    <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-03 at 12.09.04 AM.png" alt="Screen Shot 2021-03-03 at 12.09.04 AM" style="zoom:50%;" />

    - MainClass : 最高的
      - Source, Transformer : 细节

      - 中间引入了抽象层

      - 如果要进行新的扩展，都可以增加新的类，修改配置文件部分就可以了

      - 我们的新的系统完全符合开闭原则的要求



##### 5. 接口隔离原则

- Interface Segregation Principle, ISP

- 客户端不应该依赖那些它不需要的接口

- 一旦一个接口太大，则需要将它分割成一些更细小的接口，使用该接口的客户端仅需知道与之相关的方法就可以

- 什么是接口？

  - 一个接口就只代表一个角色
    - 接口划分成更小的接口，实际上就直接带来了类型的划分
  - 接口仅仅提供客户端需要的行为

- 首先必须满足 “单一职责原则”

  - 相关的操作定义在一个接口 👉 那这样的话，所有的接口都变的很小
  - 每个接口就有一个方法，这不是分给的好吗？👉 当然不是，这样的话会出现“接口爆炸现象”

- “接口隔离原则”是为“依赖倒转原则”做一个进一步的说明

  - 我们强调的是面向抽象（接口）编程
  - 但是这个接口要怎么做？👉 它应该为不同的客户端提供宽窄不同的接口

- 实例 ：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 9.31.08 PM.png" alt="Screen Shot 2021-03-04 at 9.31.08 PM" style="zoom:50%;" />

  - ClientA类除了能够看到方法operatorA()以外还能看到它不相关的方法operatorB()， operatorC()

  - 这个时候出现的问题？👉 影响系统的封装性 ：我们不能限制ClientA去使用operatorB()，operatorC()

  - 重构之后 ：

     <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 9.36.25 PM.png" alt="Screen Shot 2021-03-04 at 9.36.25 PM" style="zoom:20%;" />



##### 6. 合成复用原则

- Composite Reuse Principle, CRP

- 啥是合成关系？

  - 组合和聚合关系

- 组合和聚合有什么差别？（那一种关系对于合成对象和被合成对象依赖性更强呢？）

  - 组合什么里面被组合的对象往往是只属于一个组合对象，依赖性更强。同时，被组合对象的类型往往是不同（比如说，一个汽车轮胎只能属于一个组合体）
  - 聚合更弱的关系。一个被聚合的对象可以同属于多个聚合对象。被聚合对象往往类型是相同。

- UML当中组合和聚合都是三角箭头（哪个是空心的？哪个是实心的？）

  - 三角箭头空心 - 聚合 （弱）
  - 三角箭头实心 - 组合（强）

- 合成复用原则 ： <span style="color:red">尽量使用对象组合</span>，而不是继承来达到复用的目的

- <span style="color:red">要尽量使用组合/ 聚合关系，少用继承</span>。

  - 为啥？

    - 通过继承复用 ：实现简单，易于扩展。但是破坏系统的封装性 ，只能在有限的环境中使用

      - 白箱复用 = 代码的复制 ：

        改变一个需求的时候，如果这个改变要在父类中进行，子类中进行，那么所有的派生类都会被影响

    - 通过组合/ 聚合复用 ：

      - 耦合度相对较低 ：

        因为成员对象的变化对新对象的影响不大的。 一个方法改变了之后，我只要保证它的方法的接口没有变化。原来返回什么值，这个值需要满足什么条件，这个没有变化。那么我的实现细节的变化并不影响调用关系。

      - 可以在运行时动态进行

- 那么继承有什么用呢？
  - 继承希望复用的是“接口定义”，合成希望复用的是“接口实现”
  - 父类中间我们确认不会有变化的部分，我们才会通过子类去进行继承。而继承的时候，我们要严格遵循“里氏代换”的原则

- 实例 ：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 10.21.50 PM.png" alt="Screen Shot 2021-03-04 at 10.21.50 PM" style="zoom:40%;" />

  - 重构之后 ：

    <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 10.27.01 PM.png" alt="Screen Shot 2021-03-04 at 10.27.01 PM" style="zoom:50%;" />

    - 声明了抽象的DBUtil类 -- 依赖倒转原则



##### 7. 迪米特法则 , 最少知识原则

- Least Knowledge Principle, LKP

- 只与你的直接朋友通信

- 最需要了解的是“什么是你的直接朋友” ：

   <img src="/assets/images/软设/myNote/pic01/Untitled.png" alt="Untitled" style="zoom:30%;" />

- 好处 ：

  - 狭义的法则 & 广义的法则

    - 狭义的迪米特法则

       <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 10.37.36 PM.png" alt="Screen Shot 2021-03-04 at 10.37.36 PM" style="zoom:30%;" />

      A对象 & B对象 ： 依赖关系

      C对象 ： B对象的成员对象

      A对象只能调用B对象的方法，而不允许调用C对象中间的方法

      也就是说 ： 不允许出现比如说 a.method1().method2() 或者 a.b.method2() [只能够出现一个点号]

      好处 👉 降低类之间的耦合

      坏处 👉 造成系统的不同模块之间的通信效率降低

    -  广义的迪米特法则

      主要是对信息隐藏的控制

- "程序设计的过程"中间"狭义的迪米特法则"非常有用的

- "软件的模块的设计"中间"广义的迪米特法则"更为重要的

- 迪米特法则的主要用途在于控制信息的过载

- 实例 ：

  <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 11.13.29 PM.png" alt="Screen Shot 2021-03-04 at 11.13.29 PM" style="zoom:50%;" />

  - 重构之后 ：

    <img src="/assets/images/软设/myNote/pic01/Screen Shot 2021-03-04 at 11.16.32 PM.png" alt="Screen Shot 2021-03-04 at 11.16.32 PM" style="zoom:40%;" />

    - 由于控制类的引入，界面类和数据访问类之间不存在直接引用关系
    - 更好的灵活性
    - 控制类起到了Wrapper的作用



#### 思考🤔

- 在JDK中，java.util.Stack是 java.util.Vector类的子类，该设计合理吗？若不合理，请分析解释该设计存在的问题
  - 不合理
    - 违反里氏代换原则 ： Stack类它有自己的额外的操作
    - 违反合成复用原理 ： 复用的过程中出现问题。继承是相当于复用所有的接口，不合理。





---
