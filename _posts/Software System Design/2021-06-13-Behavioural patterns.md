---
layout: article
title: 03. Behavioural patterns
author: J_宋
tags: SoftwareSystemDesign 中文 한글
mathjax: true
key: systemDesign03
---

***

### 复习

可复用部分 == 不变的部分

从设计模式而言，更清晰的区分了变化的部分和不变的部分

디자인 원칙을 떠올려보자. ***변하는 것은 잘 변하지 않는 것과 분리해라. 즉, 변하는 녀석들을 캡슐화해라!*** 

工厂方法 ：

- 不变的 - 抽象的Product类型（Product的基类）
- 变化的 - 具体的Product类型

抽象工厂：

- 不变的 - 抽象的Product类型，抽象的Product类型之间的挂链关系
- 变化的 - 产品族

建造者 ：

- 不变的 - 构建过程，抽象的Builder，Director
- 变化的 - Product的表示，具体的Builder

***



### 策略模式 (Strategy Pattern)

- 策略模式的别名 ：Policy

- 模式解决的问题
  - 算法组 ：意味着算法实现的任务是相同的，只是说这个算法在实现上有不同的考虑。可能是为了空间和时间的复杂度不同。可能是算法的表现，比如说有一些额外的任务和不同的行为。但是它总体的目标是一样。
  - Interchangeable : 可互换（客户使用的时候，这些算法独立变化）
    - 意味着必然是有抽象，必然是合成
    - 策略模式的一个限制 ： 你要去互换这个模式，必须将这个实现细节暴露给客户

- 模式大概是什么样的结构

    <img src="/assets/images/软设/myNote/pic_02/Screen Shot 2021-05-24 at 8.51.03 PM.png" alt="Screen Shot 2021-05-24 at 8.51.03 PM" style="zoom:33%;" />

- 动机

    <img src="/assets/images/软设/myNote/pic_02/Screen Shot 2021-05-24 at 8.58.19 PM.png" alt="Screen Shot 2021-05-24 at 8.58.19 PM" style="zoom: 35%;" />

  - 过多的条件判断 -- 在代码中要回避的，因为 ：
    - 过多的条件语句本身其实没有提供太多的信息。它就是不断的处理特殊情况
    - 维护性上非常差，不符合开闭原则

- 应用性

  - 什么时候来使用策略模式？
    - 我有不同的行为，我要复用这些行为
    - 保护数据结构
    - 将不同的条件分枝组装成不同的策略类

- 优点

  - 提高复用性

  - 不需要多个if语句

- 缺点

  - 客户必须了解不同的策略
    - 状态模式跟策略模式很像。但是
      - 状态模式：有模式内部自身来完成状态切换
      - 策略模式：策略的切换是必须客户来完成
  - 你不得不将你已经分装好的类用详细的说明告诉对方，这样才可以



### 状态模式 (State Pattern) 

- 一个对象的行为取决于一个或多个动态变化的属性，这样的属性叫做状态

- 어떤 형광등 스위치가 있다고하자 :

  - 현재 불이 꺼진 상태에서 onButton() 동작하면 불이 켜지게하는 행위를 해야되고
  - 현재 불이 켜져있는 상태에서 onButton() 동작하면 불이 수면모드로 변할 수 있게 행위를 해야됨
  - 즉 같은 onButton() 를 동작시킨다 해도 현재 상황에 따라 시행되는 행위가 다름

- 分支(if语句)来实现的话，不符合“开闭原则”，可理解性很差

- 目标 ： 1.取代分支(if条件语句) 2. 希望能够让这个变化被隐藏起来

   <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-01 at 11.04.12 PM.png" alt="Screen Shot 2021-06-01 at 11.04.12 PM" style="zoom:30%;" />

  - 有变的方法 ：处理的方法(Handle) -- 单独封装成这些类
  - 然后封装之后，抽象出一个接口
  - 最后，我们用合成的方法形成一个这样的图

- 主要的部分 ： Context类拥有State类

