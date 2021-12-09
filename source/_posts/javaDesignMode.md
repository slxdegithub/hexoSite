---
title: java设计模式
date: 2021-12-03 18:51:08
categories: designMode
tags: "设计模式"
---
#  设计模式原则

设计原则核心思想:

1. 找出应用中可能需要变化之处，把他们独立出来，不要和那些不需要变化的代码放在一起。

2.  针对接口编程，而不是针对实现编程。

3. 为了交互对象之间的松耦合设计努力  

```
设计模式常见的七大原则：
1)单一职责原则
2)接口隔离原则
3)依赖倒置原则
4)里式替换原则
5)开闭原则
6)迪米特原则
7)合成复用原则
```

1. 代码重用性

2. 可读性

3. 可扩展性

4. 可靠性

5. 高内聚低耦合




## 单一隔离

```
原则上 一个类应该尽量做一件事 一个类继承一个接口 
如果实现类两个职责 当职责一进行修改的时候 很可能回对职责二造成影响
但是一个类继承一个接口会导致开销过大
在接口方法比较少的情况下可以 通过向下兼容 实现方法的单一职责
```



## 接口隔离

```
如果一个接口方法过多，实现该接口就会需要去实现很多不需要实现的方法。
这个时候我们就应该把接口进行拆分，去实现需要实现的接口即可。
```



## 依赖倒转(倒置)

```
接口和抽象类的价值在于 设计
高层模块不应该依赖于底层模块
抽象不应该依赖细节，细节应该依赖抽象
面向接口编程
传递的三种方式
1.构造器传递
2.set接口传递
3.接口传递
使用接口或者抽象类的目的是制定好规范。而不涉及任何具体的操作，把展现细节的任务交给他们的实现类去完成
多了一个缓冲利于程序的扩展和优化

```



## 里式替换原则

```
问题：在编程中如何正确的实现继承   尽量满足里式替换原则 
子类尽量不要重写父类的方法
做到透明使用 
如果子类想使用父类的方法 ，但是有可能会不小心重写了父类的方法 倒是一系列应用 带来了程序的入侵性
所以可以 子类和父类都继承一个新的base类，base类实现了更为基础的代码和方法
这样子类可以放心的重写方法
达到的效果是 所有应用基类的类应该尽量做到透明使用

```

​	

## 开闭原则

```
开闭原则是编程中 最基础最重要的原则

一个软件实体类 模块和函数应该对外扩展开放(对提供方) 对修改关闭(对使用方)  用抽象构建，用实现扩展细节
当我们增加一个功能时候 应该增加代码而不是修改代码 尽量不去修改原有的代码 
当软件需要变化时 尽量通过扩展软件实体的行为来实现变化 而不是通过修改已有的代码来实现变化

编程中遵循其他原则以及使用设计模式的目的就是遵循开闭原则

改进思路分析 把创建的Shape类做成抽象类或者接口，并提供一个抽象的draw方法或者接口，让子类去实现即可。
这样有新的图形种类时候 只需要让新的图形去继承Shape 并且实现draw方法即可，这样使用方的代码就不需要修改

满足了开闭原则
```



## 迪米特法则

```
一个对象应该对其他的对象保持最少的了解
类与类之间的关系越密切，耦合度越大

一个类里面 除了传递参数依赖类 应该尽量避免出现其他的陌生类，降低耦合度 这样代码修改起来容易

```



## 合成服用原则

```
原则是尽量使用合成/聚合 而不是使用继承

B要想使用A的方法，可以继承于A 但是这样会导致关系太强 耦合度太高
组合： 让B 里面注入一个A 
聚合:  让B里面 set 一个A 或者构造器
依赖： 在B里面把A传进来 称之为B依赖A 方法
```



# 设计模式类型

```
设计模式分为三种类型，共23种
1、创建型模式：单例模式、抽象工厂模式、原型模式、建造者模式、工厂模式
2、适配器模式：桥接模式、装饰模式、组合模式、外观模式、享元模式、代理模式
3、行为型模式：模板方法模式、命令模式、访问者模式、迭代器模式、观察者模式、中介者模式、
备忘录模式、解释器模式、状态模式、策略模式、责任链模式
```

## 创建型模式

### 单例模式

#### 饿汉式(静态常量)

```java
class Singleton{
    //构造器私有化 外部能new
    private Singleton(){
      
    }
    //本类内部创建对象实例
    private final static Singleton instance = new Singleton();

	//提供一个共有的静态方法 返回实例对象
    public static Singleton getInstance(){
        return instance;
    }
}
/*
写法较为简单，在类转载的时候就完成了实例化，避免了线程同步的问题。
缺点是在类装载的时候就完成实例化，没有达到懒加载的效果(Lazy Loading)。如果从始至终从未使用过这个实例，则会造成内存浪费，
这种基于类加载机制避免了多线程同步的问题，不过instance在类装载的时候就完成实例化，在单例模式中大多数都是调用getInstance方法，但是导致类装载的原因有很多种，因此不能确定有其他的方式导致类装载，这个时候初始化instance就没有达到懒加载的效果
这种单例模式可以用，有可能造成内存浪费
*/
```

#### 饿汉式(静态代码块)

```java
class Singleton{
    //构造器私有化 外部能new
    private Singleton(){}
    //本类内部创建对象实例
    private  static Singleton instance ;
    static{
        instance = new Singleton();
    }

	//提供一个共有的静态方法 返回实例对象
    public static Singleton getInstance(){
        return instance;
    }
}
//和静态常量类似，在静态代码块完成实例化。 优缺点也和饿汉式静态常量一样
```



#### 懒汉式(线程不安全)

```java
class Singleton{
    private static Singleton instance;
    private Singleton(){}

    //提供一个静态方法，当使用这个方法的时候才实例化
    //懒汉式(线程不安全)
    public static Singleton getInstance(){
        if (instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}
/*
小结：
1.起到了懒加载的效果，但是只能在多线程下才能使用
2.如果在多线程下 会导致线程不安全 一个线程进入if语句还没执行完 另一个线程也进来了 就会产生多个实例
3.在实际开发中 不要使用这种方式
*/
```

#### 懒汉式(线程安全)

```java
class Singleton{
    private static Singleton instance;
    private Singleton(){}
    //提供一个静态方法，加入同步处理的代码，解决了线程安全问题
    //懒汉式(线程安全)
    public static synchronized Singleton getInstance(){
        if (instance == null){
        	//synchronized(Singleton.class){ 
            //锁放在这里 并不能实现线程安全 因为线程进了if语句 迟早会执行 
                   instance = new Singleton();
            //}    
        }
        return instance;
    }
}
/*
小结：
1.解决了线程不安全问题
2.效率太低了，每个线程想获得类的实例的时候 执行getInstance()方法都要进行同步。而其实这个方法只执行一次实例化就够了，后面想要获得该实例应该是直接return，方法进行同步效率太低
3.在实际开发中 不要使用这种方式
*/
```

#### 双重检查(推荐)

```java
class Singleton{
    private static volatile Singleton singleton;
    private Singleton(){}
    public static Singleton getInstance(){
        //双重检查
        //提供一个静态的共有方法，加入双重检查代码，解决线程安全问题，同时解决懒加载问题
        if (singleton == null){
            synchronized (Singleton.class){
                if (singleton == null){
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}

/*
小结：
1.双重检查 判断了两次singleton == null 就可以保证线程安全
2.实例化代码只用执行一次 后面再次访问 的时候 如果不为空 就可以直接返回实例化对象 避免了方法反复同步
3.线程安全：实现了懒加载，效率较高
在实际开发中 推荐使用这种单例设计模式
*/
```

#### 静态内部类

```java
class Singleton{
    private static volatile Singleton instance;
    //　当一个共享变量被volatile修饰时，它会保证修改的值会立即被更新到主存，
    //当有其他线程需要读取时，它会去内存中读取新值。 保证了 可见性。
    // 满足并发编程安全的三大特性 原子性 可见性 有序性
    //构造器私有化
    private Singleton(){}
    //写一个静态内部类，该类中有一个静态属性Singleton
    private static class SingletonInstance{
        private static final Singleton INSTANCE = new Singleton();
    }    
    
    //提供一个静态方法，直接返回SingletonInstance.INSTANCE
    public static synchronized Singleton getInstance(){
        return SingletonInstance.INSTANCE;
    }
}

/*
小结:
1.这种方式采用的类装载机制来保证初始化实例只有一个线程
2.静态内部类方式在Singleton类被加载的时候并不会立即实例化，而是在需要实例化的时候，调用getInstacne方法，才会装在SingleInstance类，从而完成Singeleton的实例化
3.类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们保证了线程的安全性，在类进行初始化的时候，别的线程是无法进入的。
4.避免了线程不安全，利用静态内部类特点实现了延迟加载，效率高。
在工作中推荐使用
*/
```

