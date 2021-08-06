---
layout: article
title: 04. Structural patterns
author: J_宋
tags: SoftwareSystemDesign 中文 한글
mathjax: true
key: systemDesign04

---



### 适配器模式(Adapter Pattern) 

- 原来不能直接进行调用的或者模块组合的模块能够联合起来适用
- 我们希望能够保留客户类的代码，同时目标类（比如第三包）也改不了，所以能够加上新的类使得目标类的接口转换成为客户类希望的接口类型。保证现有类进行重用
- 适配器 - Adapter : 包装的类，也就是额外加进去的类
- 适配者 - Adaptee：包装的对象，被适配的类，第三包，目标类
  - 这样客户类不需要直接访问Adaptee类

###### 定义

- 将一个接口转换成 客户希望的另一个接口，适配器模式使接口不兼容的 那些类可以一起工作

- a.k.a **Wrapper**

- 有两种实现

  - 类结构型模式：通过继承去做 

    <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 8.56.28 PM.png" alt="Screen Shot 2021-06-26 at 8.56.28 PM" style="zoom:40%;" />

    - Target : **接口**

    - 通过类继承的复用的好处：可以改变原来的代码，因为他是白盒复用

      《Adapter》

      ```java
      public class Adapter extends Adaptee implements Target {
        public void request() {
        	specificRequest();
        } 
      }
      ```

      

  - 对象结构型模式：通过合成去做

    <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 8.53.38 PM.png" alt="Screen Shot 2021-06-26 at 8.53.38 PM" style="zoom:40%;" />

    - Target : 接口或者抽象随便

    - 最大的优点 ：可以适配相同类型的不同的子类

      《Adapter》

      ```java
      public class Adapter extends Target {
        private Adaptee adaptee;
        public Adapter(Adaptee adaptee) {
        	this.adaptee=adaptee; 
        }
        public void request() {
          adaptee.specificRequest();
        }
      }
      ```

  

  ##### 实例 ： 加密适配器

  사용자 정보를 암호화한 후 데이터베이스에 저장하는 암호화 모듈을 제공해야 하는 시스템은 이미 데이터베이스 Operation 클래스를 정의했다.개발 효율을 높이기 위해 기존의 암호화 알고리즘을 重用 해야 하는데, 이러한 알고리즘들은 제3자가 제공하는 Class에 encapsulate 되어있고 심지어 어떤 것은 소스 코드가 없는 경우도 있다.适配器模式를 사용하여 이 암호화 모듈은 기존 클래스를 수정하지 않고 제3자 암호화 방법을 重用합니다

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 9.17.36 PM.png" alt="Screen Shot 2021-06-26 at 9.17.36 PM" style="zoom:30%;" />

  

  ##### 优点

  - 将目标类和适配者类解耦
  - 添加了类的透明性和复用性
  - 灵活性和扩展性非常好

  

  ##### 适用环境

  - 系统需要使用现有的类，而这些类的接口不符合系统的需求

  ##### 应用

  - JDBC
  - InputStreamAdapter

  ##### 扩展

  - Default Adapter Pattern

    - 当不需要全部实现接口提供的方法时，可先设计一个抽象类实现接口，并为该接口中每一个方法提供一个默认实现(空方法)

      <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 9.33.04 PM.png" alt="Screen Shot 2021-06-26 at 9.33.04 PM" style="zoom: 33%;" />

  - 双向适配器

    - Adapter를 사용하는 자와 目标类가 서로 调用 할 수 있는 Adapter

      <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 9.35.58 PM.png" alt="Screen Shot 2021-06-26 at 9.35.58 PM" style="zoom: 25%;" />

  

### 组合模式(Composite Pattern)

- 最容易被联合使用的模式

- 树结构

  - Parent node = Composite Class (복합 객체)
  - Leaf node = Leaf Class (개별 객체)

  이런 복합 객체와 개별 객체를 Client가 동일하게 사용하도록 한다

  

##### 定义

- 组合多个对象形成树形结构以表示“整体-部分”的结构层次
- 对单个对象(即叶子对象)和组合对象(即容器对象)的使用具有一致性
- a.k.a Part-Whole

- 属于对象结构型模式



