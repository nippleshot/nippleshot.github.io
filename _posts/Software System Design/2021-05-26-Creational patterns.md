---
layout: article
title: 02. Creational patterns
author: J_宋
tags: SoftwareSystemDesign 中文 한글
mathjax: true
key: systemDesign02
---

***

### 复习

- 设计模式的前提 ： 

1. 反复出现的问题
2. 必然是一定的约束条件下

- 模式的使用上分为创建型模式，结构型模式，行为型模式

- 模式的实现上分为类模式和对象模式
  - 类模式：继承为主要方式实现的模式
  - 对象模式：合成为主要关系的模式

***



### 简单工厂模式 (Simple Factory Pattern)

- 定义
  - a.k.a : Static Factory Method
  - 属于类创建型模式
  - 简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类

 <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-24 at 10.11.29 PM.png" alt="Screen Shot 2021-05-24 at 10.11.29 PM" style="zoom:23%;" />



- 简单工厂模式开闭原则不太好 -- Factory类的职责相对过重

- 어떤게 필요하면 오직 한개의 정확한 parameter만 집어넣으면 필요한 객체를 얻을 수 있다. 그리고 만들어지는 자세한 과정은 알 필요가 없다

- 实例 ：권한 관리

  어떤 한 시스템에서 시스템이 사용자가 로그인할 때 입력한 ID랑 비밀번호와 데이터베이스에 있는 ID와 비밀번호가 일치한지 비교하여 신분을 검사한다. 만약 신분 검사가 통과됬다면 데이터베이스에서 해당 사용자의 권한 등급을 불러와 권한 등급에 대응하는 사용자 객체를 생성한다. 권한 등급에 따라 객체가 가질 수 있는 동작들이 다르다.

  <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-24 at 10.34.00 PM.png" alt="Screen Shot 2021-05-24 at 10.34.00 PM" style="zoom:23%;" /> 

- 好处 
  - **可以让客户在不了解创建对象的时候，能够复用创建对象的代码**
  - 将对象的创建和对象本身业务处理分离可以降低系统的耦合度
- 缺点
  - **Factory类的职责相对过重 - 开闭原则不太好**
  - 不利于系统扩展困难
- 适用环境
  - Factory类负责创建的对象比较小
- 应用
  - 在JDK类库中，DataFormat类



### 工厂方法模式 (Factory Method Pattern)

- 简单工厂模式最大的缺点 ：	
  - 添加新的Product class的话，需要修改Factory class - 违反“开闭原则”

- 定义
  - a.k.a : Virtual Constructor Pattern, Polymorphic Factory Pattern
  - 属于类创建型模式
  - 继承关系为主

<img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-24 at 11.15.00 PM.png" alt="Screen Shot 2021-05-24 at 11.15.00 PM" style="zoom: 25%;" />

[ Factory {Abstract} Class ]

```java
public abstract class Factory {
	public abstract Product getConcreteProduct(); 
}
```

[ ConcreteFactory Class ]

```java
public class ConcreteFactory extends Factory {
  public Product getConcreteProduct() {
  	return new ConcreteProduct(); 
  }
}
```



- 核心的Factory类不再负责所有Product的创建，而是将具体创建工作交给子类去做
- 为了提高系统的可扩展性和灵活性，在定义工厂和产品时都必须使用抽象层

[客户端代码]

```java
Factory factory; //Factory {Abstract} Class
Product concreteProduct; //Product {Abstract} Class
factory = new ConcreteFactory();  //ConcreteFactory Class
concreteProduct = factory.getConcreteProduct();
concreteProduct.doSomething();
```

- ConcreteFactory仍然可以接受参数，将不太会变化的放在一类工厂里面
- 用户只需要关心所需产品对应的工厂，无须关心创建细节，甚至无须知道具体产品类的类名



- 实例 ：log 기록 시스템

  어떤 log 기록 시스템이 여러가지 log 기록 방식을 제공할려한다. 예를 들어 파일 기록, 데이터베이스 기록 등등. 그리고 사용자가 요구에 따라 동적으로 log 기록 방식을 선택할 수 있다

   <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-24 at 11.35.06 PM.png" alt="Screen Shot 2021-05-24 at 11.35.06 PM" style="zoom: 25%;" />

- 优点 

  - 更好的支持开闭原则
  - 灵感性更好

- 缺点
  - 다른 Concrete Product 클래스를 추가하면 이에 대응하는 Concrete Factory 클래스도 같이 추가해야된다 
  - 代码的复杂度高
- 适用环境
  - 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时可以无须关心是哪一个工厂子类创建产品子类，需要时再动态指定，可将具体工厂类的类名存储在配置文件或数据库中。



### 抽象工厂模式 (Abstract Factory Pattern)

