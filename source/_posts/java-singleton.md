---
title: java设计模式-单例模式
date: 2021-12-06 14:48:08
categories: designMode
tags: "设计模式"
---
### 单例模式

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
    缺点是在类装载的时候就完成实例化，没有达到懒加载的效果(Lazy Loading)。
    如果从始至终从未使用过这个实例，则会造成内存浪费，
    这种基于类加载机制避免了多线程同步的问题，不过instance在类装载的时候就完成实例化，
    在单例模式中大多数都是调用getInstance方法，但是导致类装载的原因有很多种，
    因此不能确定有其他的方式导致类装载，这个时候初始化instance就没有达到懒加载的效果
    这种单例模式可以用，有可能造成内存浪	费
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
    //和静态常量类似，在静态代码块完成实例化。 优缺点也和饿汉式静态常量一样
}
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
    2.效率太低了，每个线程想获得类的实例的时候 执行getInstance()方法都要进行同步。
    而其实这个方法只执行一次实例化就够了，后面想要获得该实例应该是直接return，方法进行同步效率太低
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
    2.静态内部类方式在Singleton类被加载的时候并不会立即实例化，而是在需要实例化的时候，
    调用getInstacne方法，才会装在SingleInstance类，从而完成Singeleton的实例化
    3.类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们保证了线程的安全性，
    在类进行初始化的时候，别的线程是无法进入的。
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
        1.这借助JDK1.5中添加的枚举来实现单例模式。不仅能避免多线程同步问题，
        而且还能防止反序列化重新创建新的对象。
        2.这种方式是Effective Jaca坐着Josh Bloch提倡的方式
        推荐使用
*/
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