<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 9.48.23 PM.png" alt="Screen Shot 2021-06-26 at 9.48.23 PM" style="zoom:30%;" />

​	《Component》

```java
public abstract class Component {
	public abstract void add(Component c);
  public abstract void remove(Component c); 
  public abstract Component getChild(int i); 
  public abstract void operation();
}
```

​	《Leaf》

```java
// 不需要每个都实现代码，只需要实现operation()
public class Leaf extends Component {
  public void add(Component c) { 
    //异常处理或错误提示 （表示“你调错了”）
  }
  public void remove(Component c) { 
    //异常处理或错误提示 
  }
  public Component getChild(int i) { 
    //异常处理或错误提示 
  }
  public void operation(){
  	//实现代码
  }
}
```

​	《Composite》

```java
public class Composite extends Component {
	private ArrayList list = new ArrayList();
  public void add(Component c) {
  	list.add(c); 
  }
  public void remove(Component c) {
  	list.remove(c); 
  }
  public Component getChild(int i) {
  	(Component)list.get(i); 
  }
  public void operation() {
    for(Object obj:list) {
    	((Component)obj).operation();
    }
  } 
}
```



- Component的方法需要是两者的并集

- 缺点 ：add,remove, getChild在leaf上用不了，安全性问题

  - 我的代码在leaf上进行操作的时候，不能保证用户不在leaf上比如说调用add方法
  - 那么这个时候，我的代码没写好，那么这个代码会出错。

- **为了能够让代码更好的被维护和使用，牺牲了部分的安全性来获得透明性**

- Composite可以包含Leaf，也可以包含Composite -- 形成一个树形结构型

  

##### 实例 ： 文件浏览

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 10.23.52 PM.png" alt="Screen Shot 2021-06-26 at 10.23.52 PM" style="zoom:30%;" />



##### 优点

- 可以清晰地定义分层次的复杂对象
- 可以形成复杂的树形结构
- 更容易在组合体内加入对象构件

##### 缺点

- 设计变得更抽象

##### 适用环境

- 需要表示一个对象整体或者部分层次
-  对象的结构是动态并且复杂程度不一样，但客户需要一致地处理它们

##### 应用

- XML文档解析
- 操作系统的目录结构

##### 扩展

- 透明组合模式 ： 对待叶子节点和非叶子节点一样，所有的接口方法都可以调用

  - **透明组合模式中叶子节点的三个方法都是要抛出异常的，不是可以调用的方法**

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 10.38.49 PM.png" alt="Screen Shot 2021-06-26 at 10.38.49 PM" style="zoom:30%;" />

- 安全组合模式 ： 调用前要先判断一下是什么类型再调用其方法

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 10.39.22 PM.png" alt="Screen Shot 2021-06-26 at 10.39.22 PM" style="zoom:30%;" />



### 桥接模式(Bridge Pattern)

《Coloring Shape》

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 10.57.58 PM.png" alt="Screen Shot 2021-06-26 at 10.57.58 PM" style="zoom:30%;" />

- 对于有两个变化维度(即两个变化的原因)的系统，采用桥接模式来进行设计系统中类的个数更少， 且系统扩展更为方便
- **将继承关系转换为关联关系**， 从而降低了类与类之间的耦合，减少了代码编写量

##### 定义

- 将抽象部分与它的实现部分分离，使它们都可以独立地变化

- 属于对象结构型模式

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 10.50.20 PM.png" alt="Screen Shot 2021-06-26 at 10.50.20 PM" style="zoom:33%;" />

  - 抽象部分（图形）：就是类的抽象过程，加一个几个不同的各类抽象成一个父类。所以抽象部分是类中间他的本身的本质

  - 实现部分（颜色）：逆操作，当我加上了具体的内容之后，变成一个具体的类。

    《 Implementor 》

    ```java
    public interface Implementor {
    	public void operationImpl(); 
    }
    ```

    《 Abstraction 》

    ```java
    public abstract class Abstraction {
    	protected Implementor impl;
    	public void setImpl(Implementor impl) {
    		this.impl = impl; 
      }
    	public abstract void operation(); 
    }
    ```

    《 RefinedAbstraction 》

    ```java
    public class RefinedAbstraction extends Abstraction {
      public void operation() {
      	//代码 
        impl.operationImpl(); 
        //代码
      } 
    }
    ```

    