- **跟“策略模式”的差别** ：

  - 用法上是一样

    - 都是通过set方法替换具体的子类

  - 策略模式的缺点 ： 必须让客户了解策略之后，才能调用Set方法

  - 策略模式的缺点正好和状态模式相反

    - **进行一个透明的处理** : 为了达到透明性，set方法（状态的转换）封装在模式的内部。

  - 两个模型的目的不同

    - 策略模式 - 为了提供恰当的策略（每一个具体的Context提供一个恰当的策略）

    - 状态模式 - 为了在内的变化过程中能够呈现不同的行为

    - 策略模式 - 사용자가 쉽게 알고리즘 전략을 바꿀 수 있도록 유연성을 제공. 상속의 한계를 해결하기 위하여 나온 패턴. 

      ```java
      //Strategy 패턴을 떠올려보면 클라이언트 쪽에서 알고리즘을 변경하기 위하여 setter를 호출해 직접 수행할 알고리즘을 주입해주었던 것을 기억할 것이다. 즉, 클라이언트가 구체적인 알고리즘의 수행까지는 몰라도 어느정도 무엇무엇이 있는지 정도는 알고 있어야 한다는 것이다.
      public class DuckMain {
          public static void main(String[] args) {
              Duck a = new ADuck();
              a.setFly(new NotFlyBehavior());
              a.setQuack(new NotQuackBehavior());
              Duck b = new BDuck();
              b.setFly(new DoFlyBehavior());
              b.setQuack(new DoQuackBehavior());
            
              System.out.println("===========ADuck===========");
              a.swim();
              a.fly();
              a.quack();        
              System.out.println("===========BDuck===========");
              b.swim();
              b.fly();
              b.quack();
          }
      }
      ```

      状态模式 - 한 객체가 동일한 동작을 상태에 따라 다르게 수행해야 할 경우 사용하는 패턴

      ```java
      // 하지만 State 패턴은 각 상태 구현 클래스들이 자신들의 행위를 수행하면서 직접 Context(Light)객체의 상태를 변경해주기 때문에 클라이언트 입장에서는 직접 상태를 조작하거나 하지 않아도 된다는 점이다. 즉, 클라이언트는 상태를 몰라도 된다라는 뜻이다
      public class StatePatterMain { // Client-side
          public static void main(String[] args) {
              Light l = new Light(); // Context
              l.offButton();
              l.onButton();
              l.onButton();
          }
      }
      ```

      

- 案例 ：宾馆管理系统

   <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-01 at 11.28.35 PM.png" alt="Screen Shot 2021-06-01 at 11.28.35 PM" style="zoom: 23%;" />

   <img src="//assets/images/软设/myNote/pic_04/Screen Shot 2021-06-01 at 11.29.59 PM.png" alt="Screen Shot 2021-06-01 at 11.29.59 PM" style="zoom: 33%;" />

  ```java
  //重构之后的“空闲状态类”示例代码 
  ......
  if(预订房间){
    预订操作;
    房间类.setState(new 已预订状态类()); // *每次切换一个状态new一次，性能太低了
  }
  else if(住进房间) {
    入住操作;
    房间类.setState(new 已入住状态类());
  } 
  ......
  ```

  - ***但是没有办法完全实现符合 “开闭原则”***

  - *상태들이 행위를 수행하면서 Context 객체의 상태를 수시로 바꾸어주기 때문에 싱글톤으로 작성하지 않으면 매번 새로운 인스턴스가 생겨 불필요한 메모리를 잡아 먹을 것이고 전체적으로 성능 저하의 원인이 될 것이기 때문에 싱글톤으로 작성하였다.*

    출처: https://coding-start.tistory.com/247 

  - **状态对象尽可能小**

  - **尽可能状态对象本身是无状态的** 

    - 这个状态对象每次重新调用一个状态对象方法的时候，它应该呈现一致的行为，而不是变化的行为。

    

- 案例：论坛用户等级

  - 用户在每一个状态下都有不同行为
  - 怎么判断这个行为呢？ 在于分积分

  

- 适用环境
  - 在工作流或游戏等类型的软件中得以广 泛使用
  - 使用状态模式可以描述工作流对象(如批文)的状态转换以及不同状态下它所具有的行为
  - 使用状态模式可以对游戏角色进行控制





### 命令模式(Command Pattern)

- 命令模式的作用：使得请求发送者与请求接收者消除彼此之间的耦合

- 属于对象行为型模式

- a.k.a  Action Pattern, Transaction Pattern

- 将一个请求封装为一个对象，从而使我们可用不同的请求对客户进行参数化