- 有时候，我们需要一个工厂能够提供多个产品对象，而不是单一的产品对象

- 我们希望在代码中添加一个约束

  - 每次在制定一个工厂的时候，可以生产多种类型的产品，同时这个工厂生产出来的都是同一个产品族的产品

    <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-26 at 9.44.09 PM.png" alt="Screen Shot 2021-05-26 at 9.44.09 PM" style="zoom:30%;" /> 

  - 产品等级结构 = 不同类型的产品

    - 其实这个就是“产品的继承结构”
    - 比如说一个抽象类是电视机，那么下面的子类有海尔电视机，TCL电视机等等
    - 抽象电视机和具体品牌的电视机之间构成了一个“等级结构”

  - 产品族 = 同一个工厂生产的位于不同的产品等级结构中的一族产品

    - 相关联的不同类型的产品所构成的集合
    - 比如说，海尔电器工厂生产的海尔电视机，海尔冰箱等，这个是一个产品族

- 抽象工厂模式是简单工厂之下，有份加了新的约束条件（还希望能够生产有关联的不同类型的产品）

- 抽象工厂模式和工厂方法模式最大的区别在于

  - 工厂方法模式只生产一种产品的类型（一个产品等级）
  - 抽象工厂模式需要面对多个产品等级结构

- 定义

  - 我们什么时候使用抽象工厂模式？
    - ***当我们需要相关或相互依赖对象的时候***
  - a.k.a : Kit pattern
  - 属于对象创建型模式

   <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-26 at 10.04.30 PM.png" alt="Screen Shot 2021-05-26 at 10.04.30 PM" style="zoom: 33%;" />

  [ Factory {Abstract} Class ]

  ```java
  public abstract class AbstractFactory {
    public abstract AbstractProductA createProductA();
    public abstract AbstractProductB createProductB(); 
  }
  ```

  [ ConcreteFactory Class ]

  ```java
  public class ConcreteFactory1 extends AbstractFactory {
    public AbstractProductA createProductA() {
    	return new ConcreteProductA1(); 
    }
    public AbstractProductB createProductB() {
    	return new ConcreteProductB1();
    }
  }
  ```

  

- 优点 
  - 可以实现高内聚低耦合的设计目的 
    - 因为相关联的对象都高内聚，而不相关的对象不同的工厂隔离了所以低耦合
  - 符合“开闭原则”
    - 分析“开闭原则”的时候主要针对产品，而不是针对工厂
    
    - 工厂是为了解决产品的创建而提出的
    
    - 所以这里增加新的产品族很方便。所以增加新的产品族的方面符合“开闭原则”
    
    - 要增加新的产品类型实际上几乎是不可能的事 
      
      (생각해보면 새로운 AbstractProductC Class를 추가하는게 쉽지않음)
      
      - 增加新的产品类型必然会牵涉到改动“抽象层”
  
- 缺点

  - 难以扩展抽象工厂来生产新种类的产品

    - **增加新的工厂和产品族容易**
    - **增加新的产品等级结构麻烦** - 왜냐하면 Abstract Factory 클래스를 수정하면서 같이 Concrete Factory class 도 수정해야되는 상황이 발생하거든

  - No pain No gain : 完全的灵活性， 付出的代价是额外的抽象层

    

### 建造者模式 (Builder Pattern)

- 该模式用的不是特别多

- 属于对象创建型模式

- 建造者模式可以将部件和其组装过程分开，一步一步创建一个复杂的对象

   <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-26 at 11.57.26 PM.png" alt="Screen Shot 2021-05-26 at 11.57.26 PM" style="zoom:33%;" />

  [ Product Class ]

  ```java
  public class Product {
    private String partA; //可以是任意类型 
    private String partB;
    private String partC; 
    //partA的Getter方法和Setter方法省略 
    //partB的Getter方法和Setter方法省略 
    //partC的Getter方法和Setter方法省略
  }
  ```

  [ Builder {Abstract} Class ]

  ```java
  public abstract class Builder {
  	protected Product product = new Product();
    
  	public abstract void buildPartA(); 
    public abstract void buildPartB(); 
    public abstract void buildPartC();
    
    public Product getResult() {
    	return product; 
    }
  }
  ```

  [ Director Class ]

  ```java
  public class Director{
    private Builder builder;
    
    public Director(Builder builder){
  		this.builder=builder;
  	}
  	public void setBuilder(Builder builder){
  		this.builder=builer;
  	}
    
  	public Product construct(){
  		builder.buildPartA();
      builder.buildPartB(); 
      builder.buildPartC(); 
      return builder.getResult();
  	} 
  }
  ```

  [Client Class]

  ```java
  ......
  Builder builder = new ConcreteBuilder(); 
  Director director = new Director(builder); 
  Product product = director.construct(); 
  ......
  ```

  ​	

  - 用户只关心具体的创建的对象，而不关心对象的组件
  - Director ：主要是用来负责控制产品的生成过程
    - 该模式由于提供了额外的Director类，它隔离了客户和生成过程 :arrow_right: 客户端只需要知道具体的Director类型，通过Director类可以获得一个完整的产品对象
    - 只有抽象工厂的时候，实际上这些产品如何组合是需要暴露给Client代码来做的

  