- 策略模式和桥接模式差异在目的上
  - 策略模式：每一个Context只会使用1个策略，
  - 桥接模式：解决类型爆炸问题。每一个RefinedAbstraction是一个类型，所有的RefinedAbstraction都是可用

##### 优点

- 分离抽象接口及其实现部分
- 提高了系统的可扩充性

##### 缺点

- 增加系统的理解与设计难度

##### 适用环境

- 系统需要在构件的抽象化角色和具体化角色之间增加更多的灵活性
- 抽象化角色和具体化角色可以以继承方式对立扩展而互不影响
- 一个类存在两个独立变化的维度

##### 应用

- JVM
- JDBC



##### 适配器模式+桥接模式

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 11.38.55 PM.png" alt="Screen Shot 2021-06-26 at 11.38.55 PM" style="zoom:40%;" />



### 装饰模式(Decorator Pattern)

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 12.00.07 AM.png" alt="Screen Shot 2021-06-27 at 12.00.07 AM" style="zoom:30%;" />

- Class 또는 Object에 行为를 추가할 수 있는 2가지 방법 :
  1. 继承 ：子类在拥有自身方法的同时还拥有父类的方法
     - **继承是一种耦合度较大的静态关系，无法在程序运行时动态扩展**
  2. 关联：将一个类的对象嵌入另一个对象中
- 装饰模式以对客户透明的方式**动态地给一个对象附加上更多的责任**（额外的行为）



##### 定义

- 动态地给一个对象增加一些额外的职责(Responsibility)，就增加对象 功能来说，装饰模式比生成子类实现更为灵活

- a.k.a Wrapper
  - 适配器模式的Wrapper是为了应对变化的接口
  - 装饰模式的Wrapper是为了增加一个新的行为

- 属于对象结构型模式

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-26 at 11.57.43 PM.png" alt="Screen Shot 2021-06-26 at 11.57.43 PM" style="zoom:30%;" />

- ConcreteComponent :被装饰的对象 （例子上图片）-- 没有这个就变成无穷递归，这就是递归的Base Condition

- Decorator : （例子上小框）

- ***Component*** : 跟组合模式中的Component同样的作用

  - 使得可以继续装饰
  - 用户使用的时候，他只要使用Component的类型。因此不需要知道这个对象是装饰过的还是不装饰过的

- ConcreteDecoratorB中addedBehavior()是否违反了Liskov替换原则

  - 违反了
  - ConcreteDecoratorB.operation()里面调用的时候，可以使用增加的新的方法。应该是private addedBehavior()

《Decorator》

```java
public class Decorator extends Component {
  private Component component;
  public Decorator(Component component) {
  	this.component=component; 
  }
  public void operation() {
  	component.operation(); 
  }
}
```

《ConcreteDecorator》

```java
public class ConcreteDecorator extends Decorator {
  public ConcreteDecorator(Component component) {
  	super(component); 
  }
  public void operation() {
    super.operation();
    addedBehavior(); 
  }
  public void addedBehavior(){
  	//新增方法 
  }
}
```

《 Client 》

```java
public static void main(String args[]){
  //小对象太多
  //易于出错
  Component robot = new ConcreteDecorA(new ConcreteDecorB(new ConcreteComponent()));、
  robot.operation();
}
```



##### 实例 ： 变形金刚

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 12.51.31 AM.png" alt="Screen Shot 2021-06-27 at 12.51.31 AM" style="zoom:40%;" />

- 这个模式能不能完全符合依赖倒转？
  - 不能，这是半透明的装饰模式。如果要使用robot里面新加的方法，就要了解当前的类型，因此是不透明的



##### 优点

- 装饰模式可以提供比继承更多的灵活性
- 通过一种动态的方式来扩展一个对象的功能
- 可以创造出很多不同行为的组合
- ConcreteComponent类与ConcreteDecorator类可以对立变化

##### 缺点

- 装饰方法只能在正常对象前或者后调用，因此只能改变前置或者后置，而不能改变中间实现过程
- 小对象太多，可以和工厂方法连用，用来创建小对象
- 装饰模式比继承更加易于出错

