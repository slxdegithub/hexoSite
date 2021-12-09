---
title: java-构建者模式
date: 2021-12-08 20:02:50
categories: designMode
tags: "设计模式"
---
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

![TraditionalBuilder](TraditionalBuilder.jpg)

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

![improveBuilder](improveBuilder.jpg)

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