- 对请求排队或者记录请求日志，以及支持可撤销的操作

   <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-02 at 10.49.01 AM.png" alt="Screen Shot 2021-06-02 at 10.49.01 AM" style="zoom: 33%;" />

  - Ex) 리모콘은 실내의 전등을 키고 끄는 버튼이 있고 TV를 켜고 끄는 버튼도 있습니다. 추후에는 다양한 전자기기를 다룰 수 있는 버튼이 추가될 가능성이 있습니다

  [Invoker]

  ```java
  // 리모콘 역할을 하는 클래스에는 구체적인 행위가 정의되어 있지 않습니다。 생성자의 매개변수 혹은 setter를 이용하여 주입받은 Command 객체로 행위를 호출하는 호출자(Invoker) 역할을 하게됩니다
  public class RemoteControlInvoker {
      private Command command; 
    	// 变化的原因不在于Invoker自生有不同的行为，而是因为Invoker有一个参数本身是另外一个对象。
      public RemoteControlInvoker(Command command) {
          this.command=command;
      }
   
      public void setCommand(Command command) {
          this.command = command;
      }
      
      public void pressedButton() {
          command.execute();
      }   
  }
  ```

  [Abstract Command]

  ```java
  public interface Command {
      public void execute();
  }
  ```

  [Concrete Command] -- 각 Concrete Command는 어떤 Reciever 객체에게 어떤 동작을 할 것인지를 적으면 된다

  ```java
  // 每一个命令都是一个操作
  public class TvOnCommand implements Command {
      private TV tv;
      public TvOnCommand(TV tv) {
          this.tv=tv;
      }
      @Override
      public void execute() {
          tv.turnOn();
      }
  }
   
  public class TvOffCommand implements Command {
      private TV tv;
      public TvOffCommand(TV tv) {
          this.tv=tv;
      }
      @Override
      public void execute() {
          tv.turnOff();
      }
  }
   
  public class LightOnCommand implements Command {
      private Light light;
      public LightOnCommand(Light light) {
          this.light=light;
      }
      @Override
      public void execute() {
          light.turnOn();
      }
  }
   
  public class LightOffCommand implements Command {
      private Light light;
      public LightOffCommand(Light light) {
          this.light=light;
      }
      @Override
      public void execute() {
          light.turnOff();
      }
  }
  ```

  [Reciever] -- 명령을 받고 명령대로 동작해야되는 대상

  ```java
  public class TV {
      public void turnOn() {
          System.out.println("TV 켰음");
      }
      public void turnOff() {
          System.out.println("TV 껐음");
      }
  }
   
  public class Light {
      public void turnOn() {
          System.out.println("불 켰음");
      }
      public void turnOff() {
          System.out.println("불 껐음");
      }
  }
  ```

  [Client]

  ```java
  public class CommandMain {
      public static void main(String[] args) {
          TV tv = new TV();
        	// Client完成了命令的创建
          TvOnCommand tvOnCommand = new TvOnCommand(tv);
          TvOffCommand tvOffCommand = new TvOffCommand(tv);
          
          Light light = new Light();
          // Client完成了命令的创建
          LightOnCommand lightOnCommand = new LightOnCommand(light);
          LightOffCommand lightOffCommand = new LightOffCommand(light);
          
          RemoteControlInvoker invoker = new RemoteControlInvoker(tvOnCommand);
          invoker.pressedButton();
          invoker.setCommand(tvOffCommand);
          invoker.pressedButton();
          invoker.setCommand(lightOnCommand);
          invoker.pressedButton();
          invoker.setCommand(lightOffCommand);
          invoker.pressedButton();
      }
  }
  ```

  

  - Invoker：

    - Invoker는 어떠한 기능이 추가되더라도 코드 변경없이 기능을 얼마던지 추가 할 수 있습니다.
    - 我们真正开始执行命令的人不是Client而是Invoker

  - Client为什么出现在模式中? ：因为它完成了命令的创建

  - 명령을 캡슐화 하였고, **명령을 보내는 책임(Invoker)과 명령을 실행하는 책임(Receiver)을 나눴다**.

    