##### 扩展

- 透明装饰模式
- 半透明装饰模式
  - 不在面向抽象编程，依赖倒转的情况



### 外观模式(Facade Pattern)

- Facade ： 건물의 출입구로 이용되는 정면 외벽 부분을 가리키는 말이다

- 用户只需要直接与外观角色交互，用户与子系统之间的复杂关系由外观角色来实现

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 9.53.04 AM.png" alt="Screen Shot 2021-06-27 at 9.53.04 AM" style="zoom:30%;" />  

  

  

  ##### 定义

  - 外部与一个子系统的通 信必须通过一个统一的外观对象进行

  - Facade ： 为子系统中的一组接口提供一个一致的界面

    <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 9.57.00 AM.png" alt="Screen Shot 2021-06-27 at 9.57.00 AM" style="zoom:30%;" />

    《 Facade 》

    ```java
    public class Facade {
    	private SubSystemA obj1 = new SubSystemA(); 
      private SubSystemB obj2 = new SubSystemB(); 
      private SubSystemC obj3 = new SubSystemC(); 
      
      public void method(){
      	obj1.method(); 
        obj2.method(); 
        obj3.method();
      } 
    }
    ```

    

    - 跟哪个原则切合的？
      - 降低客户类与子系统类耦合度 -- **迪米特法则（最小知识原则）**
      - **单一职责原则**
    - ***跟中介者模式很像***
      - 中介者(Mediator)是将对象之间的交互职责封装起来
      - 外观(Facade)对象封装的是客户对复杂系统的协调



##### 优点

- 实现了子系统与客户之间的松耦合关系
- 降低了大型软件系统中的编译依赖性
- 使得客户使用系统更加容易



##### 缺点

- 不能很好地限制客户使用子系统类
- 不引入Abstract Facade的情况下，**增加新的子系统可能需要修改Facade类或者Client类的源代码** -- **违反“开闭原则”**



##### 使用环境

- 当要为一个复杂子系统提供一个简单的接口时
- 客户程序与多个子系统之间存在很大的依赖性



##### 扩展

- 多个Facade类

- 不要通过继承一个Facade类在子系统中增加新的行为，这种做法是 ❌

- 为了不违反“开闭原则”，引入Abstract Facade类

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 10.38.59 AM.png" alt="Screen Shot 2021-06-27 at 10.38.59 AM" style="zoom:33%;" />

  

  

### 享元模式(Flyweight Pattern)

- 当对象数量太多时，将导致运行代价过高，带来性能下降等问题

- 该模式通过共享技术实现相同或相似对象的重用

- 内部状态(Intrinsic State) ： 可以共享的相同内容

- 外部状态(Extrinsic State) ：需要外部环境来设置的不能共享的内容

##### 定义

- 运用共享技术有效地支持大量细粒度对象的复用
- 属于对象结构型模式

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 3.39.04 PM.png" alt="Screen Shot 2021-06-27 at 3.39.04 PM" style="zoom:40%;" />

- FlyweightFactory : 

  - FlyweightFactory的作用在于提供一个用于存储Flyweight对象的Flyweight Pool
  - 先看一看HashMap中Key有没有 
    - 如果有的话接着return
    - 没有的话创建新的对象然后把它放到HashMap之后return

- **跟哪个模式很像？**

  - 原型模式 : 	比较一下PrototypeManager和FlyweightFactory
    - PrototypeManager只负责管理
    - FlyweightFactory还负责创建
    - PrototypeManager不一定要有但FlyweightFactory是享元是模式的一部分

- 享元模式是一个**考虑系统性能的设计**模式

  《 FlyweightFactory 》

  ```java
  public class FlyweightFactory {
    private HashMap flyweights = new HashMap();
    public Flyweight getFlyweight(String key) {
      if(flyweights.containsKey(key)) {
        return (Flyweight)flyweights.get(key);
    	}else{
        Flyweight fw = new ConcreteFlyweight();
        flyweights.put(key,fw);
  			return fw;
      }
    }
  }
  ```

  《 Flyweight 》

  ```java
  public class Flyweight{
    //内部状态作为成员属性
    private String intrinsicState;
    
    public Flyweight(String intrinsicState) {
    	this.intrinsicState = intrinsicState; 
    }
    
    public void operation(String extrinsicState) {
    	...... 
    }
  }
  ```



