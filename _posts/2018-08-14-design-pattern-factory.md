---
title: "设计模式之工厂模式"
categories:
  - Degsign Pattern
tags:
  - Degsign Pattern
---

工厂模式定义一个创建对象的接口，让其子类决定自己实例化哪一个工厂类，将实例的创建工厂延迟到子类中进行。
主要有三种工厂模式：
> 1. 简单工厂模式  
2. 工厂方法模式  
3. 抽象工厂模式  


下面就一一展开阐述。  

# 1， 简单工厂模式  
模式简单，主要应用在业务简单的情况下。  
简单工厂模式主要有三种角色：
> 1. 工厂类
2. 具体产品
3. 抽象产品接口  

![simpleFactory](/assets/images/tech/designpattern/simplefactory.png)

假如说我们需要生产汽车，奔驰，宝马和奥迪，使用简单工厂模式，我们只需要告诉工厂说我们想要造什么车，直接调用工厂的方法，就行了。
从上边的类图可以明白其关系。 


首先，我们创建一个接口Car类，也就是抽象接口类。  
```java
public interface Car {
	public void create() ;
}
```

然后，针对不同的车，创建实现接口的实体类，即具体产品类。
宝马车实体类：
```java
public class Bmw implements Car {

	@Override
	public void create() {
		System.out.println("BMW run!!!");
	}
}
```
奔驰车实体类：
```java
public class Benz implements Car {

	@Override
	public void create() {
		System.out.println("Benz run!!!");
	}
}
```
奥迪车实体类：
```java
public class Audi implements Car {

	@Override
	public void create() {
		System.out.print("Audi run!!!");
	}
}
```
最后，创建其工厂类，能够根据传入的信息，生成实体类的对象。
```java
public class CarSimpleFactory {
	public Car createCar(String car) throws Exception {
		switch (car) {
		case "BMW":
			return new Bmw();
		case "Benz":
			return new Benz();
		case "Audi":
			return new Audi();
		default:
			throw new Exception("no such car!");
		}
	}
}
```

写一个简单的demo，使用工厂类来实例化对象。
```java
public class CarSimpleFactroyDemo {
	public static void main(String[] argu) throws Exception {
		CarSimpleFactory carSimpleFactory = new CarSimpleFactory();
		carSimpleFactory.createCar("BMW").create();
		carSimpleFactory.createCar("Benz").create();
		carSimpleFactory.createCar("Audi").create();

	}
}
```

输出结果：  
> BMW run!!!  
> Benz run!!!  
> Audi run!!!     

通过简单工厂模式，可以让用户不接触实体类的情况下，完成实体的创建，
工厂模式的使用，虽然带来了较多的代码，而且使用了多态。但是却带来了很好的拓展性，假如说工厂如果可以生产丰田汽车，只需要在工厂内加入丰田车实体类即可。扩展性得到很大的提高。