- 优点：

  - 降低系统的耦合度

  - 新的命令可以很容易地加入到系统中

  - 可以方便地实现对请求的Undo和Redo

    [Invoker에 undo() 추가]

    ```java
    public class RemoteControlInvoker {
        private Command command; 
        public RemoteControlInvoker(Command command) {
            this.command=command;
        }
     
        public void setCommand(Command command) {
            this.command = command;
        }
        
        public void pressedButton() {
            command.execute();
        }
      	public void undo() {
            command.undo();
        }
    }
    ```

    [Abstract Command에 undo() 정의]

    ```java
    public interface Command {
        public void execute();
      	public void undo();
    }
    ```

    [각 Concrete Command에 undo() 구현]

    ```java
    public class TvOnCommand implements Command {
        private TV tv;
        public TvOnCommand(TV tv) {
            this.tv=tv;
        }
        @Override
        public void execute() {
            tv.turnOn();
        }
      	@Override
        public void undo() {
            tv.turnOff();
        }
    }
     
    ...
    ```

    

  - Macro를 쉽게 설계할 수 있음 = 组合模式 + 命令模式

    [Concrete Command 추가]

    ```java
    public class MacroCommand implements Command {
        private List<Command> commands;
        public MacroCommand() {
            commands= new ArrayList<>();
        }
        
        public void addCommand(Command...commands) {
            if(commands.length < 1) throw new NullPointerException();
            
            for(Command c : commands) {
                this.commands.add(c);
            }
        }
      
        public void removeCommand(Command...commands) {
           ...
        }
        
        @Override
        public void execute() {
            if(commands.size()>0) {
                commands.stream().forEach(c->c.execute());
            }
        }
    }
    ```

    [Client 에다 실행시킬 명령 순서를 적으면됨]

    ```java
    public class CommandMain {
     
        public static void main(String[] args) {
            
            TV tv = new TV();
            TvOnCommand tvOnCommand = new TvOnCommand(tv);
            TvOffCommand tvOffCommand = new TvOffCommand(tv);
            
            Light light = new Light();
            LightOnCommand lightOnCommand = new LightOnCommand(light);
            LightOffCommand lightOffCommand = new LightOffCommand(light);
            
          	// Macro명령 만듬
            MacroCommand macroCommand = new MacroCommand();
            macroCommand.addCommand(tvOnCommand,
                                    tvOffCommand,
                                    lightOnCommand,
                                    lightOffCommand);
            // invoker를 통해 Macro명령를 호출하여 실행
          	RemoteControlInvoker invoker = new RemoteControlInvoker(macroCommand);
            invoker.pressedButton();
            
        }
     
    }
    ```

    

- 缺点

  - 导致过多的Concrete Command类

  





### 观察者模式(Observer Pattern)

- a.k.a  

  - **Publish/Subscribe Pattern,** 
  - **Model/View Pattern,** 
  - **Source/Listener Pattern,** 
  - **Dependents Pattern**

- 属于对象行为模式

- 定义**<span style="color:red">对象</span>间的一种一对多依赖关系**, 使得每当一个对象状态发生改变时,其相关依赖对象皆得到通知并被自动更新

- ***发生改变的对象A称为"观察目标(Subject)"***

  <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-08 at 11.21.43 PM.png" alt="Screen Shot 2021-06-08 at 11.21.43 PM" style="zoom: 33%;" />

  - 抽象的Subject类的方法
    - attach() : 添加观察者
    - detach() : 移除观察者
    - notify() : 需要通知的时候（Subject发生变化的时候）
      - 实现 ：遍历所有的队列中的观察者
  - ⚠️注意点1 ： 三个<span style="color:blue">蓝色</span>，一个<span style="color:purple">紫色</span> 
    - <span style="color:purple">紫色</span>是"Interface"
    - <span style="color:blue">蓝色</span>是"Abstract"
    - interface와 abstract의 차이 : https://www.youtube.com/watch?v=VuJHRyIq-w0
  - :warning: 注意点2：Subject拥有observers的引用, ConcreteObserver拥有subject的引用
    - 这些引用有什么作用？

  [Subject Class]

  ```java
  import java.util.*;
  public abstract class Subject {
  	protected ArrayList observers = new ArrayList(); 
    public abstract void attach(Observer observer); 
    public abstract void detach(Observer observer); 
    public abstract void notify();
  }
  ```

  [ConcreteSubject Class]

  ```java
  public class ConcreteSubject extends Subject {
    public void attach(Observer observer) {
    	observers.add(observer);
    }
    public void detach(Observer observer) {
      observers.remove(observer);
    }
    public void notify() {
      for(Object obs:observers) {
        ((Observer)obs).update(); //通知观察者这件事情，不能说“我数据变了，结束了”。那么数据是啥？
        // 我们怎么传数据？这个update()需要带参数
        // 如果update()里参数很多的话怎么办？
        // 
      }
    }
  }
  ```

  [Observer Class]

  ```java
  public interface Observer {
  	public void update(); 
  }
  ```

  [ConcreteObserver Class]

  ```java
  public class ConcreteObserver implements Observer {
    public void update() {
      //具体更新代码 
    } 
  }
  ```

  [Client]

  ```java
  Subject subject = new ConcreteSubject(); 
  Observer observer = new ConcreteObserver(); 
  subject.attach(observer);
  subject.notify();
  ```

  