#### 枚举

```java
enum  Singleton{
   INSTANCE;
   public void sayOK(){
       System.out.println("ok~~~");
   }
}

/*
小结
1.这借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，而且还能防止反序列化重新创建新的对象。
2.这种方式是Effective Jaca坐着Josh Bloch提倡的方式
推荐使用
*/
```

单例模式有八种方式：

```
饿汉式(静态常量实例化)
饿汉式(静态代码块实例化)
懒汉式(线程不安全)
懒汉式(线程安全同步方法)
懒汉式(同步代码块 ，写法错误) 并不能实现线程安全
双重检查
静态内部类
枚举
```



单例模式注意事项和细节说明：

1)单例模式保证了系统内存中 该类只存在一个对象，节省了系统资源，对于一些需要频繁创建销毁的对象，使用单例模式可提高系统的性能
2）当想实例化一个单例类的时候，必须要记住使用相应的获取对象的方法，而不是直接使用new
3）单例模式使用的场景：需要频繁的进行创建和销毁的对象、创建对象时耗时过多或者耗费资源过多（即重量级对象），但又经常用到的对象，工具类对象、频繁访问数据库或文件的对象（比如数据源、session工厂等）

```java
如果确定实例一定会使用 饿汉式是可以使用的 只是有可能会造成内存浪费 
比如java的Runtime中就用了饿汉式 
推荐使用： 双重检查、静态内部类、枚举
```

### 工厂模式

#### 简单工厂模式

需求：一个披萨项目，要便于披萨种类的扩展，要便于维护

1. 披萨的种类有很多(比如`GreekPizz`、`Chjeesepizz`等)
2. 披萨的制作有prepare，bake，cut，box
3. 完成披萨店订购功能。



####  工厂方法模式







####  抽象工厂模式

1. 抽象工厂模式：定义了一个interface用于创建相关或有依赖关系的对象 cu答族，而无需指明具体的类
2. 抽象工厂模式可以将简单工厂模式和工厂方法进行整合
3. 从设计层面看，抽象工厂模式就是对简单工厂模式的改进或者说进一步的抽象
4. 将工厂抽象成两橙，`AbsFactory`(抽象工厂)和具体实现的工厂子类，程序员可以根据创建对象类型使用对应的工厂子类。这样将单个简单地工厂变成了工厂cu答族，更利于代码的维护

```java
定义一个抽象的工厂，然后定义实体工厂实现工厂方法。

```

> Calendar中使用了工厂模式

工厂模式小结

1. 工厂模式的意义：将实例化对象的代码提取出来，放到一个类中统一管理和维护，达到和主项目的依赖关系的解耦。从而提高项目的扩展和维护性。
2. 三种工厂模式（简单工厂模式，工厂方法模式，抽象工厂模式）
3. 设计模式的依赖抽象原则
   - 创建对象实例中，不要直接new类，而是把这个new类的动作放在一个工厂的方法中，并返回。有的书上说不要直接持有具体类的应用。
   - 不要让类继承具体类，而是继承抽象类或者是实现interface（接口）
   - 不要覆盖基类中已经实现的方法



###  原型模式

#### 需求

- 现有一只羊，姓名为tom，年龄为1：颜色为白色，请编写程序创建和tom羊属性完全相同的10只羊

```java
//传统方式
public class Sheep {
    private String name;
    private int age;
    private String color;
    //生成构造和getset方法以及toString等等...
}
 public static void main(String[] args) {
     Sheep sheep = new Sheep("tom",1,"白色");
     //克隆十次...
     new Sheep(sheep.getName(),sheep.getAge(),sheep.getColor());
 }
```

传统方式的优缺点

1. 有点是比较好理解，简单易操作
2. 在创建新的对象时，总是需要重新获取原始对象的属性，如果创建的对象比较复杂，效率较低
3. 总是需要重新初始化对象，而不是动态的获得对象运行时的状态，不够灵活

思路：`java`中Object类是所有类的父类，Object提供了一个clone方法，可以将一个`java`对象复制一份，但是需要实现clone的`java`类需要实现一个接口`Clonealbe`，该接口表示该类能够复制且具有复制的能力=>原型模式

#### 基本介绍

1. 原型模式(Prototype模式)是指：用原型实例指定创建对象的种类，并且通过拷贝这些原型，创建新的对象
2. 原型模式是一种创建型设计模式，允许一个对象在创建另外一个可定制的对象，无需知道如何创建的细节
3. 工作的原理：通过将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝他们来实施创建，即对象.clone() 
4. 形象理解：猴子拔出猴毛变成其他猴子

改进：

```java
public class Sheep implements Cloneable{
    private String name;
    private int age;
    private String color;
     //生成构造和getset方法以及toString等等...
	//克隆该实例 使用默认的clone方法来完成
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Sheep sheep = null;
        Sheep clone = (Sheep) super.clone();
        return clone;
    }
}

public class Client {
    public static void main(String[] args)  {
        Sheep sheep = new Sheep("tom",1,"包塞");
        try {
            //克隆十次
            Sheep clone1 = (Sheep) sheep.clone();
            Sheep clone2 = (Sheep) sheep.clone();
            //....
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
    }
}
```

>原型模式在Spring框架中源码分析

```java
 public void testImport(){
	 Object bean1 = applicationContext.getBean(xxx);     
     Object bean2 = applicationContext.getBean(xxx);  
     //如果是scope = prototype 多例 就用到了原型模式
     //getbean每次都是通过clone生成的对象 
 }
```

原型模式完成对象的创建，如果被克隆的对象中有对象属性，那么克隆的时候并不会被克隆

#### 深拷贝和浅拷贝

浅拷贝：

1. 对于数据类型是基本类型的成员变量，浅拷贝会直接进行值传递，也就是将该属性复制一份给新的对象
2. 对于数据类型是应用数据类型的成员变量，比如成员变量是某个数组，某个类的对象等，那么浅拷贝会进行引用传递，也就是只将该成员变量的引用值(内存地址)复制一份给新的对象。实际上两个对象都指向同一个实例，所以这种情况下 一个对象中修改该成员变量会影响到另一个对象的该成员变量值
3. 比如默认开启的对象克隆就是浅拷贝
4. ` Sheep clone2 = (Sheep) sheep.clone();`

深拷贝：

1. 复制对象的**所有**基本数据类型的成员变量值 
2. 为所有应用数据类型的成员变量**申请存储空间**，并复制每个应用数据类型成员变量所引用的对象，直到该对象可达的所有对象，**也就是说，对象进行深拷贝要对整个对象(包括引用对象)进行拷贝**
3. 深拷贝实现方式1：**重写clone**方法来实现深拷贝
4. 深拷贝实现方式2：通过**序列化**实现深拷贝  

#### 小案例

```java
//克隆引用对象
public class DeepCloneTarget implements Serializable,Cloneable {

    private static final long serialVersionUID = 1L;
    private String cloneName;
    private String cloneClass;

    public DeepCloneTarget(String cloneName, String cloneClass) {
        this.cloneName = cloneName;
        this.cloneClass = cloneClass;
    }

    //因为该类的属性都是string 所以直接用默认clone方式即可
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

```java
//要克隆的对象 实现了两种克隆方式
public class DeepProtoType implements Serializable,Cloneable {

    public String name;//String 属性
    public DeepCloneTarget deepCloneTarget;//引用类型 //属性是对象 默认克隆会浅拷贝
    //深拷贝-1 使用重写clone方法
    @Override
    protected Object clone() throws CloneNotSupportedException {
        Object deep = null;
        //这里完成对基本数据类型(属性)和字符串的clone
        deep = super.clone();
        //对引用类型的属性进行单独处理
        DeepProtoType deepProtoType = (DeepProtoType) deep;
        deepProtoType.deepCloneTarget = (DeepCloneTarget) deepCloneTarget.clone();
        return deepProtoType;
    }

