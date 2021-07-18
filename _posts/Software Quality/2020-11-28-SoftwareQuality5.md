---
layout: article
title: 05. Quality Management
author: J_宋
tags: SoftwareQuality 中文
mathjax: true
key: softwareQuality05
---



##### 什么叫“质量”？ 

- 软件质量为“与软件产品满足规定的和隐含的需求能力有关的特征或者特性的全体”
- 软件质量为内外两部分的特性：
  1. 外部质量 ：用户能够感受到的
  2. 内部质量：程序员能够感受到的



##### 面向用户的质量观

- PSP中也采用了面向用户的视图，定义质量为满足用户需求的程度
- 在这个定义中，就需要进一步明确
  - 用户究竟是谁？
  - 用户需求的优先级是什么？
  - 这种用户的优先级对软件产品的开发过程产生什么样的影响？
  - 怎样来度量这种质量观下的质量水平？



##### 为什么在讨论软件质量的时候，用”缺陷”这个词，而不用”错误(Error,Fault,Bug)”?

- Bug都是指的是已知的错误
- Defect指的是未知的错误



##### 不同的方式在消除缺陷方面的效率

- 评审发现错误比测试更快 – 是因为测试的流程比评审的流程更长一点
- 尽可能用一些早起的个人Review, 团队Inspect的手段能够早起消除缺陷。这样一来，使得后面的测试发现的错误数量变小了



##### PSP质量管理策略

- 优先级排第一个是**功能正确**。功能正确跟缺陷有关

  1. 所以用“缺陷管理”去替代“质量管理”(从缺陷的视角的质量管理)
  2. 高质量产品 == 要求组成软件产品的各个组件基本无缺陷
  3. 各个组件的高质量是通过高质量评审来实现的

  

##### 那么评审怎么来做？

- 评审检查表

  - 一个个性化的检查表, 通过评审检查表，可以让程序员以系统的方式来查找错误

- 个性化的可以定制的评审检查表来自于哪里？

  - 缺陷日志 (当中最重要的3个信息：注入阶段，消除阶段，缺陷的根本原因描述)

- 质量控制指标

  - Yield

    - 用以度量每个阶段在消除缺陷方面的效率

      <img src="/assets/images/软质/myNote/pic05/Picture1.png" alt="Picture1" style="zoom:38%;" />

    - **Yield表示阶段消除能力，这种能力只能预估，没法度量**

    - Phase Yield :

      <img src="/assets/images/软质/myNote/pic05/Screen Shot 2021-07-18 at 4.14.09 PM.png" alt="Screen Shot 2021-07-18 at 4.14.09 PM" style="zoom: 33%;" />

    - Process Yield :

      <img src="/assets/images/软质/myNote/pic05/Screen Shot 2021-07-18 at 4.14.55 PM.png" alt="Screen Shot 2021-07-18 at 4.14.55 PM" style="zoom: 33%;" />

    - 这个指标没法计算的。因为”进入该阶段前遗留的缺陷个数”跟“第一次编译前注入的缺陷个数”不知道

      👉 这个意义上来说，这两个指标只能**估算**。

  - A/FR (质检失效比)

    <img src="/assets/images/软质/myNote/pic05/Picture11.png" alt="Picture11" style="zoom:38%;" />

    - 质检失效比 : <span style="color:blue">PSP质检成本</span> / <span style="color:red">PSP失效成</span>

      - <span style="color:blue">质检成本</span> == 设计评审时间 + 代码评审时间

      - <span style="color:red">失效成本</span> == 编译时间 + 单元测试时间

    - A/FR的期望值 == 2.0

    - A/FR的值越大，往往意味着越高的质量。但是它的问题在于达到了2.0之后再进步了增加时间去Review，不会创造新的价值

      👉 太多的评审，没那么必要，浪费。

    - A/FR的作用 :

      1. A/FR可以度量出来的，不是估算值。所以可以用来帮助我们去理解而判断模块质量的状态
      2. A/FR的指标帮助我们去设立一个很明确的质量的指标或者质量的目标。所以帮助我们去做质量计划

  - PQI (Process Quality Index)

    - <span style="color:brown">过程度量</span>跟<span style="color:purple">结果度量</span>的一个混合体

    - 5个数据乘积 (每个数据都是0~1之间的，结果也是0~1之间的)

      1. <span style="color:brown">设计质量： Design > Code </span>
      2. <span style="color:brown">设计评审质量：Design Review > Design * 50% </span>
      3. <span style="color:brown">代码评审质量：Code Review > Code * 50% </span>
      4. <span style="color:purple">代码质量：Compile缺陷密度 < 10个/千行 </span>
      5. <span style="color:purple">程序质量：Unit Test缺陷密度 < 5个/千行 </span>

    - 下面的图意味着PQI越高，质量越高，缺陷密度越小

      <img src="/assets/images/软质/myNote/pic05/Picture21.png" alt="Picture21" style="zoom:30%;" />

      - PQI > 0.4：质量很高

    - PQI的作用

      1. 根据PQI指标可以判断质量的状况
      2. 因为PQI包含了过程质量，支持我们做质量计划
      3. 为过程改进提供依据 (项目经理对组员建议改进的时候，可以告诉那些维度特别低)

  - 评审的速度 (Review Rate)

    - 用以指导软件工程师开展有效评审的指标
    - 代码评审速度不要超过一个小时200行
    - 文档评审不要超过一个小时4页

  - DRL (Defect Removal Leverage)

    - 不同缺陷消除手段消除缺陷的效率
    - 希望看的是DRL都是大于1 (== 单元测试消除缺陷效率是最差的)

