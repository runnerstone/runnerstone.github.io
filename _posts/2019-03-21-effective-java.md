---
title: "Effective Java"
categories:
  - Java
tags:
  - Java
---
# Effective Java

## 第二章 创建和销毁对象
### 1，考虑用静态工厂方法代替构造器*
```java
public static Boolean valueOf(boolean b){
  return b ? Boolean.TRUE : Boolean.FALSE;
}
```
>优势：   
>1，它们有名称，易于阅读。当一个类需要多个相同签名的构造器时，可以用静态工厂方法替代构造器。
>
>2，不需要每次调用的时候都创建一个对象，实例受控的类(instance-controlled)
>
>3, 它们可以返回类型的任何子类型的对象，常见应用于API可以返回对象，同时又不会将对象的类变成公有的。多用于基于接口的框架中
>
>4，在创建参数化实例的时候，可以使得代码变得简洁

>缺点：
>
>1，类如果不含有共有的或者受保护的构造器，就不能被子类化
>
>2，它们与其他的静态方法实际上没有任何区别


### 2, 遇到多个构造器参数时要考虑用构造器
```java
public class NutritionFacts {
	private final int servingSize;
	private final int servings;
	private final int calories;
	private final int fat;
	private final int sodium;
	private final int carbohydrate;

	public static class Builder {
		private final int servingSize;
		private final int servings;

		private int calories = 0;
		private int fat = 0;
		private int carbohydrate = 0;
		private int sodium = 0;

		public Builder(int servingSize, int servings) {
			this.servingSize = servingSize;
			this.servings = servings;
		}

		public Builder calories(int val) {
			calories = val;
			return this;
		}

		public Builder fat(int val) {
			fat = val;
			return this;
		}

		public Builder carbohydrate(int val) {
			carbohydrate = val;
			return this;
		}

		public Builder sodium(int val) {
			sodium = val;
			return this;
		}

		public NutritionFacts build() {
			return new NutritionFacts(this);
		}
	}

	private NutritionFacts(Builder builder) {
		servingSize = builder.servingSize;
		servings = builder.servings;
		calories = builder.calories;
		fat = builder.fat;
		sodium = builder.sodium;
		carbohydrate = builder.carbohydrate;
	}

	public static void main(String[] args) {
		NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
				.calories(100).sodium(35).carbohydrate(27).build();
	}
```

### 3, 用私有构造器或者枚举类型强化Singleton属性
### 4，通过私有构造器强化不可实例化的能力
### 5，避免创建不必要的对象
### 6，消除过期的对象引用
### 7，避免使用终结方法
>终结方法，finalizer   
>缺点：   
>不能保证被及时的执行

## 第3章，对于所有对象都通用的方法
### 8，覆盖equals时请遵守通用约定
>1，类的每个实例本质上都是唯一的  
>2，不关心类是否提供了逻辑相等的测试功能   
>3，超类已经覆盖了equals，从超类继承过来的行为对于子类也是合适的。  
>4，类是私有的或者是包级私有的，可以确定它的equals方法永远不会被调用  
>5，当编写equals方法时，应该考虑三个问题，它是否时对称的、传递的和一致的，因为equals具有对称性、传递性和一致性
### 9，覆盖equals时总要覆盖hashCode
>（1）HashCode的存在主要是用于查找的快捷性，如Hashtable，HashMap等，HashCode经常用于确定对象的存储地址；  
>（2）如果两个对象相同， equals方法一定返回true，并且这两个对象的HashCode一定相同；  
>（3）两个对象的HashCode相同，并不一定表示两个对象就相同，即equals()不一定为true，只能说明这两个对象在一个散列存储结构中
### 10，始终要覆盖toString方法
>toString方法应该返回对象中包含的所有值得关注的信息   
### 11，谨慎地覆盖clone
### 12，考虑实现Comparable接口
```java
public int compareTo( NumberSubClass referenceName )
```
>referenceName -- 可以是一个 Byte, Double, Integer, Float, Long 或 Short 类型的参数。   
>如果指定的数与参数相等返回0  
>如果指定的数小于参数返回 -1  
>如果指定的数大于参数返回 1 

## 第4章，类和接口
### 13，使类和成员的可访问性最小化
> 有效的降低各个模块之间的耦合   
> 规则:  
>规则1，尽可能地使每个类或者成员不被外界访问   
>1.1 私有的，private：只有在声明该成员的顶层类内部才可以访问这个成员  
>1.2 包级私有的，package-private：声明该成员的包内部的任何类都可以访问这个成员，技术上称为缺省(default)访问级别，没有成员指定访问修饰符。   
>1.3 受保护的，protected：声明该成员的类的子类可以访问，并且声明该包内部的任何类也可以访问这个成员   
>1.4 共有的，public：在任何地方都可以访问   
### 14, 在公有类中使用访问方法而非公有域
>如果类可以在它所在的包外部进行访问，就提供访问方法
```java
class Point {
	private double x;
	private double y;

	public Point(double x, double y) {
		this.x = x;
		this.y = y;
	}

	public double getX() {
		return x;
	}

	public double getY() {
		return y;
	}

	public void setX(double x) {
		this.x = x;
	}

	public void setY(double y) {
		this.y = y;
	}
}
```
>对于类是包级私有的，或者是私有的嵌套类，直接暴露它的数据域并没有本质的错误。
### 15，使可变性最小化
>五条规则   
>1，不要提供任何会修改对象状态的方法   
>2，保证类不会被扩展   
>3，使所有的域都是final   
>4，使所有的域都成为私有的
>5，确保对于任何可变组件的互斥访问