    //深拷贝2 使用对象的序列化实现(推荐)

    public Object deepClone(){

        //创建流对象
        ByteArrayOutputStream bos = null;
        ObjectOutputStream oos = null;
        ByteArrayInputStream bis = null;
        ObjectInputStream ois = null;
        try {
            bos = new ByteArrayOutputStream();
            oos = new ObjectOutputStream(bos);
            oos.writeObject(this); //当前这个对象以对象流的方式输出 序列化
            //再反序列化 读出来 相当于存了值 成功克隆了一个新的对象
            bis = new ByteArrayInputStream(bos.toByteArray());
            ois = new ObjectInputStream(bis);
            DeepProtoType copy = (DeepProtoType) ois.readObject();
            return copy;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }finally {
            try {
                bos.close();
                oos.close();
                bis.close();
                ois.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

```java
//测试
public class Client {
    public static void main(String[] args) throws CloneNotSupportedException {
        DeepProtoType dp = new DeepProtoType();
        dp.name = "小水牛";
        dp.deepCloneTarget = new DeepCloneTarget("大水牛", "敲能喝");
        //方式一 完成深拷贝
       /*
        DeepProtoType clone = (DeepProtoType) dp.clone();
        System.out.println(dp.hashCode());//460141958
        System.out.println(clone.hashCode());//1163157884
       */
       //方式二 完成深拷贝
        DeepProtoType p = (DeepProtoType) dp.deepClone();
        System.out.println("克隆原型："+dp.name + "--" + dp.deepCloneTarget.hashCode());
        //克隆原型：小水牛--325040804

        System.out.println("克隆的对象"+p.name + "--" + p.deepCloneTarget.hashCode());
        //克隆的对象小水牛--2065951873
    }
}
//推荐使用的是通过序列化实现深拷贝，这样无论原型如何修改都不会影响克隆 因为直接通过流来反序列化创建
```

#### 小结

1. 创建新的对象比较复杂时，可以利用原型模式简化对象的创建过程，同时也能够提高效率
2. 不用重新初始化对象，而是动态的获得对象运行时的状态
3. 如果原型对象发生变化（增加或减少属性），其他克隆对象也会发生相应的变化，无需修改代码
4. 在实现深克隆的时候 可能需要比较复杂的代码
5. 缺点：需要为每一个类配备一个克隆方法，这对全新的类来说不是很难，但是对已有的类来说进行改造的时候 需要修改它的源代码，违背了`OCP`（开闭原则）。

### 建造者模式

1. 需求：需要建房子 这一过程为打桩 砌墙 封顶
2. 房子有各种各种的 比如普通房 高楼 别墅 各种房子的过程虽然一样 但是要求不要相同
3. 编写程序完成需求

#### 案例分析

```java
public abstract class AbstractHouse {  //抽象房子类
    public abstract void buildBasic();// 打地基
    public abstract void buildWalls(); //砌墙
    public abstract void roofed();//封顶
    public void build(){ //造房子
        buildBasic();
        buildWalls();
        roofed();
    }
}
//普通房子类
public class CommonHouse extends AbstractHouse {
    @Override
    public void buildBasic() {
        System.out.println("给普通房子打地基");
    }

    @Override
    public void buildWalls() {
        System.out.println("给普通房子砌墙");
    }
    @Override
    public void roofed() {
        System.out.println("给普通房子封顶");
    }
}



public class Client {
    public static void main(String[] args) {
        CommonHouse commonHouse = new CommonHouse();
        commonHouse.build();
    }
}

```

![java](TraditionalBuilder.jpg)

1. 有点是比较好理解，简单易操作

2. 设计的程序结构过于简单，没有设计缓存层对象，程序的扩展和维护不好,也就是说 这种设计方案把产品（房子）和创建产品的过程（建房子build方法）封装在一起 ，耦合性增强了

3. 解决方案：将**产品**和**产品建造过程**解耦 => 建造者模式

   

#### 基本介绍

1. 建造者模式(Builder Pattern)又叫生成器模式，是一种对象构建模式。它可以将复杂对象的建造过程抽象出来（抽象类别） 使这个抽象过程的不同方法可以构造出不同表现(属性)的对象。
2. 建造者模式是一步一步创建一个复杂的对象 ，他允许用户指定复杂的类型就可以构建他们，用户不需要知道具体构建细节。

#### 建造者模式的四个角色

1. Product(产品角色) ：一个具体的产品对象
2. Builder(抽象建造者)：创建一个Product对象的各个部件的接口/抽象类
3. `ConcreteBuilder`(具体建造者)实现接口，构建和装配各个部件
4. Director(指挥者)：构建一个Builder接口的对象，它主要是用于创建一个复杂的对象。主要有两个作用，一是：隔离客户与对象产生的过程，二是：复杂控制产品对象的生产过程

产品：

```java
//产品
public class House {//对应产品
    private String base;
    private String wall;
    private String roofed;
//对应getset方法
}
```

抽象的建造者Builder:

```java
public abstract class HouseBuilder {

    protected House house = new House(); //组合一个产品
    //将建造的流程写好,抽象方法
    public abstract void buildBasic();
    public abstract void buildWalls();
    public abstract void roofed();

    //建造房子 建好后将房子返回
    public House BuildHouse(){
        return house;
    }
}
```

指挥者Director

```java
//指挥者 这里去指定制作流程 返回房子
public class HouseDirector {
    HouseBuilder houseBuilder = null;
    //1.构造器传入  依赖
    public HouseDirector(HouseBuilder houseBuilder) {
        this.houseBuilder = houseBuilder;
    }
    //2.通过set方法传入 聚合
    public void setHouseBuilder(HouseBuilder houseBuilder) {
        this.houseBuilder = houseBuilder;
    }
    //如何处理建房流程，交给指挥者
    public House constructorHouse(){
        houseBuilder.buildBasic();
        houseBuilder.buildWalls();
        houseBuilder.roofed();
        return houseBuilder.BuildHouse();
    }
}
```

两个产品类：

```java
//普通房子
public class CommonHouse extends HouseBuilder{
    @Override
    public void buildBasic() {
        System.out.println("普通房子打地基五米");
    }
    @Override
    public void buildWalls() {
        System.out.println("普通房子砌墙10cm");
    }
    @Override
    public void roofed() {
        System.out.println("普通房子封顶");
    }
}
//高楼
public class HighBuilding extends HouseBuilder{
    @Override
    public void buildBasic() {
        System.out.println("高楼打地基50");
    }
    @Override
    public void buildWalls() {
        System.out.println("高楼砌墙20cm");
    }
    @Override
    public void roofed() {
        System.out.println("高楼透明屋顶");
    }
}
```

实现：

```java
public class Client {
    public static void main(String[] args) {
        //盖普通房子
        CommonHouse commonHouse = new CommonHouse();
        //准备创建房子的指挥者
        HouseDirector houseDirector = new HouseDirector(commonHouse);
        //完成盖房子，返回产品（房子）
        House house = houseDirector.constructorHouse();

        //盖高楼
        HighBuilding highBuilding = new HighBuilding();
        houseDirector.setHouseBuilder(highBuilding);
        House house1 = houseDirector.constructorHouse();
    }
}
```

![java](improveBuilder-1637309987736.jpg)

`JDK`中用到的建造者模式 `java.lang.StringBuilder`

- `Appendable`接口定义了多个append方法（抽象方法） ， 即`Appendable`为建造者，定义了抽象方法
- `AbstractStringBuilder`已经是建造者，只是不能实例化
- `StringBuilder`即充当了指挥者角色，同时同时充当了具体的建造者。建造方法的实现是由`AbstractStringBuilder`完成，而`StringBuilder`继承了`AbstractStringBuilder`

**建造者模式的注意事项和细节**

1. 客户端（使用程序）不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象。

2. 建造者模式所创建的产品一般具有较多相同的共同点，其组成部分相似，如果产品之间的差异性很大，则不是和使用建造者模式，因此其使用范围收到一定的限制。

3. 如果产品内部变化复杂，可能会导致需要定义很多具体建造者来实现这种变化，导致系统变得庞大，因此要考虑是否适合选择建造者模式。

4. 每一个具体的建造者都相对独立，而与其他的具体建造者无关，因此可以很方便的替换具体建造者或者增加新的具体建造者，用户使用不同的建造者即可得到不同的产品对象。

5. 可以更加精细的控制产品的建造过程，将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰，也更方便使用程序来控制创建流程。

6. 增加新的具体创建者无需修改原油类库的代码，指挥者针对抽象建造者类变成，系统扩展方便，符合开闭原则。

   >**抽象工厂模式和建造者模式**

   抽象工厂模式实现对产品家族的创建，一个产品家族是一系列产品，具有不同分类维度的产品组合，采用抽象工厂模式不需要关心构建过程，只关心什么产品由什么工厂生产。而建造者模式则是按照指定的蓝图构建产品，它的主要目的是通过组装零配件而生产一个新的产品。



##  结构型模式

### 适配器模式

基本介绍

1. 适配器模式(`Adapter Pattern`)将某个类的接口转换成客户端期望的另一个接口表示，主要目的是兼容性，让原本因接口不匹配不能一起工作的两个类可以协同工作。其别名为包装器(Wrapper)
2. 适配器模式属于结构型模式
3. 主要分为三类:类适配器模式，对象适配器模式  ，接口适配器模式   

工作原理

- 适配器模式：将一个类的接口转换成另一种接口，让原本接口不兼容的类可以兼容
- 从用户的角度看不到适配者，是解耦的。
- 用户调用适配器转换出来的目标接口方法，适配器再调用被适配者的相关接口方法
- 用户收到反馈结果，只感觉是和目标接口交互

#### 类适配器模式

```java
/**
 * @description:被适配的类 src 提供方、
 **/
public class Voltage220V {
    public int output220V(){
        int src = 220;
        System.out.println("电压=" + src + "V");
        return src;
    }
}
```

```java
//充电接口
public interface Voltage5V {
    public int output5V();
}
```

```java
/**
 * @description:适配器类
 * 继承被适配的类 实现适配器类的接口
 **/
public class VoltageAdapter extends Voltage220V implements Voltage5V{
    @Override
    public int output5V() {
        int srcV  = output220V();
        int destV = srcV/44;
        return destV;
    }
}
```

```java
/**
 * @description：手机 有充电方法 依赖了一个接口
 **/
public class Phone {
    //充电方法
    public void charging (Voltage5V iVoltage){
        if (iVoltage.output5V() == 5){
            System.out.println("电压为5V，可冲");
        }else if (iVoltage.output5V() > 5){
            System.out.println("电压大于5V 无法充电");
        }
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        System.out.println("====类适配器模式");
        Phone phone = new Phone();
        phone.charging(new VoltageAdapter());
    }
}
```

类适配器模式注意事项和细节

1. Java是单继承机制 ，所以类适配器需要继承`src`这一点算是一个缺点，因为这要求`dst`必须是接口，有一定的局限性
2. `src`类的方法在Adapter中都会暴露出来，增加了使用成本
3. 由于其继承了`src`类，所以它可以根据需求重写`src`类的方法，使得Adapter的灵活性增强了\

#### 对象适配器

基本介绍

1. 基本思路和类的适配器相同，只是将Adapter类做修改，不是继承`src`类，而是持有`src`的类型，以解决兼容问题。即：持有`src`类，实现`dst`类接口，完成`src`->`dst`的适配。
2. 根据“合成复用原则”，在系统中尽量使用关联关系来代替继承关系。
3. 对象适配器模式是适配器模式常用的一种

```java
/**
 * @description:被适配的类
 **/
public class Voltage220V {
    //提供220电压
    public int output220V(){
        int src = 220;
        System.out.println("电压=" + src + "V");
        return src;
    }
}
```

```java
//适配接口
public interface Voltage5V {
    public int output5V();
}
```

```java
/**
 * @description:适配器类
 * 继承被适配的类 实现适配器类的接口
 **/
public class VoltageAdapter  implements Voltage5V {
    private Voltage220V voltage220;
    //通过构造器 传入一个Voltage220V 实例
    public VoltageAdapter(Voltage220V voltage220){
        this.voltage220 = voltage220;
    }
    @Override
    public int output5V() {
        int destV = 0;
        if (null != voltage220){
            int src = voltage220.output220V();//获取220V电压
            System.out.println("适配器适配");
            destV = src /44;
            System.out.println("适配完成，输出的电压为" + destV);
        }
        return destV;
    }
}
```

```java
/**
 * @description：手机 有充电方法 依赖了一个接口
 **/
public class Phone {
    //充电方法
    public void charging (Voltage5V iVoltage){
        if (iVoltage.output5V() == 5){
            System.out.println("电压为5V，可冲");
        }else if (iVoltage.output5V() > 5){
            System.out.println("电压大于5V 无法充电");
        }
    }
}
```

```java
public class Client {
    public static void main(String[] args) {
        System.out.println("====对象适配器模式");
        Phone phone = new Phone();
        phone.charging(new VoltageAdapter(new Voltage220V() ));
    }
}
```

对象适配器模式注意事项和细节

1. 对象适配器和类适配器算是同一种思想，只不过是实现方式不同。根据合成服用原则，使用组合替代继承，所以他解决了适配器必须继承`src`的局限性问题，也不再要修`dst`必须是接口。
2. 使用成本更低，更灵活

#### 接口适配器模式

基本介绍

1. 一些书籍称为：适配器模式（Default Adapter Pattern）或缺省适配器模式。
2. 当不需要全部实现接口提供的方法时，可以设计一个抽象类实现接口，并为该接口中每一个方法提供一个**默认实现（空方法）**，那么该抽象类的子类可有选择的覆盖父类的某些方法实现需求。
3. 适用于一个接口不想使用其所有的方法的情况。

#### 源码分析

- `SpringMVC`中的`HandlerAdapter`就使用到了适配器模式 **很牛逼，但是现在看不懂，记得回来看~**

#### 小结

1. 三种命名方式，是根据`src`以怎样的形式给到Adapter(在`Adapte`里的形式)来命名的。
2. 三种适配器
   - 类适配器：以类给到，在Adapter里，就是将`src`当做类继承
   - 对象适配器：以对象给到，在Adapter里，将`src`作为一个对象持有
   - 接口适配器：以接口给到，在Adapter里，将`src`作为一个接口，实现
3. Adapter模式最大的作用还是将原本不兼容的接口融合在一起工作。
4. 实际开发中 实现不拘泥于这三种经典形式

### 桥接模式

问题分析：类爆炸问题。

#### 基本介绍

- 桥接模式(Bridge)：将实现与抽象类在两个不同的类层次中，使两个层次可以独立改变
- Bridge基于类的最小设计原则，通过使用继承，聚合，封装等不同方式来让不同的类承担不同的职责。它的主要特点是把抽象(Abstraction)和实现(Implementation)分离开，从而可以保证各个部分的独立性以及对他们的功能扩展

代码

```java
//接口
public interface Brand {
	void open();
	void close();
	void call();
}
//Vivo实现类
public class Vivo implements Brand {
	@Override
	public void open() {
		System.out.println(" Vivo手机开机 ");
	}
	@Override
	public void close() {
		System.out.println(" Vivo手机关机 ");
	}
	@Override
	public void call() {
		System.out.println(" Vivo手机打电话 ");
	}
}
//XiaoMi实现类
public class XiaoMi implements Brand {
	@Override
	public void open() {
		System.out.println(" 小米手机开机 ");
	}
	@Override
	public void close() {
		System.out.println(" 小米手机关机 ");
	}
	@Override
	public void call() {
		System.out.println(" 小米手机打电话 ");
	}
}
```

```java
//抽象成 桥
public abstract class Phone {
	//组合品牌
	private Brand brand;
	//构造器
	public Phone(Brand brand) {this.brand = brand;}

	protected void open() {
		this.brand.open();
	}
	protected void close() {
		brand.close();
	}
	protected void call() {
		brand.call();
	}
}
//两个抽象子类
public class UpRightPhone extends Phone {
	//构造器
	public UpRightPhone(Brand brand) {
		super(brand);
	}
	public void open() {
		super.open();
		System.out.println(" 直立样式手机 ");
	}

	public void close() {
		super.close();
		System.out.println(" 直立样式手机 ");
	}

	public void call() {
		super.call();
		System.out.println(" 直立样式手机 ");
	}
}
//折叠式手机类，继承 抽象类 Phone
public class FoldedPhone extends Phone {
	//构造器
	public FoldedPhone(Brand brand) {
		super(brand);
	}
	public void open() {
		super.open();
		System.out.println(" 折叠样式手机 ");
	}
	public void close() {
		super.close();
		System.out.println(" 折叠样式手机 ");
	}
	public void call() {
		super.call();
		System.out.println(" 折叠样式手机 ");
	}
}
```

```java
//客户端实现方式
public class Client {

	public static void main(String[] args) {
		//获取折叠式手机 (样式 + 品牌 )
		Phone phone1 = new FoldedPhone(new XiaoMi());
		phone1.open();
		phone1.call();
		phone1.close();

		Phone phone2 = new FoldedPhone(new Vivo());
		phone2.open();
		phone2.call();
		phone2.close();

		UpRightPhone phone3 = new UpRightPhone(new XiaoMi());
		phone3.open();
		phone3.call();
		phone3.close();

		UpRightPhone phone4 = new UpRightPhone(new Vivo());
		phone4.open();
		phone4.call();
		phone4.close();
	}

}
```



#### 源码分析

1. `JDBC`的Driver接口，从桥接模式来看，Driver就是一个接口，下面可以有`Mysql`的Driver，也可以有`Orical`的Driver，这些就可以当做实现接口类

![java](JDBCqiaojie.jpg)

#### 小结

1. 实现了抽象和实现类的分离，极大地提升了系统的灵活性，让抽象类和实现类部分独立开，有助于系统实现分层设计，从而产生更好的结构化系统。
2. 对于系统的高层部分，只需要知道抽象部分和实现部分的接口即可，其他的部分有具体业务来完成。
3. 桥接模式替代了多层继承方案，可以减少子类的个数，降低系统的管理和维护成本。
4. 桥接模式的引入增加了系统的理解和设计难度，由于聚和关联关系建立在抽象层，要求开发者针对抽象进行设计和编程。
5. 桥接模式要求正确识别出系统两个独立变化的维度，因此适用范围有一定的局限性。
6. 常见的应用场景:
   - `JDBC`驱动系统
   - 银行转账系统
     - 转账分类：网上转账，柜台转账，ATM转账
     - 转账用户类型：普通用户，银卡用户，金卡用户...
   - 消息管理
     - 消息类型：即使消息，延时消息..
     - 消息分类：短信，`QQ`，微信..

### 装饰者模式

>**定义**：**动态的将新功能附加到对象上**，在对象功能扩展方面，比继承更有弹性，也体现了开闭原则(`OCP`)

#### 案例分析

- 星巴克咖啡订单项目(咖啡馆)
- 咖啡种类/单品咖啡：咖啡种类/单品咖啡:Espresso(意大利浓咖啡)、`ShortBlack`、`LongBlack`(美式咖啡)、Decaf(无因咖啡)
- 调料:Milk、Soy(豆浆)、Chocolate
- 要求在扩展新的咖啡种类时，具有良好的扩展性、改动方便、维护方便
- 使用`OO`的来计算不同种类咖啡的费用:客户可以点单品咖啡，也可以单品咖啡+调料组合。

分析，一份单品咖啡可能有多份调料，而且单品咖啡和调料应该易于扩展，符合开闭原则。所以不能把单品咖啡和调料融合在一起，应该把咖啡当做**被装饰者**，调料当成**装饰者**，实现调料的具体子类为修饰着。图解图下

![java](decorator.jpg)

```java
//总抽象类Drink 被装饰者
public abstract class Drink {
	public String des; // 描述
	private float price = 0.0f;
	//...实现get 和 set方法
    
	//计算费用的抽象方法
	//子类来实现
	public abstract float cost();
}

//咖啡类 作为缓存层  旗下是单体咖啡实类
public class Coffee  extends Drink {
	@Override
	public float cost() {
		return super.getPrice();
	}
}
//两种咖啡种类
public class DeCaf extends Coffee {
	public DeCaf() {
		setDes(" 无因咖啡 "); //介绍
		setPrice(1.0f);		//咖啡价格
	}
}
public class Espresso extends Coffee {
	public Espresso() {
		setDes(" 意大利咖啡 ");
		setPrice(6.0f);
	}
}

```

```java
//Decorator装饰者
public class Decorator extends Drink {
	private Drink obj; //被装饰者
	public Decorator(Drink obj) { //组合被装饰者
		this.obj = obj;
	}
	@Override
	public float cost() {
		// getPrice 自己价格
		return super.getPrice() + obj.cost();
	}
	@Override
	public String getDes() {
		// obj.getDes() 输出被装饰者的信息
		return des + " " + getPrice() + " && " + obj.getDes();
	}
}
//具体的Decorator， 这里就是调味品
public class Milk extends Decorator {
	public Milk(Drink obj) {
		super(obj);
		setDes(" 牛奶 ");
		setPrice(2.0f); 
	}
}
public class Soy extends Decorator{
	public Soy(Drink obj) {
		super(obj);
		setDes(" 豆浆  ");
		setPrice(1.5f);
	}
}
public class Chocolate extends Decorator {
	public Chocolate(Drink obj) {
		super(obj);
		setDes(" 巧克力 ");
		setPrice(3.0f); // 调味品 的价格
	}
}
```

```java
//具体实现代码
public class CoffeeBar {
	public static void main(String[] args) {
		// 装饰者模式下的订单：2份巧克力+一份牛奶的LongBlack

		// 1. 点一份 LongBlack
		Drink order = new LongBlack();
		System.out.println("费用1=" + order.cost());
		System.out.println("描述=" + order.getDes());

		// 2. order 加入一份牛奶
		order = new Milk(order);
		System.out.println("order 加入一份牛奶 费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 描述 = " + order.getDes());

		// 3. order 加入一份巧克力
		order = new Chocolate(order);

		System.out.println("order 加入一份牛奶 加入一份巧克力  费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 加入一份巧克力 描述 = " + order.getDes());

		// 3. order 加入一份巧克力
		order = new Chocolate(order);

		System.out.println("order 加入一份牛奶 加入2份巧克力   费用 =" + order.cost());
		System.out.println("order 加入一份牛奶 加入2份巧克力 描述 = " + order.getDes());
		System.out.println("===========================");
		
		Drink order2 = new DeCaf();
		
		System.out.println("order2 无因咖啡  费用 =" + order2.cost());
		System.out.println("order2 无因咖啡 描述 = " + order2.getDes());
		
		order2 = new Milk(order2);
		
		System.out.println("order2 无因咖啡 加入一份牛奶  费用 =" + order2.cost());
		System.out.println("order2 无因咖啡 加入一份牛奶 描述 = " + order2.getDes());
	}

}
```



#### 源码分析

>Java的IO结构，`FilterInputStream`就是一个**装饰者**

![java](InputStream.jpg)

```java
public class DecoratorToJDKIO {
    public static void main(String[] args) throws Exception {
        /*说明
            1. InputStream是抽象类,类似我们前面讲的 Drink
            2.FileInputStream是 InputStream子类，类似我们前面的 DeCaf, LongBlack
            3.FilterInputStream是 InputStream子类:类似我们前面的Decorator修饰者
            4. DatalnputStream是 FilterInputStream子类，具体的修饰者，类似前面的 Milk, Soy 等
            5.FilterInputStream类有protected volatile InputStream in;即含被装饰者
            6.分析得出在jdk 的io体系中，就是使用装饰者模式
         */
        DataInputStream dis = new DataInputStream(new FileInputStream("/"));
        System.out.println(dis.read());
        dis.close();
    }
}
```



### 组合模式

#### 案例分析：

![java](composition.png)

传统方案解决问题分析

- 将学院看做了学校的子类，将专业看做了学院的子类，相当于从组织大小进行分成
- 但是这样不能很好的实现管理操作，比如增删改查。
- 解决方案，把学校，学院，专业当做组织结构，她们之间没有继承关系，而是一个树形结构，可以更好的实现管理操作，=> 组合模式

#### 基本介绍

1. 组合模式(Composite Patterm）)，又叫部分整体模式，它创建了对象组的树形结构，将对象组合成树状结构以表示“整体-部分”的层次关系。
2. 组合模式依据树形结构来组合对象，用来表示部分以及整体层次。
3. 组合模式使得用户对单个对象和组合对象的访问具有一致性，即:组合能让客户以一致的方式处理个别对象以及组合对象

#### 代码实现

```java
public abstract class OrganizationComponent {
	private String name; // 名字
	private String des; // 说明
	
	protected  void add(OrganizationComponent organizationComponent) {
		//默认实现
		throw new UnsupportedOperationException();//不支持操作异常
	}
	protected  void remove(OrganizationComponent organizationComponent) {
		//默认实现
		throw new UnsupportedOperationException();
	}
	//构造器
	public OrganizationComponent(String name, String des) {
		super();
		this.name = name;
		this.des = des;
	}
	//实现getset方法。。。。
	
	//方法print, 做成抽象的, 子类都需要实现
	protected abstract void print();
}
```

```java
//College
public class College extends OrganizationComponent {

	//List 中 存放的Department
	List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

	// 构造器
	public College(String name, String des) {
		super(name, des);
	}
	// 重写add
	@Override
	protected void add(OrganizationComponent organizationComponent) {
		//  将来实际业务中，Colleage 的 add 和  University add 不一定完全一样
		organizationComponents.add(organizationComponent);
	}
	// 重写remove
	@Override
	protected void remove(OrganizationComponent organizationComponent) {
		organizationComponents.remove(organizationComponent);
	}
	@Override
	public String getName() {
		return super.getName();
	}
	@Override
	public String getDes() {
		return super.getDes();
	}
	// print方法，就是输出University 包含的学院
	@Override
	protected void print() {
		System.out.println("--------------" + getName() + "--------------");
		//遍历 organizationComponents 
		for (OrganizationComponent organizationComponent : organizationComponents) {
			organizationComponent.print();
		}
	}
}

//University 就是 Composite , 可以管理College
public class University extends OrganizationComponent {

	List<OrganizationComponent> organizationComponents = new ArrayList<OrganizationComponent>();

	// 构造器
	public University(String name, String des) {
		super(name, des);
	}

	// 重写add
	@Override
	protected void add(OrganizationComponent organizationComponent) {
		organizationComponents.add(organizationComponent);
	}

	// 重写remove
	@Override
	protected void remove(OrganizationComponent organizationComponent) {
		organizationComponents.remove(organizationComponent);
	}

	@Override
	public String getName() {
		return super.getName();
	}

	@Override
	public String getDes() {
		return super.getDes();
	}

	// print方法，就是输出University 包含的学院
	@Override
	protected void print() {
		System.out.println("--------------" + getName() + "--------------");
		//遍历 organizationComponents 
		for (OrganizationComponent organizationComponent : organizationComponents) {
			organizationComponent.print();
		}
	}
}

//Department
public class Department extends OrganizationComponent {

	//没有集合
	public Department(String name, String des) {
		super(name, des);
	}
	//add , remove 就不用写了，因为他是叶子节点
	
	@Override
	public String getName() {
		return super.getName();
	}
	
	@Override
	public String getDes() {
		return super.getDes();
	}
	
	@Override
	protected void print() {
		System.out.println(getName());
	}
}
```

```java
//Client实现
public class Client {
	public static void main(String[] args) {
		//从大到小创建对象 学校
		OrganizationComponent university = new University("清华大学", " 中国顶级大学 ");
		
		//创建 学院
		OrganizationComponent computerCollege = new College("计算机学院", " 计算机学院 ");
		OrganizationComponent infoEngineercollege = new College("信息工程学院", " 信息工程学院 ");
		
		//创建各个学院下面的系(专业)
		computerCollege.add(new Department("软件工程", " 软件工程不错 "));
		computerCollege.add(new Department("网络工程", " 网络工程不错 "));
		computerCollege.add(new Department("计算机科学与技术", " 计算机科学与技术是老牌的专业 "));
		
		//
		infoEngineercollege.add(new Department("通信工程", " 通信工程不好学 "));
		infoEngineercollege.add(new Department("信息工程", " 信息工程好学 "));
		
		//将学院加入到 学校
		university.add(computerCollege);
		university.add(infoEngineercollege);
		
		//university.print();
		infoEngineercollege.print();
	}
}
```

注意事项：

注意事项和细节

1. 简化客户端操作，客户端只需要面对一致的对象而不用考虑整体部分或者叶子结点的问题。
2. 具有较强的扩展性，当我们要更改组合对象时，我们只需要调整内部的层次关系，客户端不用做出任何改动。
3. 方便创建出复杂的层次结构。客户端不用理会组合里面的组成细节，容易添加结点或者叶子从而创建出复杂的树形结构
4. 需要便利组织结构，或者处理的对象具有树形结构时，非常适合使用组合模式
5. 要求较高的抽象性， 如果结点和叶子具有很多差异性的话，比如很多方法和属性都不一样，不适合用组合模式



### 外观模式

#### 基本介绍

- 外观模式(Facade)，也叫“过程模式” : 外观模式为子系统的一组接口提供一个一致的界面，此模式定义了一个高层接口，这个接口使得这一子系统更加容易使用
- 外观模式通过定义一个一致的接口，用以屏蔽内部子系统的细节，使得调用端只需跟这个接口发生调用，而无需关系这个子系统的内部细节



原理类图说明(外观模式的角色)

1. 外观类(Facade):为调用端提供统一的调用接口，外观类知道哪些子类系统负责处理请求，从而将调用端的请求代理给适当的子系统对象
2. 调用者(Client):外观接口的调用者
3. 子系统的集合:指模块或者子系统，处理Facade对象指派的任务，他是功能的提供者

外观模式就是解决多个复杂接口带来的使用困难，起到简化用户使用的作用。

#### 源码分析

>`Mybatis`中的configuration中使用了外观模式，
>
>用户只需要传递一个Object，调用`metaObject`方法，而`getMetaObeject`方法中则组合了一系列`ObjectFactory`,一次判断返回的实例。相当于简化了用户的操作，在内部实现了一系列逻辑、
>
>当我们调用子系统变得很困难的时候，以更高一层的接口来调用子系统，来提高系统使用和复用。



外观模式的注意事项和细节

1. 外观模式对外屏蔽了子系统的细节，因此外观模式降低了客户端对子系统使用的复杂性
2. 外观模式对客户端与子系统的耦合关系，让子系统内部的模块更易维护和扩展
3. 通过合理使用外观模式，可以帮助我们更好的划分访问的层次
4. 当系统需要进行分层设计时，可以考虑使用Facade模式
5. 在维护一个遗留的大型系统时，可能这个系统已经变得非常难以维护和扩展，此时可以考虑为新系统开发一个Facade类，来提供遗留系统的比较清晰简单的接口，让新系统与Facade类交互，提高复用性
6. 不能过多的或者不合理的使用外观模式，使用外观模式好还是直接调用模块好。要以让系统有层次，利于维护为目的。

### 享元模式

#### 案例分析

小型的外包项目，给客户A做一个产品展示网站，客户A的朋友感觉效果不错，也希望做这样的产品展示网站，但是要求都有些不同:

- 
  有客户要求以新闻的形式发布
- 有客户人要求以博客的形式发布
- 有客户希望以微信公众号的形式发布

>1)需要的网站结构相似度很高，而且都不是高访问量网站，如果分成多个虚拟空间来处理,
>相当于一个相同网站
>的实例对象很多，造成服务器的资源浪费
>2)解决思路:整合到一个网站中，共享其相关的代码和数据，对于硬盘、内存、CPU、数据库空间等服务器资源
>都可以达成共享，减少服务器资源
>3)对于代码来说，由于是一份实例，维护和扩展都更加容易
>4)上面的解决思路就可以使用享元模式来解决