##### 优点

- 可以极大减少内存中对象的数量
- Flyweight对象可以在不同的环境中被共享

##### 缺点

- 需要分离出内部状态和外部状态 -- 使得程序的逻辑复杂化
- 读取外部状态使得运行时间变长

##### 适用环境

- 有大量相同或者相似的对象
- 对象大部分状态都可以外部化
- 在多次重复使用Flyweight对象时



##### 应用

在JDK类库中定义的String类使用了Flyweight模式

```java
public class Demo{
  public static void main(String args[]){
    String str1 = "abcd"; 
    String str2 = "abcd"; ✅
    String str3 = "ab" + "cd";
    String str4 = "ab";
    str4 += "cd"; ✅
    System.out.println(str1 == str2);
    System.out.println(str1 == str3);
    System.out.println(str1 == str4);
  }
}
```



##### 扩展

- 简单工厂模式生成Flyweight对象

- FlyweightFactory可以使用单例模式(Singletone)进行设计

- 享元模式+组合模式

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 4.31.36 PM.png" alt="Screen Shot 2021-06-27 at 4.31.36 PM" style="zoom:30%;" />





### 代理模式(Proxy Pattern)

- 非常非常难的一个模式 ： 他的思想比较简单，但是变体比较多
- 通过Proxy对象去掉客户不能看到的内容和服务或者添加客户需要的额外服务
  - 代理模式和其他的封装的很大区别 ：改变了我们需要访问的对象的可见性和行为
- 通过引入一个新的对象来实现
  1. 对真实对象的操作
  2. 将新的对象作为真实对象的一个替身

##### 定义

- 给某一个对象提供一个代理，并由代理对象控制对原对象的引用
- 属于对象结构型模式

<img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 4.52.36 PM.png" alt="Screen Shot 2021-06-27 at 4.52.36 PM" style="zoom:30%;" />

- Proxy对象和RealSubject对象接口是一致的
- 可以增加一些新的行为来控制对RealSubject的访问(preRequest和postRequest)
  - preRequest : 先检测一下你的权限是否满足
  - realSubjectrequest : 提供服务
  - postRequest

《 Proxy 》

```java
public class Proxy implements Subject {
  private RealSubject realSubject = new RealSubject();
  
  public void preRequest() {
    ......
  }
  
  public void request(){
  	preRequest(); 
    realSubject.request(); 
    postRequest();
  }
  
  public void postRequest() {
    ......
  }
}
```



##### 实例 ：论坛权限控制代理

- 한 카페에서 기존 가입자와 관광객의 권한은 다릅니다. 기존 가입자는 게시글을 사용할 수 있습니다, 자신의 등록정보 수정, 자신의 게시글 수정 등의 기능을 하고, 관광객은 다른 사람의 게시글만 볼 수 있을 뿐 다른 권한은 없습니다.이 권한 관리 모듈을 설계하기 위해 프록시 모드를 사용합니다 （保护代理）

  <img src="/assets/images/软设/myNote/pic_05/Screen Shot 2021-06-27 at 5.14.33 PM.png" alt="Screen Shot 2021-06-27 at 5.14.33 PM" style="zoom:33%;" />

  - 在PermissionProxy当中 ：根据用户不同的Permission来决定是否能调用这样的方法
  - 如果有新的等级，那么RealPermission不需要改变，只需要增加一个新的Proxy就行了

  

##### 扩展

- 远程代理（Remote）
  - 为一个位于不同的地址空间的对象提供一个本地的Proxy对象,这个不同的地址空间可以是在同一台主机中，也可以在另一台主机中
- 虚拟代理（Virtual）
  - 如果需要创建一个资源消耗较大的对象，先创建一个消耗相对较小的对象来表示，真实对象只在需要时才会被真正创建
  - 虚拟代理模式是一种内存节省技术
  - 对系统进行优化并提高运行速度
- 保护代理（Protect or Access）
  - 控制对一个对象的访问，可以给不同的用户提供不同级别的使用权限





---

《Reference》

1. 2021年(春) 软件设计系统 : 潘敏学



***

