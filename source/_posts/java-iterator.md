---
title: java设计模式-迭代器模式
date: 2021-12-08 08:27:13
categories: designMode
tags: "设计模式"
---
### 迭代器模式

传统模式案例：一个学校有多个学院，每个学院下面又有各种专业，但是每个学院可能存储的数据不一样，可能是集合，可能是数组，可能是map等等等，如何做到用一个统一的接口遍历全部对象， 而不暴露内部细节，简化用户的使用。

#### 基本介绍: 

1. 迭代器模式(Iterator Pattern) 是常用的设计模式 ，属于行为型模式
2. 如果我们的集合元素是用不同的方式实现的，有数组，还有`java`的集合类，或者还有其他方式，当客户端要遍历这些集合元素的时候就要使用多种便利方式，而且还会暴露元素的内部结构，可以考虑使用迭代器模式解决。
3. 迭代器模式，提供一种遍历集合元素的统一接口，用一致的方法遍历集合元素，不需要知道集合对象的底层表示。即：不暴露其内部结构。

用例图：

![iterator01](iterator01.png)



类图说明：

- Iterator ： 迭代器接口，是系统提供，含义 `hasNext`， next  ， remove)
- `ConcreateIterator`：具体的迭代器类，管理迭代。
- Aggregate ：一个统一的聚合接口，将 客户端 和 具体聚合 解耦。
- `ConcreateAggreage`：具体的聚合 持有对象集合，并提供方法返回一个迭代器
- Client：通过Iterator和Aggregate 依赖子类

>编写一个学校院系结构，在一个页面中展示出学校院系组成，一个学校有多个学院，一个学院有多个系

#### 代码分析：

```java
//College接口
public interface College {
	//名字
	public String getName();
	
	//增加系的方法
	public void addDepartment(String name, String desc);
	
	//返回一个迭代器,遍历
	public Iterator  createIterator();
}
```

```java
//用来存储数据的统一模型
public class Department {

	private String name;
	private String desc;
	public Department(String name, String desc) {
		this.name = name;
		this.desc = desc;
	}
	public String getName() {return name;}
	public void setName(String name) {this.name = name;}
	public String getDesc() {return desc;}
	public void setDesc(String desc) {this.desc = desc;}
	
}
```

```java
//计算机学院具体类
public class ComputerCollege implements College {

	//模拟每个学院存储数据的方式集合不一样
	Department[] departments;
	int numOfDepartment = 0 ;// 保存当前数组的对象个数


	//初始化数据
	public ComputerCollege() {
		departments = new Department[5];
		addDepartment("Java专业", " Java专业 ");
		addDepartment("PHP专业", " PHP专业 ");
		addDepartment("大数据专业", " 大数据专业 ");
		
	}
	
	@Override
	public String getName() {
		return "计算机学院";
	}

	@Override
	public void addDepartment(String name, String desc) {
		Department department = new Department(name, desc);
		departments[numOfDepartment] = department;
		numOfDepartment += 1;
	}

	@Override
	public Iterator createIterator() {
		return new ComputerCollegeIterator(departments);
	}

}
```


```java
//计算机学院迭代器
public class ComputerCollegeIterator implements Iterator {
	//不同的迭代器实现不同的遍历方法

	//这里我们需要Department 是以怎样的方式存放=>数组
	Department[] departments;
	int position = 0; //遍历的位置
	

	public ComputerCollegeIterator(Department[] departments) {
		this.departments = departments;
	}

	//判断是否还有下一个元素
	@Override
	public boolean hasNext() {
		if(position >= departments.length || departments[position] == null) {
			return false;
		}else {
		
			return true;
		}
	}

	@Override
	public Object next() {
		Department department = departments[position];
		position += 1;
		return department;
	}
	
	//删除的方法，默认空实现
	public void remove() {
		
	}

}
```