#### 基本介绍

1. 享元模式(Flyweight Pattern)也叫蝇量模式:运用共享技术有效地支持大量细粒度的对象

2. 常用于系统底层开发，解决系统的性能问题。像数据库连接池，里面都是创建好的连接对象，在这些连接对象中有我们需要的则直接拿来用，避免重新创建，如果没有我们需要的，则创建一个
3. 享元模式能够解决重复对象的内存浪费的问题，当系统中有大量相似对象，需要缓冲池时。不需总是创建新对象，可以从缓冲池里拿。这样可以降低系统内存，同时提高效率
4. 享元模式经典的应用场景就是池技术了，String常量池、数据库连接池、缓冲池等等都是享元模式的应用，享元模式是池技术的重要实现方式

![java](xiangyuan.jpg)

>对类图的说明
>对原理图的说明-即(模式的角色及职责)
>1) `FlyWeight`是抽象的享元角色，他是产品的抽象类,同时定义出对象的外部状态和内部状态(后面介绍)的接口或实现
>2) `ConcreteFlyWeight`是具体的享元角色，是具体的产品类，实现抽象角色定义相关业务
>3) `UnSharedConcreteFlyWeight`是不可共享的角色，一般不会出现在享元工厂。
>4) `FlyWeightFactory`享元工厂类，用于构建一个池容器(集合)，同时提供从池中获取对象方法

