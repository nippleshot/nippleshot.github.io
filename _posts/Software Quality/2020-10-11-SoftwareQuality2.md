---
layout: article
title: 02. Historical Evolution of Software Process and Classic Work
author: J_宋
tags: SoftwareQuality 中文
mathjax: true
key: softwareQuality02
---



#### 软件发展三大阶段  

##### 50年代~70年代 ： 软硬件一体化

- 软件应用的特征
  - 软件的存在目的只是完成计算用
  - 唯一的功能 ： 辅助计算
  - 不会复杂，只是一个计算器，所以复杂度有限的 
  - 需求变更几乎不用考虑
  - *60年代 ：软件作坊*
    - 某些硬件当中类似于操作系统或者软件使得可以有相对标准化的方式去开发软件
    - 功能简单，规模小（比如说电子表格）
- 软件开发的特征
  - 当时硬件非常昂贵的资源，公司没有能力去购买计算机，所以租用了
  - 团队以硬件工程师和数学家为主，当时的做法一定是反复演算
  - *60年代 ：软件作坊*
    - 软件或者计算机不是仅仅能用来做科学计算。它是可以在商业活动里面使用。从而很多非专业领域的人员涌入了软件开发里面
    - 伴随着高级程序语言的出现，门口进一步的降低
- 经典软件过程
  - "Measure twice, cut once” ：硬件成本很高，所以硬件工程师和数学家反复了去演算，演算没有问题了，然后租计算机跑结果
  - “Code and fix” ：60年代末，开始关注操作系统级别的项目开发，然后发现编码改错不行了

- 开发方法
  - 硬件开发 ： 有一大堆的规格说明，就是有循序的过程。每一个阶段都定出了机器严格的规格要求



##### 70年代～90年代 ： 软件成为独立的产品

- 软件应用的特征
  - (因为操作系统的出现)软件摆脱了硬件束缚。不用针对特定的硬件去软件开发
  - 因为软件摆脱了硬件，软件的功能越来越强大
  - 伴随着软件功的强大，规模和复杂度剧增
  - 80年代开始，个人电脑出现了。所以普通人成为软件用户。用户的数量增加了。需求也多变
  - 因为软件成为独立的产品，出现有些公司卖软件。
- 软件开发的特征
  - 80年代 ：面向对象编程技术（对结构化程序设计的很大的推动）
  - 70年代，80年代重要的成功：成熟度模型
- 经典软件过程
  - Royce瀑布模型，成熟度模型
- 开发方法
  - 形式化方法 ：一个可以支持极高质量开发方法
  - 结构化程序设计：系统开发技术角度来讲，分枝化，自顶向下
  - 还是存在的问题
    - 扩展性，可用性方面存在明显的不足
    - 瀑布模型成为一个重文档，慢节奏的过程



##### 90年代～今天 ：网络化和服务化

- 软件应用的特征
  - 功能更复杂，规模更大
  - 用户数量急剧增加 -- 移动计算，手机迅速增加
  - 快速演化和需求不确定
  - 分发方式的变化
- 软件开发的特征
  - 因为有了虚拟化技术之后，使得服务器，硬件不用太多考虑的
  -  盛行共享和开源
- 经典软件过程
  - 敏捷方法（XP，SCRUM，Kanban）
- 开发方法
  - 开源软件开发方法, Inner source, Crowdsourcing



#### 考题分析

##### Q1 : 为什么Royce瀑布模型不适合软件开发？

<img src="/assets/images/软质/myNote/pic02/Picture1.png" alt="Picture1" style="zoom:80%;" />

- Royce文章中的3个观点：

1. 瀑布模型不是一个单元的模型，是一个实例（最简单的，最复杂的都有）

2. 项目团队应该根据项目的困难，挑战等等综合考虑之后选择一个合适的瀑布模型

3. 大部分的项目团队都是过低的估算的项目难度。从而选择了过于简单的瀑布模型



##### Q2 : 请描述CMMI模型的5个等级的特征

| 等级                       | 特征                                                         |
| -------------------------- | ------------------------------------------------------------ |
| 1级 Initial                | - 开发相对混乱，依赖个人英雄主义，没有过程概念               |
| 2级 Managed                | - 项目小组级体现着项目管理的特征，有项目计划和跟踪等等       |
| 3级 Defined                | - 小组可以根据自己的项目的特点，在公司的标准化的流程基础之上定制使用开发过程 |
| 4级 Quantitatively Managed | - 构建预测模型，关注重点在于过程的度量和控制<br/>- 尽可能很小的范围之内活动，从而提升对未来预测的能力 |
| 5级 Optimizing             | - 继续应用统计方法识别过程偏差，找到问题根源并消除，避免未来继续发生类似问题 |

- ***3级一下就相当于我只看当前***

- ***4级往上我就需要根据未来做预测，决策***



##### Q3 : CMMI是过程改进模型而非软件过程或者软件过程模型 ?

- CMM是认为一种过程改进的参考模型，不是具体的软件过程的模版或模型。
- CMMI是流水线的改造的。整个模型里面从来没有说哪一个步骤先做，哪一个步骤后做。
- 所以说CMMI是过程改进模型，不是具体的软件过程模型。



##### Q4 : CMMI不是过程优劣的标准，也不适合用作公司之间的能力比较

- CMM或者CMMI等级仅仅表示这家公司或者这个软件组织在实现它自己的商业目标上的能力。
- 如果两个公司，它的商业目标和业务目标是一样的，这个时候可以根据等级来判断能力的强弱。
- 所以这个是相对概念。CMMI是不同的公司来说含义是不一样。



##### Q5 : 为什么CMMI跟Agile是一个伪命题

- 敏捷是属于软件过程或者软件开发方法。
- CMMI不是软件过程，他不是具体的软件开发方法。





***

《Reference》

1. 2020年(秋) 软件质量与管理 : 荣国平



***