- 其他考虑因素

  - 建议如果有条件，尽可能打印出来评审。脱离原来工作环境

  - 评审时机选择 ：（前提：评审的目标 -- 把所以的错误改掉 ）我们尽可能花小的时间，要消除多个错误

    - 编译之前 - 评审发现错误都改完之后，编译
    - 编译之后 - 语法错误改掉，然后做评审
    - 消除缺陷的能力几乎一定的没有差异的情况下，先评审后编译会更省时间。这个结论的前提是评审的目的（就是找到所有的错误）

  - 个人评审（Review）和小组评审（Inspection） 

    - 个人评审相对不那么正式的
    - 小组评审帮助个人发现不了的问题
    - 一定是先认真个人评审，然后做小组评审

  - 九宫图

    <img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-03 at 10.04.02 PM.png" alt="Screen Shot 2020-12-03 at 10.04.02 PM" style="zoom: 67%;" />

    - 缺陷 ：假设了这一份文档质量跟之前是一样的。这意味着评审对象（软件）质量必须是跟之前的一样这才能成立

  - Capture & Recapture方法 

    <img src="/assets/images/软质/myNote/pic05/Screen Shot 2021-07-18 at 4.42.47 PM.png" alt="Screen Shot 2021-07-18 at 4.42.47 PM" style="zoom:35%;" />

    - 好处 ：不要历史数据，所以实际工作当中更容易被使用 

    - ⚠️ 注意 

      - 实际情况当中，3个人以上的人来评审。那么把“独立发现错误最多的人--他发现的错误当中别人都没发现的”作为A。剩下的人作为整体作为B

      

##### 设计与质量的关系

- 充分设计可以显著减少最终程序的规模，提升质量。

- 设计本身也是一种排错的过程

- PSP设计过程

  <img src="/assets/images/软质/myNote/pic05/Screen Shot 2021-07-18 at 4.46.06 PM.png" alt="Screen Shot 2021-07-18 at 4.46.06 PM" style="zoom:35%;" />

- 设计的时候应该考虑的内容

  <img src="/assets/images/软质/myNote/pic05/Screen Shot 2021-07-18 at 4.47.11 PM.png" alt="Screen Shot 2021-07-18 at 4.47.11 PM" style="zoom:45%;" />

  1. 通过两个维度，他给出是一个软件开发当中设计你应该要包含内容的最小集
  2. 这支持可评审设计（评审者根据这些信息就可以明确地下这个结论--这个设计是合理不合理）

- PSP设计模版

  <img src="/assets/images/软质/myNote/pic05/Screen Shot 2021-07-18 at 4.49.38 PM.png" alt="Screen Shot 2021-07-18 at 4.49.38 PM" style="zoom:45%;" />



##### UML 🆚 PSP设计模版

| UML                                                    | PSP                                            |
| ------------------------------------------------------ | ---------------------------------------------- |
| 用例图，时序图 ：提供的信息                            | OST                                            |
| 时序图，类图 ：描述了类之间的关系 & 对象之间的交互信息 | ❌                                              |
| 类图：描述了方法的型构 (没描述了方法的行为)            | FST                                            |
| ❌                                                      | LST用以描述程序的静态逻辑                      |
| 状态图 ： (❌)                                          | SST (描述状态，状态转换条件，状态转换中的动作) |



##### 设计的层次

<img src="/assets/images/软质/myNote/pic05/Picture31.png" alt="Picture31" style="zoom:38%;" />

- 系统需求定义 ： 这个系统解决什么样问题，它要干什么
- 需求规格说明：这个系统解决方案是啥，它怎么来解决问题的
- 注意点1 ：自顶向下组成的过程，思想
- 注意点2 ：每一次都很相似，所以每一层都应该用可评审的设计



##### 设计验证方法

- 对于设计，这个难度会比较高，不是简单的说“我只要阅读一下我就发现设计的问题”