类似线程池，享元共享的数据是不可变的，可以共享资源。如果没有就创建，有就直接拿来用

```java
//用户
public class User { 
	private String name;
	public User(String name) {
		super();
		this.name = name;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}
//网站的抽象类
public abstract class WebSite {
	public abstract void use(User user);//抽象方法
}
```

```java
// 网站工厂类，根据需要返回压一个网站
public class WebSiteFactory {

	//集合， 充当池的作用
	private HashMap<String, ConcreteWebSite> pool = new HashMap<>();
	
	//根据网站的类型，返回一个网站, 如果没有就创建一个网站，并放入到池中,并返回
	public WebSite getWebSiteCategory(String type) {
		if(!pool.containsKey(type)) {
			//就创建一个网站，并放入到池中
			pool.put(type, new ConcreteWebSite(type));
		}
		
		return (WebSite)pool.get(type);
	}
	
	//获取网站分类的总数 (池中有多少个网站类型)
	public int getWebSiteCount() {
		return pool.size();
	}
}

//具体网站
public class ConcreteWebSite extends WebSite {
	//共享的部分，内部状态
	private String type = ""; //网站发布的形式(类型)
	//构造器
	public ConcreteWebSite(String type) {
		
		this.type = type;
	}
	@Override
	public void use(User user) {
		System.out.println("网站的发布形式为:" + type + " 在使用中 .. 使用者是" + user.getName());
	}
}
```

