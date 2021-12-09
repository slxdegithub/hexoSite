---
title: java-中介者模式
date: 2021-12-09 14:02:37
categories: designMode
tags: "设计模式"
---
### 中介者模式

传统案例

智能家居项目，智能家庭包括各种设备，闹钟，咖啡机，电视机，窗帘等

主人要看电视时，各个设备可以完成看电视的准备工作，比如流程为：闹铃响起，咖啡机开始做咖啡，窗帘自动落下，电视开始播放。



#### 基本介绍

1. 中介者模式(Mediator Pattern)，	用一个中介者对象来封装一系列的交互对象，中介者使各个对象不需要显式的相互作用，从而使其耦合松散，而且可以独立的改变他们之间的交互
2. 中介者模式属于行为型模式，使代码易于维护
3. 比如MVC模式，C(Controller控制器)是M(Model模型）和（View视图）的中介者，在前后端交互时气到了中间人的作用。



原理类图

![mediator](mediator01.png)

类图说明

1. Mediator就是抽象中介者，定义了同事对象到中介者对象的接口
2. Colleague是抽象同事类
3. `ConcreteMediator` 具体的中介者对象，它需要知道所有的具体的同事类，即以一个集合来管理(`HashMap`)，并接受某个同事对象消息，完成相应的任务
4. `ConcreteColleague` 具体的同事类，会有很多，每个同事只知道自己的行为，而不了解其他的行为(方法)，但是他们都依赖中介者对象。



#### 代码实现

实现类图：

![mediator02](mediator02.png)

代码：

```java
//中介者抽象类
public abstract class Mediator {
	//将给中介者对象，加入到集合中
	public abstract void Register(String colleagueName, Colleague colleague);

	//接收消息, 具体的同事对象发出
	public abstract void GetMessage(int stateChange, String colleagueName);

	public abstract void SendMessage();
}
```

```java
//同事抽象类
public abstract class Colleague {
	private Mediator mediator;
	public String name;

	public Colleague(Mediator mediator, String name) {
		this.mediator = mediator;
		this.name = name;
	}

	public Mediator GetMediator() {
		return this.mediator;
	}

	public abstract void SendMessage(int stateChange);
}
```

```java
//具体的中介者类
public class ConcreteMediator extends Mediator {
	//集合，放入所有的同事对象
	private HashMap<String, Colleague> colleagueMap;
	private HashMap<String, String> interMap;

	public ConcreteMediator() {
		colleagueMap = new HashMap<String, Colleague>();
		interMap = new HashMap<String, String>();
	}

	@Override
	public void Register(String colleagueName, Colleague colleague) {
		colleagueMap.put(colleagueName, colleague);

		if (colleague instanceof Alarm) {
			interMap.put("Alarm", colleagueName);
		} else if (colleague instanceof CoffeeMachine) {
			interMap.put("CoffeeMachine", colleagueName);
		} else if (colleague instanceof TV) {
			interMap.put("TV", colleagueName);
		} else if (colleague instanceof Curtains) {
			interMap.put("Curtains", colleagueName);
		}

	}

	//具体中介者的核心方法
	//1. 根据得到消息，完成对应任务
	//2. 中介者在这个方法，协调各个具体的同事对象，完成任务
	@Override
	public void GetMessage(int stateChange, String colleagueName) {

		//处理闹钟发出的消息
		if (colleagueMap.get(colleagueName) instanceof Alarm) {
			if (stateChange == 0) {
				((CoffeeMachine) (colleagueMap.get(interMap
						.get("CoffeeMachine")))).StartCoffee();
				((TV) (colleagueMap.get(interMap.get("TV")))).StartTv();
			} else if (stateChange == 1) {
				((TV) (colleagueMap.get(interMap.get("TV")))).StopTv();
			}

		} else if (colleagueMap.get(colleagueName) instanceof CoffeeMachine) {
			((Curtains) (colleagueMap.get(interMap.get("Curtains"))))
					.UpCurtains();

		} else if (colleagueMap.get(colleagueName) instanceof TV) {//如果TV发现消息

		} else if (colleagueMap.get(colleagueName) instanceof Curtains) {
			//如果是以窗帘发出的消息，这里处理...
		}

	}

	@Override
	public void SendMessage() {

	}

}
```

```java
//同事类

//具体的同事类
public class Alarm extends Colleague {

	//构造器
	public Alarm(Mediator mediator, String name) {
		super(mediator, name);
		//在创建Alarm 同事对象时，将自己放入到ConcreteMediator 对象中[集合]
		mediator.Register(name, this);
	}

	public void SendAlarm(int stateChange) {
		SendMessage(stateChange);
	}

	@Override
	public void SendMessage(int stateChange) {
		//调用的中介者对象的getMessage
		this.GetMediator().GetMessage(stateChange, this.name);
	}

}

//咖啡机
public class CoffeeMachine extends Colleague {

	public CoffeeMachine(Mediator mediator, String name) {
		super(mediator, name);
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void StartCoffee() {
		System.out.println("It's time to startcoffee!");
	}

	public void FinishCoffee() {

		System.out.println("After 5 minutes!");
		System.out.println("Coffee is ok!");
		SendMessage(0);
	}
}

//窗帘
public class Curtains extends Colleague {

	public Curtains(Mediator mediator, String name) {
		super(mediator, name);
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void UpCurtains() {
		System.out.println("I am holding Up Curtains!");
	}

}

//TV
public class TV extends Colleague {

	public TV(Mediator mediator, String name) {
		super(mediator, name);
		mediator.Register(name, this);
	}

	@Override
	public void SendMessage(int stateChange) {
		this.GetMediator().GetMessage(stateChange, this.name);
	}

	public void StartTv() {
		System.out.println("It's time to StartTv!");
	}

	public void StopTv() {
		System.out.println("StopTv!");
	}
}
```

```java
//Client
public class ClientTest {

	public static void main(String[] args) {
		//创建一个中介者对象
		Mediator mediator = new ConcreteMediator();
		
		//创建Alarm 并且加入到  ConcreteMediator 对象的HashMap
		Alarm alarm = new Alarm(mediator, "alarm");
		
		//创建了CoffeeMachine 对象，并  且加入到  ConcreteMediator 对象的HashMap
		CoffeeMachine coffeeMachine = new CoffeeMachine(mediator, "coffeeMachine");
		
		//创建 Curtains , 并  且加入到  ConcreteMediator 对象的HashMap
		Curtains curtains = new Curtains(mediator, "curtains");
		TV tV = new TV(mediator, "TV");
		
		//让闹钟发出消息
		alarm.SendAlarm(0);
		coffeeMachine.FinishCoffee();
		alarm.SendAlarm(1);
	}

}
```



#### 源码分析

> `MVC`模式中 View 和 Controller 的关系就有点像 中介者模式 
>
> View想要访问另一个View 就通过Controller调用各种Model ，条件判断 ，最终返回另一个View



#### 注意事项和细节

1. 多个类相互耦合，会形成网状结构，使用终结者模式将网状结构分离为星形结构，进行了解耦
2. 减少了类间依赖，降低了耦合，符合了迪米特法则
3.  中介者承担了较多的责任，一旦中介者出现了问题，整个系统就会受到影响
4. 如果设计不当，中介者对象本身变得过于复杂，这点在实际使用的时候要注意，代码要谨慎写。