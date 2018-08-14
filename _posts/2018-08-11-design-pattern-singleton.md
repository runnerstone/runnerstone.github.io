---
title: "设计模式之单例模式"
categories:
  - Degsign Pattern
tags:
  - Degsign Pattern
---

单例模式是为了只创建一个实例。
具有以下特点：
> 单例只能有一个实例  
> 单例必须创建自己唯一的实例  
> 单例必须向其他对象提供这一实例  

![singleton](/assets/images/tech/designpattern/singleton.png)

常见实现方式：  

1. 懒汉模式  
懒汉模式当需要实例时才会创建
```java  
public class LSingleton {
	private static LSingleton instance;
	private LSingleton() {	}
	public static LSingleton getInstance() {
	 if (instance == null) {
	 instance = new LSingleton();
	 }
	 return instance;
	 }
}
```
缺点：线程不安全，当多个线程同时访问时，会出现多个对象。

2. 线程安全懒汉模式  
针对上述的问题，为了保证并发情况下的线程安全，在调用getInstance()时加锁，从而保证线程的安全。  
```java
public class LSingleton {
	private static LSingleton instance;

	private LSingleton() {	}
	public static synchronized LSingleton getInstance() {
		if (instance == null) {
			instance = new LSingleton();
		}
		return instance;
	}
}
```
缺点：并发是特殊的情况，此方法会带来资源的额外占用，效率较低。

3. 饿汉模式  
初始化类时创建实例，创建成功后一直存在。 降低了内存的使用率。
```java
public class ESingleton {
	private static ESingleton instance = new ESingleton();

	private ESingleton() {

	}

	public static ESingleton getInstance() {
		return instance;
	}
}
```
缺点：没有lazy loading的效果。  
4. 静态内部类
定义一个内部私有类，只在getInstance()时被调用，满足了lazy loading的效果。
```java
public class SSingleton {
	private static class SSingletonHolder {
		private static final SSingleton instance = new SSingleton();
	}

	private SSingleton() {

	}

	public static final SSingleton getInstance() {
		return SSingletonHolder.instance;
	}
}
```
5. 枚举类
高级代码，写意风格。   
自由序列化，保证了只有一个实例，线程安全；effective java推荐使用。
```java
//effective java 推荐使用，使用较少, 调用方法EnumSingleton.Instance.doSomething()
public enum EnumSingleton {
	Instance;
	public void dosometing() {	}
}
```
6. 双重校验锁法  
线程安全，lazy loading
```java
public class LockSingleton {
    private volatile static LockSingleton instance;
    private LockSingleton(){
    }
    public static LockSingleton getInstance(){
        if(instance==null){
            synchronized (LockSingleton.class){
                if(instance==null){
                    instance=new LockSingleton();
                }
            }
        }
        return instance;
    }
}
```