```java
//Client 客户端
public class Client {
	public static void main(String[] args) {
		// 创建一个工厂类
		WebSiteFactory factory = new WebSiteFactory();
		// 客户要一个以新闻形式发布的网站
		WebSite webSite1 = factory.getWebSiteCategory("新闻");
		
		webSite1.use(new User("tom"));
		// 客户要一个以博客形式发布的网站
		WebSite webSite2 = factory.getWebSiteCategory("博客");
		webSite2.use(new User("jack"));
		// 客户要一个以博客形式发布的网站
		WebSite webSite3 = factory.getWebSiteCategory("博客");
		webSite3.use(new User("smith"));

		// 客户要一个以博客形式发布的网站
		WebSite webSite4 = factory.getWebSiteCategory("博客");
		webSite4.use(new User("king"));
		System.out.println("网站的分类共=" + factory.getWebSiteCount());
	}
}
```



#### 源码分析

```java
public class FlyWeight {
	public static void main(String[] args) {
		//如果 Integer.valueOf(x) x 在  -128 --- 127 直接，就是使用享元模式返回,如果不在
		//范围类，则仍然 new 
		//小结:
		//1. 在valueOf 方法中，先判断值是否在 IntegerCache 中，
         //如果不在，就创建新的Integer(new), 否则，就直接从 缓存池返回
		//2. valueOf 方法，就使用到享元模式
		//3. 如果使用valueOf 方法得到一个Integer 实例，范围在 -128 - 127 ，执行速度比 new 快

		Integer x = Integer.valueOf(127); // 得到 x实例，类型 Integer
		Integer y = new Integer(127); // 得到 y 实例，类型 Integer
		Integer z = Integer.valueOf(127);//..
		Integer w = new Integer(127);
		
		System.out.println(x.equals(y)); // 大小，true
		System.out.println(x == y ); //  false
		System.out.println(x == z ); // true
		System.out.println(w == x ); // false
		System.out.println(w == y ); // false
		
		Integer x1 = Integer.valueOf(200);
		Integer x2 = Integer.valueOf(200);
		System.out.println("x1==x2" + (x1 == x2)); // false
	}
}
```

