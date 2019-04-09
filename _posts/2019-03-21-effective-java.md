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

### 16，复合优先于继承*   
>继承的缺点：   
>1，继承不易控制，容易使得软件很脆弱，特别是继承不在同一个包下的类。   
>2，继承打破父类的封装    

### 17，要么为继承而设计，并提供文档说明，要么就禁止继承*   
>1，该类的文档必须精确的描述覆盖每个方法带来的影响   

### 18，接口优于抽象类    
>1，现有的类可以很容易被更新，以实现新的接口   
>2，接口是定义minxin（混合类型）的理想选择   
>3，接口允许我们构造非层次结构的类型框架   
>4，骨架实现类，将接口和抽象类的优点结合起来。优美之处在于为抽象类提供了实现的帮助，但是又不强加“抽象类被用作类型定义时”所特有的严格限制。   虚拟多重继承      
>
### 19，接口只用于定义类型    
>常量接口是对接口的不良使用   
>要导出常量，常见的合理选择：  
>1, 若这些常量与某个现有的类或者接口紧密相关，应该将这些常量添加到这个类或者接口中。  
>2, 若这些常量最好被看作枚举类型的成员，就应该用枚举类型(enum type)来导出这些常量  
>3, 否则，应该使用不可实例化的工具类(utility class)来导出这些常量

>简而言之，接口应该只被用来定义类型，不应该被用来导出常量。   

### 20，层次优于标签类
>标签类过于冗长

```java
class Figure {
	enum Shape {
		RECTANGLE, CIRCLE
	};

	// Tag field - the shape of this figure
	final Shape shape;

	// These fields are used only if shape is RECTANGLE
	double length;
	double width;

	// This field is used only if shape is CIRCLE
	double radius;

	// Constructor for circle
	Figure(double radius) {
		shape = Shape.CIRCLE;
		this.radius = radius;
	}

	// Constructor for rectangle
	Figure(double length, double width) {
		shape = Shape.RECTANGLE;
		this.length = length;
		this.width = width;
	}

	double area() {
		switch (shape) {
		case RECTANGLE:
			return length * width;
		case CIRCLE:
			return Math.PI * (radius * radius);
		default:
			throw new AssertionError();
		}
	}
}
```   

>标签类正是类层次的一种简单仿效
>如何将标签类转为类层次  
>1, 为标签类中每个方法都定义一个包含抽象方法的抽象类，每个方法的行为都依赖于标签值

```java
abstract class Figure {
	abstract double area();
} 
```   

>2，为每种原始标签类都定义根类的具体子类。 

```java
class Circle extends Figure {
	final double radius;
	Circle(double radius) {
		this.radius = radius;
	}
	double area() {
		return Math.PI * (radius * radius);
	}
}    
```    

>好处: 可以用来反映类型之间的本质的层次关系，有助于增强灵活性，并进行更好的编译时检查类型。  

```JAVA
class Square extends Rectangle {
	Square(double side) {
		super(side, side);
	}
}
```    

### 21，用函数对象表示策略*    

### 22，优先考虑静态成员类*     

>嵌套类(nested class)，指被定义在另一个类的内部的类。目的是为外围类(enclosing class)提供服务。   
>嵌套类种类：静态成员类(static member class), 非静态成员类(nonstaic member class)，匿名类(anonymous class)和局部类(local class)。      

>1，静态成员类：一种简单的嵌套类，

## 第5章，泛型
### 23，请不要在新代码中使用原生态类型*
>泛型(generic)，声明中具有一个或者多个类型参数的类或者接口。每种泛型定义一组参数化类型，格式为：先是类或者接口的名称，接着用<>把对应与泛型形式类型参数的实际类型参数列表括起来。
```java
List<String>
```
>每个泛型都定义了一个原生态类型(raw type),即不带任何实际类型参数的泛型名称。例如，在List<E>相对应的原生类型就是List。
>泛型的优点：1，编译时能够找到相关的错误信息。2，从集合中删除元素时，不再需要进行手工转换。  

