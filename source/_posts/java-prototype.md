---
title: java-原型模式
date: 2021-12-08 20:01:23
categories: designMode
tags: "设计模式"
---
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