- 观察者模式的问题

  1. <span style="color:red">Observer为啥是Interface？</span>

     <span style="color:red">**声明成接口的都是和应用功能正交的功能，而且观察者中的update都是需要各个观察者自己实现的，基本上没有什么公共部分**</span>

  2. **Subject的引用到底有什么用？**

     The Subject may simply use the update() function and set a value in the Observer. In this way, we can eliminate the reference. Alternatively, it can inform the Observer that something has changed, and the Observer will contact the Subject using the reference it has and receive the new values.
     The implementation of holding the reference can also be utilized if the Observers need to tell the Subject about something.

  3. Update( )方法到底应该如何来写？参数如何来写？

     

- 实例 ：猫、狗与老鼠

  고양이는 쥐와 개의 관찰 목표(Subject)이고 쥐와 개는 관찰자(Observer)이다. 고양이가 울면 쥐는 도망가고 개는 소리는 쫒는다.

  <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-08 at 11.54.40 PM.png" alt="Screen Shot 2021-06-08 at 11.54.40 PM" style="zoom:40%;" />



- 优点

  - 可以实现表示层和数据逻辑层的分离
    - 表示层：ConcreteObserver的角色
    - 数据逻辑层：Subject
  - 在Subject和Observer之间建立一个抽象的耦合
  - 支持广播通信
  - 符合“开闭原则”
    - 可以添加，修改Observer

- 缺点

  - 将所有的观察者都通知到会花费很多时间

  - 循环观察的问题

    - A通知B, B通知C，C通知D，D通知A ..
    - 系统很容易崩溃

    

- 适用环境

    <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-09 at 12.11.47 AM.png" alt="Screen Shot 2021-06-09 at 12.11.47 AM" style="zoom:20%;" />

-  模式应用

  -  观察者模式在软件开发中应用非常广泛
    - 如某电子商务网站可以在执行发送操作后给用户多个发送商品打折信息，
    - 某团队战斗游戏中某队友牺牲将给所有成员提示等等 --> 多对多关系
      - 如果每一个人维护一个观察者队列，整个代码特别大 -- （所以中介者模式来解决）
    - 凡是涉及到一 对一或者一对多的对象交互场景都可以使用观察者模式

- 模式扩展

  - <span style="color:red">**Java语言提供的对观察者模式的支持**</span>

    - JDK的java.util package中, 提供了Observable抽象类以及Observer接口

       <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-09 at 12.27.00 AM.png" alt="Screen Shot 2021-06-09 at 12.27.00 AM" style="zoom:30%;" />

    - Observer接口

      ```java
      void update(Observable o, Object arg);
      ```

      - 传递数据的时候，你把这些数据统一的封装成Object。然后通过参数传过去

      - 那么为啥有Observable？

        - **一个Observer可能观察很多Subject**。因此需要根据Observer的类型来判断到底变化以便获取数据

        - 例如正确转化Object的类型，获取正确数据

        - **用来让观察者识别当前到底是哪个被观察者发来的更新信息，以便进行下一步处理**

          

    - Observable类

      <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-09 at 12.44.17 AM.png" alt="Screen Shot 2021-06-09 at 12.44.17 AM" style="zoom:30%;" /> 

      - notify() 有两个版本

        - 有参数的是push模式, 将所有信息都push过去
        - 无参数的是pull模式，传递给update的第二个Object类参数是null。观察者保留被观察者的引用，然后主动去pull信息

      - 为啥有Observable？

        1. 支持pull模式
        2. 控制更新的频度 (通过setChange方法) -- 什么时候要调用更新方法

        

  - MVC模式

    - Subject = Model
    - Observer = View, Controller充当两者之间的Mediator
    - 当Model层的数据发生改变时，View层将自动改变其显示内容





### 中介者模式(Mediator Pattern) 

- 在用户与用户直接聊天的设计方案中，用户对象之间存在很强的关联性

- 属于对象行为型模式

- 用一个"中介对象"来封装一系列的对象交互

  - 中介者使各对象不需要显式地相互引用
  - 可以独立地改变它们之间的交互

   <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-09 at 11.10.53 AM.png" alt="Screen Shot 2021-06-09 at 11.10.53 AM" style="zoom:30%;" />

  

  - 中介者模式可以使对象之间的关系数量急剧减少

      <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-09 at 11.12.41 AM.png" alt="Screen Shot 2021-06-09 at 11.12.41 AM" style="zoom:30%;" />

  - Mediator承担两方面的职责:

    1. 在结构上中转作用
    2. 在行为上协调作用

  