>如果使用原生态类型，就失掉了泛型在安全性和表述性方面的所有优势。   
### 24，消除非受检警告
>尽可能消除每一个非受检警告
>如果无法消除警告，同时可以证明引起警告的代码时类型安全的，（只有在这种情况下才）可以用一个@SuppressWarnings("unchecked")注解来禁止这条警告。   
>SuppressWarning注解可以用在任何粒度的级别中，从单独的局部变量声明到整个类都可以，应该始终在尽可能小的范围中使用该注解。   
>每当使用该注解时，都要添加一条注释，说明为这么这么做时安全的。     
### 25, 列表优先于数组*
>1，数据是协变的，泛型是不可变的。  
>2，数据是具体化的，数组会在运行时发现所犯的错误，列表则可以在编译时发现错误。   
>3，数组和泛型不能很好地混合使用。例如List<E>[]。   
### 26，优先考虑泛型*

### 27，优先考虑泛型方法*

### 28，利用有限制通配符来提升API的灵活性。*
>PECS，表示producer-extends，consumer-super 

```JAVA
	public void pushAll(Iterable<? extends E> src) {
		for (E e : src)
			push(e);
	}

  	// Wildcard type for parameter that serves as an E consumer
	public void popAll(Collection<? super E> dst) {
		while (!isEmpty())
			dst.add(pop());
	}
```

### 29，优先考虑安全的异构容器*

## 第6章，枚举和注解
### 30，用enum代替int常量*   
> 枚举类型(enum type)是指由一组固定的常量组成合法值的类型，例如一年的季节，或者扑克牌的花色    
> int枚举类型有很多不足：   
> 1，类型安全性和使用方便性方面没有任何帮助。2，采用int枚举模式的程序是十分脆弱的。3，将int枚举常量翻译成可打印的字符串，并没有很便利的方法。   
> 常见变体：String枚举模式   
>java枚举类型是通过公有的静态final域为每个枚举常量导出实例的类。其没有访问的构造器，是真正的final，是实例受控的，是单例的泛型化，本质上是单元素的枚举。   
>优点：1，提供编译时的类型安全。2，枚举类型还允许任意添加方法和域，并实现任意的接口。
>为何要将方法或者域添加到枚举类型中呢？   
>首先，将数据与它的常量关联起来，为了将数据与枚举常量关联起来，得声明实例域，并编写一个带有数据并将数据保存在域中的构造器。

```java
public enum Planet {
	MERCURY(3.302e+23, 2.439e6), VENUS(4.869e+24, 6.052e6), EARTH(5.975e+24,
			6.378e6), MARS(6.419e+23, 3.393e6), JUPITER(1.899e+27, 7.149e7), SATURN(
			5.685e+26, 6.027e7), URANUS(8.683e+25, 2.556e7), NEPTUNE(1.024e+26,
			2.477e7);
	private final double mass; // In kilograms
	private final double radius; // In meters
	private final double surfaceGravity; // In m / s^2

	// Universal gravitational constant in m^3 / kg s^2
	private static final double G = 6.67300E-11;

	// Constructor
	Planet(double mass, double radius) {
		this.mass = mass;
		this.radius = radius;
		surfaceGravity = G * mass / (radius * radius);
	}

	public double mass() {
		return mass;
	}

	public double radius() {
		return radius;
	}

	public double surfaceGravity() {
		return surfaceGravity;
	}

	public double surfaceWeight(double mass) {
		return mass * surfaceGravity; // F = ma
	}
}
```

> 枚举有一个静态的values方法，按照声明顺序返回它的值数据。

### 31，用实例域代替序数
> 永远不要根据枚举的序数导出与它关联的值，而是要将它保存在一个实例域中。

```JAVA
public enum Ensemble {
	SOLO(1), DUET(2), TRIO(3), QUARTET(4), QUINTET(5), SEXTET(6), SEPTET(7), OCTET(
			8), DOUBLE_QUARTET(8), NONET(9), DECTET(10), TRIPLE_QUARTET(12);

	private final int numberOfMusicians;

	Ensemble(int size) {
		this.numberOfMusicians = size;
	}

	public int numberOfMusicians() {
		return numberOfMusicians;
	}

	public static void main(String []args){
		System.out.println(Ensemble.SOLO.numberOfMusicians());
	}
}
```

### 32，用EnumSet代替位域


























>注意：*代表需要进一步的阅读和理解