```java
//信息工程学院具体实现类
public class InfoCollege implements College {

	//模拟每个学院存储数据的方式集合不一样
	List<Department> departmentList;
	
	//初始化数据
	public InfoCollege() {
		departmentList = new ArrayList<Department>();
		addDepartment("信息安全专业", " 信息安全专业 ");
		addDepartment("网络安全专业", " 网络安全专业 ");
		addDepartment("服务器安全专业", " 服务器安全专业 ");
	}
	
	@Override
	public String getName() {
		return "信息工程学院";
	}

	@Override
	public void addDepartment(String name, String desc) {
		Department department = new Department(name, desc);
		departmentList.add(department);
	}

	@Override
	public Iterator createIterator() {
		return new InfoCollegeIterator(departmentList);
	}
}
```

```java
//信息工程学院迭代器
public class InfoCollegeIterator implements Iterator {
	//不同的迭代器实现不同的遍历方法
	
	List<Department> departmentList; // 信息工程学院是以List方式存放系
	int index = -1;//索引
	

	public InfoCollegeIterator(List<Department> departmentList) {
		this.departmentList = departmentList;
	}

	//判断list中还有没有下一个元素
	@Override
	public boolean hasNext() {
		if(index >= departmentList.size() - 1) {
			return false;
		} else {
			index += 1;
			return true;
		}
	}

	@Override
	public Object next() {
		return departmentList.get(index);
	}
	
	//空实现remove
	public void remove() {
		
	}
}
```

```java
//统一实现打印遍历迭代
public class OutPutImpl {
	
	//学院集合
 	private List<College> collegeList;

	//有参构造实现聚合
	public OutPutImpl(List<College> collegeList) {
		this.collegeList = collegeList;
	}

	//遍历所有学院,然后调用printDepartment 输出各个学院的系
	public void printCollege() {
		//从collegeList 取出所有学院, Java 中的 List 已经实现Iterator
		Iterator<College> iterator = collegeList.iterator();
		
		while(iterator.hasNext()) {
			//取出一个学院
			College college = iterator.next();
			System.out.println("=== "+college.getName() +"===== " );
			//取出学院的迭代器执行迭代方法
			printDepartment(college.createIterator()); //得到对应迭代器
		}
	}

	//输出 学院输出 系
	public void printDepartment(Iterator iterator) {
		while(iterator.hasNext()) {
			Department d = (Department)iterator.next();
			System.out.println(d.getName());
		}
	}
}
```

```java
//客户端方法public class Client {
	public static void main(String[] args) {
		//创建学校(学院集合)
		List<College> collegeList = new ArrayList<College>();

		//创建计算机学院和大数据学院
		ComputerCollege computerCollege = new ComputerCollege();
		InfoCollege infoCollege = new InfoCollege();

		//添加计算机学院和大数据学院
		collegeList.add(computerCollege);
		collegeList.add(infoCollege);
		//统一放到打印实现类实现迭代器遍历
		OutPutImpl outPutImpl = new OutPutImpl(collegeList);
		outPutImpl.printCollege();
	}
}
```

#### 源码分析：

> `JDK`中的`ArrayList`集合中就使用到了迭代器模式
>
> List就是聚合接口 `ArrayList`就是聚合接口实现类 并且实现了一个特有的迭代器 Iteraror

![List](iterator02.png)



#### 注意事项和细节

1. 优点
   - 提供了一个统一的方法遍历对象，客户不再考虑聚合的类型，使用一种方法就可以遍历对象了。
   - 隐藏了聚合的内部结构，客户端要遍历聚合的时候只能取到迭代器，而不会知道聚合的具体组成
   - 提供了一种设计思想，就是一个类应该只有一个引起变化的原因(单一职责原则)。在聚合类中，我们吧迭代器分开，就是要把管理对象集合和遍历对象集合的责任分开，这样一来集合改变的话，只影响到集合对象。而如果便利方式改变的话，只影响到了迭代器。
   - 当腰展示一组相似对象，或者遍历一组相同对象同时使用，适合使用迭代器模式
2. 缺点
   - 每个聚合对象都要一个迭代器，会生成多个迭代器不好管理类