#### 小结：

1. 享元模式中的享可以理解为共享，元理解为对象，就是共享对象模式。比如共享连接池
2. 系统中有大量对象，这些对象消耗大量内存，并且对象的状态大部分可以外部化时，我们就可以考虑选用享元模式
3. 用唯一标识码判断，如果内存中有，则返回这个唯一标识码所标识的对象，用`HashMap/HashTable`存储
4. 享元模式大大的减少了对象的创建，降低了程序内存的占用，提高效率。
5. 享元模式提高了系统的复杂度，因为需要分离出内部状态和外部状态，而外部状态具有固化特性，不应该随着内部状态的改变而改变，这点在使用享元模式需要注意。
6. 使用享元模式时，注意划分内部状态和外部状态，并且需要有一个工厂类加以控制，
7. 享元模式经典的应用场景是需要缓冲池，比如String常量池，数据库连接池，以及围棋观赛黑白子对象。

## 行为型模式

### 模板模式

#### 案例分析

制作豆浆

#### 基本介绍

1. 模板方法模式(Template  Method Pattern) ,在一个抽象类公开了执行它的方法的模板。它的子类可以按需重写方法实现，但调用将以抽象类中定义的方法执行。
2. 简单说模板方法模式定义一个操作中的算法骨架，而将一些步骤延迟到子类中，使得子类可以不改变一个算法的结构就可以重定义该算法的某些步骤。

原理类图说明

- `AbstractClass` 抽象类，类中实现了模板方法，定义了算法的骨架，具体子类需要去实现 其他的抽象方法
- `ConcreateClass` 实现抽象方法，已完成算法中特点子类的步骤

```java
//抽象类，表示豆浆
public abstract class SoyaMilk {

	//模板方法, make , 模板方法可以做成final , 不让子类去覆盖.
	final void make() {
		select(); 
		if(customerWantCondiments()) { //判断是实现默认方法还是子类重写的方法
			addCondiments();
		}
		soak();
		beat();
	}
	
	//选材料
	void select() {
		System.out.println("第一步：选择好的新鲜黄豆  ");
	}
	
	//添加不同的配料， 抽象方法, 子类具体实现
	abstract void addCondiments();
	
	//浸泡
	void soak() {
		System.out.println("第三步， 黄豆和配料开始浸泡， 需要3小时 ");
	}
	 
	void beat() {
		System.out.println("第四步：黄豆和配料放到豆浆机去打碎  ");
	}
	
	//钩子方法，决定是否需要添加配料
	boolean customerWantCondiments() {
		return true;
	}
}


```

```java
public class RedBeanSoyaMilk extends SoyaMilk {
	@Override
	void addCondiments() {
		System.out.println(" 加入上好的红豆 ");
	}
}

public class PureSoyaMilk extends SoyaMilk {
	@Override
	void addCondiments() {
		//空实现
	}
	@Override
	boolean customerWantCondiments() {
		return false;
	}
}

public class PeanutSoyaMilk extends SoyaMilk {
	@Override
	void addCondiments() {
		System.out.println(" 加入上好的花生 ");
	}
}
```

```java
public class Client {

	public static void main(String[] args) {
		System.out.println("----制作红豆豆浆----");
		SoyaMilk redBeanSoyaMilk = new RedBeanSoyaMilk();
		redBeanSoyaMilk.make();
		
		System.out.println("----制作花生豆浆----");
		SoyaMilk peanutSoyaMilk = new PeanutSoyaMilk();
		peanutSoyaMilk.make();
		
		System.out.println("----制作纯豆浆----");
		SoyaMilk pureSoyaMilk = new PureSoyaMilk();
		pureSoyaMilk.make();
	}
}
```

在模板方法中添加一个钩子方法默认不做事，子类就可以根据狗子方法来决定是否覆盖这个方法。

这样就可以默认实现了

#### 源码分析

>Spring中 `IOC`容器初始化的时候 使用了模板模式
>
>
>
>

#### 小结： 

