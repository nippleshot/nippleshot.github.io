---
layout: article
title: 05. Defensive Programming
author: J_宋
tags: SoftwareSystemDesign 中文 한글
mathjax: true
key: systemDesign05
---



我们在编写程序的时候，经常会做出各种各样的假设来简化我们的问题。

- 这段代码肯定会一直正常运行，传递给我的参数总是有效的 等



什么是防御式编程？

- **通过预见到问题所在，做出相应的防范措施来防止类似意外的发生**
- **主要思想 : 要承认程序都会有问题，都会被修改**



防御式编程 🆚 检查错误

- 防御性编程并不能排除所有的程序错误



防御式编程 🆚 调试

- 防御式编程是一种防卫方式，而不是补救（고치고 보완하는）方式



防御式编程 🆚 测试

- 测试可以验证代码现在是正确的，但不保证在经历修改之后不会出错



处理非法输入数据

- Garbage in（外部来的值）, Garbage out 很不负责任的说法
  - 我们希望的是Garbage in, No Garbage out

1. 检查所有来源于外部的数据值
2. 检查子程序所有输入参数值
3. 决定如何处理错误的输入数据



断言 -- Assertions

- **主要用于开发和维护阶段**
- 让程序在运行时进行自检的代码
- 可以看成是一个可执行的注释

- Assertions有两个部分

  1. 用Bool表达式来判断情况
  2. 显示的信息（不是必须的）

   <img src="/assets/images/软设/myNote/pic_06/Screen Shot 2021-06-27 at 6.21.11 PM.png" alt="Screen Shot 2021-06-27 at 6.21.11 PM" style="zoom:33%;" />

- 用途 ：

  - Input & Output Parameter值处于预期的范围内

  - 指针非空

    ...

- 生成产品代码时并不编译进去，因为

  1. 不希望用户看到产品代码中的Assertions信息
  2. Assertions会降低系统性能

- 错误处理代码 🆚 Assertions
  - 错误处理代码 ： 处理预期会发生的非正常情况
  - Assertions ： 检查永远不该发生的情况
- 用Assertions来注解并验证前/后置条件
  - Public方法而言用错误处理
  - Private方法而言用Assertions
  - 不变式 invariants : 永远都应该为真的条件

- 例子

   <img src="/assets/images/软设/myNote/pic_06/Screen Shot 2021-06-27 at 6.47.25 PM.png" alt="Screen Shot 2021-06-27 at 6.47.25 PM" style="zoom:33%;" />

  - assert i == 10; 💁🏻 若 i != 10, 意味着这个for循环运行过程中间出现了问题



错误(Fault)处理技术

- 错误 ： 用户错误，程序员错误，意外情况 ...

- 技术

  1. 返回中立值

  2. 换用下一个正确的数据（在处理数据流【网上视频】的时候）

  3. 返回与上一次相同的数据

  4. 把警告信息记录到日志文件中

  5. 关闭程序

     ....

- 人身安全攸关的软件 - 正确性(correctness)是最重要的特征

  - 永不返回不准确的结果，不返回结果比返回不准确的结果好

- 消费类应用软件 - 健壮性(robustness)

  - 以保证软件可以持续地运转下去，哪怕有时做出一些不够准确的结果

- 实例

  <img src="/assets/images/软设/myNote/pic_06/Screen Shot 2021-06-27 at 7.12.52 PM.png" alt="Screen Shot 2021-06-27 at 7.12.52 PM" style="zoom:33%;" />

  1. 按照正常的工作流作为错误处理 
     - 优点：控制流很清晰
     - 缺点：可维护性比较差
  2. 通过引入多个控制变量来进行
     - 缺点 ：需要明显的控制变量
  3. 清晰，但是很多情况下发生了错误之后不直接return,而做其他的工作
  4. 没什么，，
  5. 异常的方式
     - 缺点：异常的情况不明显



异常

- C++支持try-catch但是不支持try-catch-finally，为啥呢？

  - finally的作用是啥？：在发生异常的时候，确保仍然可以执行清理工作
    - ex) 网络连接打开了之后你一定要关闭， 不能因为发生异常就不关闭
  - C++提供了资源获得及初始化

- Trace a Java program exection

   <img src="/assets/images/软设/myNote/pic_06/Screen Shot 2021-06-27 at 9.07.54 PM.png" alt="Screen Shot 2021-06-27 at 9.07.54 PM" style="zoom:33%;" />

  

- 在其他编码实践方法无法解决的情况下才使用异常
- 在恰当的抽象层次抛出异常
  - 确保异常的抽象层次与子程序接口的抽象层次是一致的



隔栏 -- Barricade

- 以防御式编程为目的而进行隔离的一种方法
- 系统分成两个部分
  - “安全”部分，“不安全”部分
- Barricade 🆚 Assertions
  - 隔栏的使用使断言和错误处理有了清晰的区分
  - **隔栏部分包含了“脏数据”, 通过隔离部分之后的是“干净数据”**
    - **隔栏外部的程序应使用错误处理技术**
    - **隔栏内部的程序就应使用断言技术**
      - 隔栏内子程序出现错误数据，就是程序里的问题



辅助调试的代码

- 进攻式编程 -- Offensive Programming
  - 主动暴露可能出现错误的态度

- 辅助调试的代码会使软件的体积变得庞大且速度变慢

  - 辅助调试的代码를 삭제하는 방법

    1. 预处理器 

       ```c++
       #define DEBUG
       ...
       #if defined(DEBUG)
         // debugging code
       #endif
       ```

       

    2. Debugging stubs 

       ```c++
       void DoSomething(SOME_TYPE *pointer){
         ...
         CheckPointer(pointer); // pointer를 검사하는 sub-program 호출하기
         ...
       }
       
       // 만약 개발단계 일 경우
       void CheckPointer(void *pointer){
         // 执行第1项检查—可能是检查它不为NULL
         // 执行第2项检查—可能是检查它的地址是合法的
         // 等等...
       }
       
       // 만약 상품화 된 코드일 경우
       void CheckPointer(void *pointer){
         // no code, just return to caller
       }
         
       ```



상품화된 코드에 防御式代码를 얼마나 보류해야되나?

- 개발 단계에는 가능한 많으면 좋다
- 하지만 배포 단계에는 코드 실행에 지장을 주지 않도록 줄여야함



어떻게 防御式编程를 대해야되나?

- 과도한 防御式编程은 문제를 야기할 수 있다
- 그럼으로 어디에 防御를 할 지 잘 고려하고 防御式编程의 우선순위를 정한다



#### 练习

 <img src="/assets/images/软设/myNote/pic_06/Screen Shot 2021-06-27 at 10.16.46 PM.png" alt="Screen Shot 2021-06-27 at 10.16.46 PM" style="zoom:40%;" />





---

《Reference》

1. 2021年(春) 软件设计系统 : 潘敏学



***

