# 设计模式
## 什么是设计模式
设计模式是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、使得代码更容易被他人理解、保证代码的可靠性、程序的重用性。 

## 设计模式六大原则
### 单一职责原则
单一职责原则简称 SRP，他想表达的就是字面意思，一个类只承担一个职责。

有时候我们可以将一个复杂的接口拆成两个不同的接口，这两个接口承担着不同的责任，这就是依赖了单一职责原则；它的定义就是：应该有且仅有一个原因引起类的变更。

关于职责的定义很模糊，什么才是职责呢？不同的人有不同的解读，所以该原则很难运用，需要开发者的慧眼。

下面以一个简单的例子展示
![DP1](../Images/DesignPattern_1.png)

### 里氏替换原则
- 子类可以扩展父类的功能，但不能改变父类原有的功能；
- 只要父类出现的地方子类就可以出现，而且替换为子类不会出现任何错误。

### 依赖倒转原则
依赖倒置原则的 3 个含义：
- 高层模块不应该依赖低层模块，两者都应该依赖其抽象；
- 抽象不应该依赖细节；
- 细节应该依赖抽象；

这里的抽象指的就是接口或抽象类，细节指的是实现类 。
其核心思想是：要面向接口编程，不要面向实现编程。

为什么面向接口呢？面向接口就是面向抽象，由于在软件设计中，细节具有多变性，而抽象层则 相对稳定 ，因此以抽象为基础搭建起来的架构要比以细节为基础搭建起来的架构要稳定得多。

### 接口隔离原则
接口隔离原则（Interface Segregation Principle，ISP）要求程序员尽量将臃肿庞大的接口拆分成更小的和更具体的接口，让接口中只包含客户感兴趣的方法。
就是要让接口中的方法尽可能的少而精。
同时注意，根据接口隔离原则拆分接口时，首先必须满足单一职责原则，不能无限拆分。

### 迪米特法则

迪米特法则（Law of Demeter，LoD）又叫作最少知识原则（Least Knowledge Principle，LKP)，产生于 1987 年美国东北大学（Northeastern University）的一个名为迪米特（Demeter）的研究项目，由伊恩·荷兰（Ian Holland）提出，被 UML 创始者之一的布奇（Booch）普及，后来又因为在经典著作《程序员修炼之道》（The Pragmatic Programmer）提及而广为人知。

迪米特法则的定义是：只与你的直接朋友交谈，不跟“陌生人”说话（Talk only to your immediate friends and not to strangers）。其含义是：如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。其目的是降低类之间的耦合度，提高模块的相对独立性。

迪米特法则中的“朋友”是指：当前对象本身、当前对象的成员对象、当前对象所创建的对象、当前对象的方法参数等，这些对象同当前对象存在关联、聚合或组合关系，可以直接访问这些对象的方法。

### 开闭原则
开闭原则是Java世界里最基础的设计原则，它指导我们如何建立一个稳定的、灵活的系统。
他要求软件实体应该对扩展开放，对修改关闭。
这里的软件实体包括：
- 模块；
- 抽象和类；
- 方法；

前面提到的几个原则都是开闭原则的具体形态，也就是说前五个原则就是指导设计的工具和方法，而开闭原则才是精神领袖。

开闭原则 在面向对象设计领域中的地位类似于牛顿第一定律在力学中的地位。

## 设计模式如何分类
通常情况下，我们可以将设计模式分为创建模式、结构模式和行为模式三种类型。

创建型模式：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式

结构型模式：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式

行为型模式：策略模式、模板方法模式、观察者模式、迭代器模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式

## 创建型模式
### 单例模式
保证一个类只有一个实例，并且提供一个访问该实例的全局访问点

**应用**：

- 日志应用
- 数据库连接池
- 多线程线程池

具体实现方式可以去查看网上相关资料，一般分为懒汉式和饿汉式。Java语言实现懒汉式一般来说是使用静态内部类的方式。

### 工厂方法模式和抽象工厂模式
工厂方法模式是一种创建型设计模式，它定义了一个用于创建对象的接口，子类决定实例化哪一个类。这样做的目的是将对象创建的过程封装起来，当增加新的产品类时，不需要修改工厂类，只需要修改工厂子类。

抽象工厂模式是一种创建型设计模式，它定义了具体产品类和具体工厂类之间的一个抽象接口。每个具体工厂类只能创建一个具体产品类的实例。这种模式涉及到多个抽象产品类，每个抽象产品类可以派生出多个具体产品类。

## 结构型模式
