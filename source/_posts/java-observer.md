---
title: java-观察者模式
date: 2021-12-08 19:53:38
categories: designMode
tags: "设计模式"
---
### 观察者模式

 天气预报需求：

1. 气象站可以将每天测量到的温度，湿度，气压等一公告的形式发布出去，比如发布到自己的网站或者第三方
2. 设计开放性API 便于其他第三方接入气象站获取数据
3. 每次更新的时候 就要实时通知其他的第三方更新。

#### 基本介绍

1. 观察者模式，对象之间多对一依赖的一种设计方案

![observe01](observe01.png)

#### 代码案例

```java
//观察者接口，有观察者来实现
public interface Observer {

	public void update(float temperature, float pressure, float humidity);
}
```

```java
//接口, 让WeatherData 来实现
public interface Subject {

	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}
```

```java
/**
 * 相当于气象站本身，包含了一个第三方的集合，可以增加和移除第三方接口，并且能够实时通知第三方更新
 * 类是核心
 * 1. 包含最新的天气情况信息 
 * 2. 含有 观察者集合，使用ArrayList管理
 * 3. 当数据有更新时，就主动的调用   ArrayList, 通知所有的（接入方）就看到最新的信息
 * @author Administrator
 *
 */
public class WeatherData implements Subject {
    //温度 气压 湿度
	private float temperatrue;
	private float pressure;
	private float humidity;
	//观察者集合
	private ArrayList<Observer> observers;
	
	//加入新的第三方

	public WeatherData() {
		observers = new ArrayList<Observer>();
	}

	public float getTemperature() {
		return temperatrue;
	}

	public float getPressure() {
		return pressure;
	}

	public float getHumidity() {
		return humidity;
	}

	public void dataChange() {
		//调用 接入方的 update
		notifyObservers(); //通知所有观察者更新
	}

	//当数据有更新时，就调用 setData
	public void setData(float temperature, float pressure, float humidity) {
		this.temperatrue = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		//调用dataChange， 将最新的信息 推送给 接入方 currentConditions
		dataChange();
	}

	//注册一个观察者
	@Override
	public void registerObserver(Observer o) {
		observers.add(o);
	}

	//移除一个观察者
	@Override
	public void removeObserver(Observer o) {
		if(observers.contains(o)) {
			observers.remove(o);
		}
	}

	//遍历所有的观察者，并通知更新
	@Override
	public void notifyObservers() {
		for(int i = 0; i < observers.size(); i++) {
			observers.get(i).update(this.temperatrue, this.pressure, this.humidity);
		}
	}
}
```

```java
//第三方网站 实现观察者接口 实现update方法
public class CurrentConditions implements Observer {

	// 温度，气压，湿度
	private float temperature;
	private float pressure;
	private float humidity;

	// 更新 天气情况，是由 WeatherData 来调用，我使用推送模式
	public void update(float temperature, float pressure, float humidity) {
		this.temperature = temperature;
		this.pressure = pressure;
		this.humidity = humidity;
		display();
	}

	// 显示
	public void display() {
		System.out.println("***Today mTemperature: " + temperature + "***");
		System.out.println("***Today mPressure: " + pressure + "***");
		System.out.println("***Today mHumidity: " + humidity + "***");
	}
}

//新的第三方 只需要实现Observe接口的update方法，就能够拿到数据对自己进行处理
```

```java
public class Client {
//客户端
	public static void main(String[] args) {
		//创建一个WeatherData
		WeatherData weatherData = new WeatherData();
		
		//创建观察者
		CurrentConditions currentConditions = new CurrentConditions();
		BaiduSite baiduSite = new BaiduSite();
		
		//注册到weatherData
		weatherData.registerObserver(currentConditions);
		weatherData.registerObserver(baiduSite);
		
		//测试
		System.out.println("通知各个注册的观察者, 看看信息");
		weatherData.setData(10f, 100f, 30.3f);
		
		
		weatherData.removeObserver(currentConditions);
		//测试
		System.out.println();
		System.out.println("通知各个注册的观察者, 看看信息");
		weatherData.setData(10f, 100f, 30.3f);
	}

}
```

#### 观察者模式的好处

- 观察者模式设计后，会以集合的方式来管理用户(Observer)，包括注册，移除和通知。
- 这样，我们增加观察者(可以理解为一个新的公告板) 就不需要去修改核心类`WeatherData`，不会修改代码，遵守了`OPC`原则。

源码分析

> JDK中的Observable类就使用了观察者模式
>
> Spring事件驱动模型就是观察者模式的一个经典应用

