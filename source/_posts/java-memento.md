---
title: java-备忘录模式
date: 2021-12-13 19:45:43
categories: designMode
tags: "设计模式"
---
### 备忘录模式

#### 基本介绍：  

1. 备忘录模式(Memento patten)在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样以后就可以将该对象恢复到原先保存的状态。
2. 现实生活中的备忘录是用来记录某些要去做的事情，或者是记录已经达成的共同意见的事情，以防忘记了。而在软件层面，备忘录模式有着相同的含义，备忘录主要用来记录一个对象的某种状态，或者某些数据，当要做回退时，可以从备忘录对象里获取原来的数据进行恢复操作。

![image-20211213193806376](java-memento/memento01.png)



#### 代码实现：

```java
//用来保存状态 state可以替换为各种想要保存的东西
public class Memento {
	private String state;

	//构造器
	public Memento(String state) {
		super();
		this.state = state;
	}

	public String getState() {
		return state;
	}
}
```

```java
//想要使用备忘录的对象 拥有恢复状态可备份状态的功能
public class Originator {

	private String state;//状态信息

	public String getState() {
		return state;
	}

	public void setState(String state) {
		this.state = state;
	}
	
	//编写一个方法，可以保存一个状态对象 Memento
	//因此编写一个方法，返回 Memento
	public Memento saveStateMemento() {
		return new Memento(state);
	}
	
	//通过备忘录对象，恢复状态
	public void getStateFromMemento(Memento memento) {
		state = memento.getState();
	}
}
```

```java
//对应一个角色可以有很多个状态，所以用List来保存便于管理
public class Caretaker {
	
	//在List 集合中会有很多的备忘录对象
	private List<Memento> mementoList = new ArrayList<>();
	
	public void add(Memento memento) {
		mementoList.add(memento);
	}
	
	//获取到第index个Originator 的 备忘录对象(即保存状态)
	public Memento get(int index) {
		return mementoList.get(index);
	}
}
```

```java
//客户端
public class Client {

	public static void main(String[] args) {

		Originator originator = new Originator();
		Caretaker caretaker = new Caretaker();
		
		originator.setState(" 状态#1 攻击力 100 ");
		//保存了当前的状态
		caretaker.add(originator.saveStateMemento());
		
		originator.setState(" 状态#2 攻击力 80 ");
		caretaker.add(originator.saveStateMemento());
		
		originator.setState(" 状态#3 攻击力 50 ");
		caretaker.add(originator.saveStateMemento());
		
		System.out.println("当前的状态是 =" + originator.getState());
		//希望得到状态 1, 将 originator 恢复到状态1
		
		originator.getStateFromMemento(caretaker.get(0));
		System.out.println("恢复到状态1 , 当前的状态是");
		System.out.println("当前的状态是 =" + originator.getState());
	}
}
```

游戏案例代码实现：

```java
//守护者对象, 保存游戏角色的状态
public class Caretaker {

	//如果只保存一次状态
	private Memento memento;
	//对GameRole 保存多次状态
	//private ArrayList<Memento> mementos;
	//对多个游戏角色保存多个状态
	//private HashMap<String, ArrayList<Memento>> rolesMementos;

	public Memento getMemento() {
		return memento;
	}
	public void setMemento(Memento memento) {
		this.memento = memento;
	}

}
```

```java
public class GameRole {

	private int vit;
	private int def;
	
	//创建Memento ,即根据当前的状态得到Memento
	public Memento createMemento() {
		return new Memento(vit, def);
	}
	
	//从备忘录对象，恢复GameRole的状态
	public void recoverGameRoleFromMemento(Memento memento) {
		this.vit = memento.getVit();
		this.def = memento.getDef();
	}
	
	//显示当前游戏角色的状态
	public void display() {
		System.out.println("游戏角色当前的攻击力：" + this.vit + " 防御力: " + this.def);
	}

	public int getVit() {
		return vit;
	}

	public void setVit(int vit) {
		this.vit = vit;
	}

	public int getDef() {
		return def;
	}

	public void setDef(int def) {
		this.def = def;
	}
}
```

```java
public class Memento {

	//攻击力
	private int vit;
	//防御力
	private int def;
	public Memento(int vit, int def) {
		super();
		this.vit = vit;
		this.def = def;
	}
	public int getVit() {
		return vit;
	}
	public void setVit(int vit) {
		this.vit = vit;
	}
	public int getDef() {
		return def;
	}
	public void setDef(int def) {
		this.def = def;
	}
}
```

```java
public class Client {

	public static void main(String[] args) {
		//创建游戏角色
		GameRole gameRole = new GameRole();
		gameRole.setVit(100);
		gameRole.setDef(100);
		
		System.out.println("和boss大战前的状态");
		gameRole.display();
		
		//把当前状态保存caretaker
		Caretaker caretaker = new Caretaker();
		caretaker.setMemento(gameRole.createMemento());
		
		System.out.println("和boss大战~~~");
		gameRole.setDef(30);
		gameRole.setVit(30);
		
		gameRole.display();
		
		System.out.println("大战后，使用备忘录对象恢复到站前");
		
		gameRole.recoverGameRoleFromMemento(caretaker.getMemento());
		System.out.println("恢复后的状态");
		gameRole.display();
	}
}
```





#### 注意事项和细节

1. 给用户提供了一种可以恢复状态的机制，可以使用户能够比较方便的回到某个历史的状态。
2. 实现了信息的封装，使得用户不需要关心状态的保存细节。
3. 如果类的成员变量过多，势必会占用比较大的资源，而且每一次保存都会消耗一定的内存、这个需要注意。
4. 使用的应用场景
   - 后悔药
   - 游戏存档
   - Windows里的ctrl+z
   - 浏览器的回退 
   - 数据库的事务管理
5. 为了节省内存，备忘录模式可以结合原型模式一起食用