- 实例 ：KFC套餐

  建造者模式可以用于描述KFC如何创建套餐:套餐是一 个复杂对象，它一般包含主食(如汉堡、鸡肉卷等)和 饮料(如果汁、可乐等)等组成部分，不同的套餐有不 同的组成部分，而KFC的服务员可以根据顾客的要求， 一步一步装配这些组成部分，构造一份完整的套餐，然后返回给顾客。

  - 不同的部分创建的就是Builder

  - 服务员是Director

      <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-28 at 12.40.23 PM.png" alt="Screen Shot 2021-05-28 at 12.40.23 PM" style="zoom:53%;" />



- 优点

  - 用户只需要使用不同的Director就可以得到不同的创建对象

  - 更好的控制复杂度

    - 如果具体创建很复杂，通过Director我们可以精细的控制创建的过程

  - 增加新的ConcreteBuilder，无需修改修改原有的代码，Director针对Abstract Builder编程的

    - 符合”开闭原则“

      

- 缺点

  - **如果Product之间的差异性很大**，则不适合使用建造者模式
  - **如果Product的内部变化复杂**，需要定义ConcreteBuilder，导致系统变得很庞大。



- 适用环境

  - 需要生成的产品对象的属性相互依赖，需要指定其生成顺序

  - 在很多游戏软件中，地图包括天空、地面、背景等组成部分，人物角色包括人体、服装、装备等组成部分，可以使用建造者模式对其进行设计，通过不同的具体建造者创建不同类型的地图或人物。

    

- 建造者模式与抽象工厂模式的比较

  - 返回产品 ：
    -  抽象工厂模式 ： 生产一个产品族的产品
    - 建造者模式：复杂（需要封装和复用）的完整产品
  - 实现过程 ：
    - 抽象工厂模式 ：客户端实例化Factory类，然后调用Factory方法获取所需Product对象
    - 建造者模式：客户端可以不直接调用建造者的相关方法， 而是通过Director类来指导如何生成对象，包括对象的组装过程和建造步骤， 它侧重于一步步构造一个复杂对象，返回一个完整的对象
  - 如果将抽象工厂模式看成 "자동차 부품 생성 공장"
  - 那么建造者模式就是一个 "자동차 조립 공장"



### 原型模式 (Prototype Pattern)

- 属于对象创建型模式

- 通过给出一个原型对象来指明所要创建的对象的类型，然后用复制这个原型对象的办法创建出更多同类型的对象

  <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-28 at 3.32.04 PM.png" alt="Screen Shot 2021-05-28 at 3.32.04 PM" style="zoom: 33%;" />

  - java.lang.Object提供一个clone()方法
  - 实现克隆的Java类必须实现一个标识接口Cloneable，表示这个Java类支持复制
  - 한 클래스에 Member variable들 포함하고 있으면，Member variable의 객체도 같이 복사 할지 안할지에 따라 이 패턴은 두가지 형식으로 나눌 수 있음 : 深克隆和浅克隆

- 实例 ： Email 복사 -- 深克隆

  - 메일 객체는 여러가지 내용들(발송자, 수신자, 제목, 내용, 날짜 등등)을 포함하고 있기 때문에, 어떤 시스템에서 메일 복사 기능을 제공할려면 이미 만들어진 메일 객체에 대해서 복제 방식을 통해 새로운 메일 객체를 만들 수 있다. 만약에 어떤 한 부분을 수정하고 싶으면 원래 메일 객체를 수정할 필요없이 오직 복제한 새로운 메일 객체만 수정하면 된다. 

     [ Java version ]

     <img src="/assets/images/软设/myNote/pic_03/Screen Shot 2021-05-28 at 3.49.05 PM.png" alt="Screen Shot 2021-05-28 at 3.49.05 PM" style="zoom:33%;" /> 
  
     

- 优点
  - 简化对象的创建过程
  - 提高新实例的创建效率
- 缺点
  - 需要为每一个类配备一个克隆方法
  - 实现深克隆时需要编写较为复杂的代码
- 适用环境
  - **创建新对象成本较大**
  - 避免使用分层次的工厂类来创建分层次的对象
- 应用 ：Struts2中Action对象，Spring bean实例





---

《Reference》

1. 2021年(春) 软件设计系统 : 潘敏学



***

