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