- 优点 ：

  1. 简化了对象之间的交互
  2. 将各Colleague解耦
  3. 减小子类生成
  4. 可以简化各Colleague类的设计和实现

- 缺点：

  1. 在ConcreteMediator类中包含了Colleague之间的交互细节，可能会导致**ConcreteMeditator类非常复杂**，使得**系统难以维护**

- 适用环境：

  - 系统中对象之间存在复杂的引用关系
  - 想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类

- 应用：

  - MVC是Java EE的一个基本模式
    - 此时Controller作为一种中介者
    - View上用的是组合模式
    - 不同的Controller用的是策略模式

- 中介者模式与迪米特法则

  - 在中介者模式中，通过创造出一个Mediator对象，将系统中有关的对象所引用的其他对象数目减少到最少，使得一个对象与其Colleague之间的相互作用被这个对象与Mediator对象之间的相互作用所取代
  - <span style="color:red">中介者模式就是迪米特法则的的一个典型应用</span>



### 模板方法模式(Template Method Pattern)

- 继承复用的是静态的结构信息
  - 我可以复用一个算法结构（或者说框架的步骤）
- 合成复用的是动态的行为信息
- 该模式就是体现继承优势的模型之一

- 属于类行为型模式

- 子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤

   <img src="/assets/images/软设/myNote/pic_04/Screen Shot 2021-06-09 at 12.07.24 PM.png" alt="Screen Shot 2021-06-09 at 12.07.24 PM" style="zoom: 33%;" />

  - templateMethod()：通常被声明称final static

  - 只有类之间的继承关系，没有对象关联关系

  - [Abstract Class -- 父类] 의 Method 종류 :

    1. Template Method(模版方法) = 把基本操作方法组合在一起形成一个总算法

    2. Operation Method(基本方法) = 实现算法各个步骤的方法

       <有三种方法>

       - Abstract Method

       - Concrete Method

       - Hook Method - 通过在子类中实现 的钩子方法对父类方法的执行进行约束，实现子类对父类行为的反向控制

         - Hook Method 종류1 - 空方法 ：嵌入在算法的步骤中（例如开头或者结尾，用户可以添加实现，也可以不添加）

         - Hook Method 종류2 - ’挂钩‘方法 ：一种是返回bool值的方法，可以控制是否执行某个步骤

       ```java
       public abstract class AbstractClass{
         public void templateMethod(){ // Template Method(模版方法)
           primitiveOperation1(); 
        	 	primitiveOperation2(); 
         	primitiveOperation3();
           if(isPrint()){
             doSomething();
           }
         }
         
       	//Operation Method(基本方法) -- Concrete Method
         public void primitiveOperation1(){
         	//实现代码
         }
         
         //Operation Method(基本方法) -- Abstract Method
       	public abstract void primitiveOperation2(); 
         
         //Operation Method(基本方法) -- Hook Method ’挂钩‘方法
         public boolean isPrint() { 
       		return true; 
         }
         
         //Operation Method(基本方法) -- Hook Method 空方法
         public void primitiveOperation3(){ 
           
         }
       
       ```

    [Concrete Class -- 子类]

    ```java
    public class ConcreteClass extends AbstractClass {
      public void primitiveOperation2() {
      	//实现代码 
      }
      public void primitiveOperation3() {
      	//实现代码 
      }
    }
    ```

    

- 优点：

  - 在一个类中抽象地定义算法，而由它的子类实现细节的处理
  - 一种代码复用的基本技术
  - 反向的控制结构
    - 通过一个父类调用其子类的操作
    - 通过对子类的扩展增加性的行为
    - 符合“开闭原则”

- 缺点：

  - 定义一个子类导致类的个数增加
    - 但是更加符合“单一职责原则”，使得类的内聚性提高

- 适用情况：

  - 各子类中公共的行为应被提取出来并集中到一个公共父类中以避免代码重复

  - 算法中固定不变的部分设计为Abstract Class中Template Method和(基本方法) Concrete Method，而一些可以改变的细节由其Concrete Class来实现

  - 控制子类的扩展，确保父类控制处理流程的逻辑顺序

    

- Hollywood Principle
  - 在Template Method Pattern中，
    - 子类不显式(명시적으로)调用父类的方法
    - 而是通过覆盖父类的方法来实现某些具体的业务逻辑
  - 这种机制被称为Hollywood Principle
    - 子类不需要调用父类，而通过父类来调用子类





---

《Reference》

1. 2021年(春) 软件设计系统 : 潘敏学



***