1. 基本思想是，算法只存在于一个地方，也就是在父类中，容易修改。修改算法时，只需要修改父类方法或者已经实现的某些步骤，子类就会继承这些修改。
2. 实现了最大化代码复用。父类的模板方法和已实现的某些步骤会被子类继承而直接使用。
3. 即统一了算法，也提供了很大的灵活性，父类的模板方法确保了算法结构保持不变，同时由子类提供部分步骤的实现。
4. 该模式不足之处是 每一个不同的实现都需要一个子类实现，容易导致类的个数增加导致系统庞大
5. 一般方法都加上关键字final，防止子类重写模板方法
6. 模板方法使用场景：当要完成在某个过程，该过程要执行一系列步骤，这一系列的步骤基本相同，单个别步骤在实现的时候可能不同，通常考虑用模板方法来处理。

### 命令模式

#### 案例需求

1)我们买了一套智能家电，有照明灯、风扇、冰箱、洗衣机，我们只要在手机上安装 `app`就可以控制对这些家电
工作。
2)这些智能家电来自不同的厂家，我们不想针对每一种家电都安装一个`App`，分别控制，我们希望只要一个`app`
就可以控制全部智能家电。
3)要实现一个`app`控制所有智能家电的需要，则每个智能家电厂家都要提供一个统一的接口给app调用，这时就
可以考虑使用命令模式。
4)命令模式可将“动作的请求者”从“动作的执行者”对象中解耦出来.
5)在我们的例子中，动作的请求者是手机 `app`，动作的执行者是每个厂商的一个家电产品

#### 基本介绍

- 命令模式(Command Pattern):在软件设计中，我们经常需要向某些对象发送请求，但是并不知道请求的接收
  者是谁，也不知道被请求的操作是哪个，
  我们只需在程序运行时指定具体的请求接收者即可，此时，可以使用命令模式来进行设计
- 命名模式使得请求发送者与请求接收者消除彼此之间的耦合，让对象之间的调用关系更加灵活，实现解耦。
- 在命名模式中，会将一个请求封装为一个对象，以便使用不同参数来表示不同的请求(即命名)，同时命令模式
  也支持可撤销的操作。
- 通俗易懂的理解:将军发布命令，士兵去执行。其中有几个角色:将军(命令发布者)、士兵(命令的具体执
  行者)、命令(连接将军和士兵)。
  `lInvoker`是调用者（将军)，Receiver是被调用者（士兵)，`MyCommand`是命令，实现了Command接口，持有接收对象

#### 原理类图分析



#### 代码实现

类图分析：

![java](command.png)

```java
//创建命令接口
public interface Command {
	//执行动作(操作)
	public void execute();
	//撤销动作(操作)
	public void undo();
}

//关灯命令
public class LightOffCommand implements Command {
	// 聚合LightReceiver
	LightReceiver light;

	// 构造器
	public LightOffCommand(LightReceiver light) {
			this.light = light;
		}
	@Override
	public void execute() {
		// 调用接收者的方法
		light.off();
	}
	@Override
	public void undo() {
		// 调用接收者的方法
		light.on();
	}
}

//开灯命令
public class LightOnCommand implements Command {
	//聚合LightReceiver
	LightReceiver light;
	
	//构造器
	public LightOnCommand(LightReceiver light) {
		this.light = light;
	}
	@Override
	public void execute() {
		//调用接收者的方法
		light.on();
	}
	@Override
	public void undo() {
		//调用接收者的方法
		light.off();
	}
}

/**
 * 没有任何命令，即空执行: 用于初始化每个按钮, 当调用空命令时，对象什么都不做
 * 其实，这样是一种设计模式, 可以省掉对空判断
 * @author Administrator
 *
 */
public class NoCommand implements Command {
	//只需要实现两个空方法 钩子
	@Override
	public void execute() {
	}

	@Override
	public void undo() {
	}
}

//任务接收者 执行者
public class LightReceiver {
	public void on() {
		System.out.println(" 电灯打开了.. ");
	}
	public void off() {
		System.out.println(" 电灯关闭了.. ");
	}
}
```

```java
//控制器 遥控器 Invoker 任务发布者
public class RemoteController {

	// 开 按钮的命令数组
	Command[] onCommands;
	Command[] offCommands;

	// 执行撤销的命令
	Command undoCommand;

	// 构造器，完成对按钮初始化
	public RemoteController() {
		onCommands = new Command[5];
		offCommands = new Command[5];

		for (int i = 0; i < 5; i++) {
			onCommands[i] = new NoCommand();
			offCommands[i] = new NoCommand();
		}
	}

	// 给我们的按钮设置你需要的命令
	public void setCommand(int no, Command onCommand, Command offCommand) {
		onCommands[no] = onCommand;
		offCommands[no] = offCommand;
	}

	// 按下开按钮
	public void onButtonWasPushed(int no) { // no 0
		// 找到你按下的开的按钮， 并调用对应方法
		onCommands[no].execute();
		// 记录这次的操作，用于撤销
		undoCommand = onCommands[no];

	}

	// 按下开按钮
	public void offButtonWasPushed(int no) { // no 0
		// 找到你按下的关的按钮， 并调用对应方法
		offCommands[no].execute();
		// 记录这次的操作，用于撤销
		undoCommand = offCommands[no];

	}
	
	// 按下撤销按钮
	public void undoButtonWasPushed() {
		undoCommand.undo();
	}

}
```

```java
//Client客户端
public class Client {

	public static void main(String[] args) {
		//使用命令设计模式，完成通过遥控器，对电灯的操作
		//创建电灯的对象(接受者)
		LightReceiver lightReceiver = new LightReceiver();
		
		//创建电灯相关的开关命令
		LightOnCommand lightOnCommand = new LightOnCommand(lightReceiver);
		LightOffCommand lightOffCommand = new LightOffCommand(lightReceiver);
		
		//需要一个遥控器
		RemoteController remoteController = new RemoteController();
		
		//给我们的遥控器设置命令, 比如 no = 0 是电灯的开和关的操作
		remoteController.setCommand(0, lightOnCommand, lightOffCommand);
		
		System.out.println("--------按下灯的开按钮-----------");
		remoteController.onButtonWasPushed(0);
		System.out.println("--------按下灯的关按钮-----------");
		remoteController.offButtonWasPushed(0);
		System.out.println("--------按下撤销按钮-----------");
		remoteController.undoButtonWasPushed();
		

		//如果后期需要扩展新的命令 就非常容易扩展 不需要修改Controller的内容 符合开闭原则
		/*
		System.out.println("=========使用遥控器操作电视机==========");
		TVReceiver tvReceiver = new TVReceiver();
		TVOffCommand tvOffCommand = new TVOffCommand(tvReceiver);
		TVOnCommand tvOnCommand = new TVOnCommand(tvReceiver);
		//给我们的遥控器设置命令, 比如 no = 1 是电视机的开和关的操作
		remoteController.setCommand(1, tvOnCommand, tvOffCommand);
		
		System.out.println("--------按下电视机的开按钮-----------");
		remoteController.onButtonWasPushed(1);
		System.out.println("--------按下电视机的关按钮-----------");
		remoteController.offButtonWasPushed(1);
		System.out.println("--------按下撤销按钮-----------");
		remoteController.undoButtonWasPushed();
		*/

	}
}
```

#### 源码分析

> Spring中的JDBCTemplate中使用了模板模式

#### 小结

>注意事项和细节

1. 将发起请求的对象与执行请求的对象解耦。发起请求的对象是调用者，调用者只要调用命令对象的execute()方法就可以让接收着工作，而不必知道记得接收着对象谁，如何实现的，命令对象会负责让接受者执行请求的动作，也就是“请求发起者”和“请求执行者”之间的解耦是通过命令对象实现的，命令对象起到了扭到桥梁的作用。
2. 容易设计一个命令队列，只要把命令对象放到队列，就可以实现多线程的命令
3. 容易实现对请求的撤销和重做
4. 命令模式的不足：可能导致某些系统有过多的具体命令类，增加了系统的复杂度，这点在使用的时候要注意
5. 空命令也是一种设计模式，它为我们省去了判空的操作，在上面的案例，如果没有用空命令，那么每一个按键我们都要做判空操作，对编码带来了一定的麻烦。
6. 命令模式经典的应用场景：界面的一个按钮都是一条命令，模拟CMD（DOS）命令，订单的撤销/恢复、触发反馈机制