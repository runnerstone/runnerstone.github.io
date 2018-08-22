---
title: "设计模式之工厂模式"
categories:
  - Degsign Pattern
tags:
  - Degsign Pattern
---

# 工厂模式  
工厂模式让子类决定实例化哪一个类，并将类的实例化延迟到具体的子类中进行。  
工厂模式主要有4种角色：
> 1. 抽象工厂  
2. 具体工厂  
3. 抽象产品  
4. 具体产品  

![factory](/assets/images/tech/designpattern/factoryPattern.png)

仍然采用上边的例子，首先，我们有一个抽象产品的类。
```java
#抽象产品角色
public interface Car
{
    public void run();

}
```
针对不同的车型，我们需要实现具体产品，也即是具体产品类。  s
宝马车实体类：
```java
public class Bmw implements Car
{
    public Bmw() {
        System.out.println("BMW Created!!!");
    }
    @Override
    public void run()
    {
        System.out.println("BMW Run!!!");
    }
}
```
奥迪车实体类：
```java
public class Audi implements Car
{
    public Audi()
    {
        System.out.println("Audi Created!!!");
    }

    @Override
    public void run()
    {
        System.out.println("Audi Run!!!");
    }
}
```
奔驰车实体类：
```java
public class Benz implements Car
{
    public Benz()
    {
        System.out.println("Benz Created!!!");
    }

    @Override
    public void run()
    {
        System.out.println("Benz Run!!!");
    }
}
```
然后，我们需要创建一个抽象工厂类，CarFactory。
```java
public interface CarFactory
{
    public Car createCar();

}
```
接着，针对不同类型的Car来创建具体工厂，该工厂实现了抽象工厂的接口。
宝马工厂类  
```java
public class BmwFactory implements CarFactory
{

    @Override
    public Car createCar()
    {
        return new Bmw();
    }
}
```
奥迪工厂类
```java
public class AudiFactory implements CarFactory
{

    @Override
    public Car createCar()
    {
        return new Audi();
    }

}
```
奔驰工厂类
```java
public class BenzFactory implements CarFactory
{

    @Override
    public Car createCar()
    {
        return new Benz();
    }

}
```
写一个简单的demo,来展示如何使用工厂模式。
```java
public class CarFactoryDemo {
	public static void main(String[] args) {

		BenzFactory benzFactory = new BenzFactory();
		benzFactory.createCar().run();
		BmwFactory bmwFactory = new BmwFactory();
		bmwFactory.createCar().run();
		AudiFactory audiFactory = new AudiFactory();
		audiFactory.createCar().run();
	}
}
```

输出结果：  
> Benz Created!!!  
> Benz Run!!!  
> BMW Created!!!  
> BMW Run!!!  
> Audi Created!!!  
> Audi Run!!!    
  
工厂模式具有如下优缺点:  

优点：  

> 1. 当用户创建需要的产品时，只需要调用相应的工厂，用户只需要关注工厂即可，不需要关注实例化细节。  
> 2. 工厂模式的关键是多态，基于工厂角色和产品角色的多态设计。其让工厂自主决定实例化的对象。（工厂模式又称为多态工厂模式，是因为具体的工厂类具有同一个抽象父类。）
> 3. 当加入新的产品时，只需要新增加一个产品类和产品工厂，不需要修改原来的抽象工厂和抽象产品，因此，拓展性极好，完全符合“开闭原则”。 
   
缺点：    
  
> 1. 添加新产品时，需要添加工厂类和产品类，增加系统的复杂度，带来了新的开销。  
> 2. 为了系统的可拓展性，引入了抽象层，可能还需要用到DOM和反射等技术，降低了系统的可读性，增加了系统的抽象程度。