- 设计验证方法可以帮助设计文档的时候，可以找出整个系统当中的可能的设计错误

- 方法

  - 状态机验证

    - 什么是正确的状态机？

      评价状态机设计是否正确，其实主要是看转换条件 

      转换条件有两个要求：完整跟正交

      - 完整 ：所有的条件组合都被覆盖到了
      - 正交 ：在同样的条件组合之下，下一个状态必须唯一的

    - 验证方法 ：3个步骤

      1. 检验状态机，消除死循环和陷阱状态。

      2. 检查状态转换，验证完整性和正交性。

      3. 评价状态机，检验是否体现设计意图。

    - 陷阱 ：某一个状态不能到End，某一个状态没有路径

    - 例子 ：

      <img src="/assets/images/软质/myNote/pic05/Picture41.png" alt="Picture41" style="zoom:38%;" />

      :diamonds: /n : 最大允许尝试的次数

      :diamonds: 为了安全的考虑 ：不会告诉ID错误或者PW错误

      :diamonds: 这个状态机的设计是对不对？

       - 所有的状态都是可以End (不存在陷阱)

       - CheckID ➡️ CheckPW 这里有一个条件（n < nMax）所以这不是死循环♻️

       - 设计意图也是没有太大的问题，我们认为OK吧

       - CheckPW的出发我们可以发现3条路（转换3个转换条件都是复杂的） :

         <img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-04 at 12.33.12 PM.png" alt="Screen Shot 2020-12-04 at 12.33.12 PM" style="zoom:33%;" />

         <img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-04 at 12.41.21 PM.png" alt="Screen Shot 2020-12-04 at 12.41.21 PM" style="zoom:50%;" />

         -- 复杂的条件组合下我们才使用“真值表”，普通的情况下不使用

         -- n（3种）, Password（2种）, ID（2种）： 总共有12种可能性

         -- CID 表示CheckID, F表示False, T表示True

         -- 我们根据真值表，这个设计对不对？ (答案 : 设计是错误的)

         👉 完整的角度来看

         ​	“下一个状态都有定义的”说明符合完整性要求

         ​	<img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-04 at 12.59.50 PM.png" alt="Screen Shot 2020-12-04 at 12.59.50 PM" style="zoom:50%;" />	

         ​	所以这个设计满足了完整性

         👉 正交的角度来看

         ​	“下一个状态是唯一的”说明符合正交性要求

         ​	<img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-04 at 1.04.01 PM.png" alt="Screen Shot 2020-12-04 at 1.04.01 PM" style="zoom:50%;" />

         ​	其他情况都是没有问题的

         ​	我们看一下 n=nMax, Password=Valid, ID=Valid和 n>nMax, Password=Valid, ID=Valid的情况 ：

         ​		尽管都是End其实描述的是两个不同的现象 👈 这不是“唯一”的

         ​	所以这个正交出问题了

  - 符号化执行验证

    - 将描述设计的伪代码程序用代数符号来表示，然后系统地开展分析和验证

    - “理解这个设计”是你可以去发现错误的前提

    - 一片段伪码的设计例子 ：很清楚地知道伪码干的事情（X，Y交换）

      <img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-04 at 1.29.08 PM.png" alt="Screen Shot 2020-12-04 at 1.29.08 PM" style="zoom:50%;" />

      <img src="/assets/images/软质/myNote/pic05/Screen Shot 2020-12-04 at 1.29.34 PM.png" alt="Screen Shot 2020-12-04 at 1.29.34 PM" style="zoom:50%;" />

    - 好处 ：概念上特别简单，给出的结论是可靠的

    - 坏处 ：不适合在复杂逻辑结构

  - 执行表验证

    - 执行表用一种有序的方法来跟踪伪码程序的执行状况，分析程序行为，从而验证设计

  - 跟踪表验证

    - 对执行表验证方法的一种扩充
    - 提供更加高效地开展验证工作
    - 差别点
      - 识别将伪码程序符号化的机会，并加以符号化
      - 定义并且优化用例组合

  - 正确性检验

    - 伪码程序当成数学定理，采用形式化方法加以推理和验证

    - 例子 ：

      ```pseudocode
      while(condition)
      	begin
      		states;
      	end
      ```

      一个正确的While循环设计应当满足如下条件 ：

      条件1： condition是否最终一定会为“假”，从而使得循环可以结束；

      条件2： condition为“真”的时候，单独的循环结构执行结果与循环体再加一个循环结构，其执行结果是否一致？

       - 就是下面两个执行的结果执行的结果必须是一样的

         ```pseudocode
         while(condition){
         	states;
         }
         ```

         ```pseudocode
         while(condition){
         	states;
         	while(condition){
         		states;
         	}
         }
         ```

      条件3： condition为“假”的时候，循环体内所有变量是否未被修改？

      

