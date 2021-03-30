---
title: javaSE  
p: java-note/javaSE/javaSE  
date: 2019-10-12 10:32:06  
categories: java  
tags: [java]  
---

[TOC]

# java控制台执行  

可以直接编译＋运行  

```cmd  
 java Create.java  
```

# 字符串String  

## 特点  

String内容不可改变  

字符串常量就是String的匿名对象  

所谓的直接赋值实际上就是相当于将一个匿名对象设置了一个名字而已  
	  

String类的匿名对象是由系统自动设置，而不是有用户自己定义的  
String当做参数传递没有改变，是因为新的String通过赋值指向了新的对象，所以对传入的String没有影响  

## 修改类  

StringBuffer  
	  

StringBuffer的内容可以修改  
	  

方法：  
		public StringBuffer reverse()  
		append  
		insert  
		delete  
	线程安全的，都是同步方法  
和String一样都实现了CharSequence接口  
StringBulid  
	是StringBuffer基本一样  
	线程不安全的  

## 两种实例化方式  

直接赋值  
new构造方法  
	  

其内容不会保存在对象池中  
	  

使用new关键字，在堆上开辟一个内存  
	  

手动入池  
		  

public native String intern();  
	  

容易找出空间浪费，不建议使用	  

# 常用类  

## 数字类  

## 大数类  

### 大整数类BigInteger  

String构造方法  
	public BigInteger(String val)  

### 大浮点数BigDecimal  

构造方法  
	String构造方法  
	double构造方法  
可实现准确的四舍五入操作  
	public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)  
	除以1实现四舍五入  
		divide(new BigDecimal(1),scale,  
BigDecimal.ROUND_HALF_UP)  
	round(new MathContext(setPrecision, RoundingMode.HALF_UP))  
		MathContext构造方法默认四舍五入  
	setScale(newScale, RoundingMode.HALF_UP)  
		小数位后保留  

## Math类  

 Math类里面提供的方法都是static方法，Math类里面都没有普通方法  
四舍五入round  
	public static long round(double a)  
	如果负数进行四舍五入时，大于-0.5才为-1  

# JDBC  

1、加载数据库驱动  
	E:\app\test\product\11.2.0\dbhome_1\jdbc\lib\ojdbc6.jar  
	  

Oracle驱动类：oracle.jdbc.dirver.OracleDrive  
	连接oracle  
		驱动程序下载oracle自带有  
	  

加载驱动类class.forName("oracle.jdbc.dirver.OracleDrive")  

2、建立数据库连接  
	DriverManager.getConnection(url, user, password)  
	数据库链接地址（URL）  
		oracle  
			jdbc：oracle:连接方式：主机名称：端口号：数据库的SID（Security Identifier）  
			  

连接本机的mldn数据库：  
				jdbc:oracle:thin:@localhost:1521:mldn  
java之中所有的数据库操作类和接口在java.sql  
数据库驱动程序有数据库生成商提供  
JDBC在实现数据库驱动连接对象使用工厂设计设计模式，而DriverManager就是工厂类  
	所以客服端调用连接时，隐藏子类的具体连接实现  

# 正则表达式  

Pattern类  
	获得此类对象必须通过Compile()方法，编译正则表达式  
## Matcher类  

Pattern类获得  

```java
package java.util.regex;
```

| name               | description |      |
| ------------------ | ----------- | ---- |
| public int start() |             |      |
|                    |             |      |
|                    |             |      |



## 字符串的正则运用  

	matches()：正则验证  
	replaceAll(String regex, String replacement):全部替换  
	replaceFirst(String regex, String replacement)：替换首个  
	split(String regex) ：全部拆分  
	split(String regex, int limit)：部分拆分  

## 正则标记  

都在Pattern类定义  
单个字符（匹配数量1）  
	字符：由一个字符组成  
	\\转移字符'\'  
	'\t'制表符  
	\n换行符  
字符集（数量1）  
	[abc]表示字符a、b和c中的任意一个（或的关系）  
	[ ^abc]表示不是abc中任意一个（^非的关系）  

	**[x-y] x的ascii到y的ascii码之间的值**  
	  
	[a-z]所有小写字母**（也可以[e-i])**  

  







[a-zA-Z]任意字母  
	  

[0-9]任意一位数字  
简化字符表达式（数量1）  
	. :任意一位字符  
	\d： =[0-9]  
	\D : = [ ^0-9]  
	\s: 任意空白字符，如’\t','\n'  
	\S:任意非空白字符  
	\w : =[a-zA-Z_0-9]  
	\W : = [ ^a-zA-Z_0-9]  
边界表达式（不要在java中用，javaScript中用）  
	^:正则开始  
	$:正则结束  
数量表达式  
	正则？：表示正则可以出现0次或1次 {0,1}  
	正则+：表示正则可以出现1次或1次以上 >=1 {1,}  
	正则*：表示正则可以出现0次或1次或多次 >= 0 {0,}  
	正则{n}：表示正则正好出现n次  
	正则{n,}：表示正则出现n次及以上  
	正则{n,m}：表示正则出现{n,m}次  
逻辑表达式  
	正则1正则2：判断第一个完成以后再判断第二个正则  
	正则1|正则2：两个正则的或  
（正则）：**将多个正则作为一组**，可以为这一组单独设置次数注解  

# Annotation  

3个最常用的基础注解  
	声明覆写操作@Override  
		明确告诉编译器覆写，如果没有覆写成功则会报错  
	声明过期操作@desperated  
	压制警告@SupperssWarming  

## 多线程  

实现  
	Thread实现  
	Runnable接口实现，能多继承  
		Thread类有Runnable的构造函数  
	Callable泛型接口实现，能有返回值  
		Thread类没有Callable的构造函数  
		FutureTask类负责接收call方法的返回值（接收Callable构造），实现RunnableFuture接口，RunnableFuture接口实现了Runnable接口、Future接口的get方法（负责接收返回值）  
	需要重写run方法，start开始  
		不用run方法是因为需要判断线程是否重复启动，并且需要不同操作系统提供start0的资源调配方法  
休眠Sleep（Thread）  
	几个线程一起休眠感觉是一起结束（时间长)，当是结束之后还是要抢占资源（时间短）,顺序是不固定的  
优先级  
	优先级越高，越有可能抢占到资源，越有可能执行  
同步synchronized  
	同步代码块  
	同步方法public synchronized 返回值  
等待wait(Object类）  
	notify唤醒  

## 对象克隆  

如果没有实现Cloneable的接口会上抛一个异常  
Cloneable接口  
	没有方法和全局常量  
	此为标识接口，表示一种能力  
需要覆写Object类的clone方法  
	protected native Object clone() throws CloneNotSupportedException;  

# 反射  

“反”通过对象找到类的出处  
java.lang.Class反射的源头  
## 三种实例化方式  
第一种：调用Object类中的getClass()  
	需要对象实例化  
第二种：类.class属性  
	不需要对象实例化，需要import  
	Spring、Hibernate  
第三种：Class提供的forName()方法  
	不需要import导入类，类用String描述  

```java
public static Class<?> forName(String className)  
```

```java
//		 通过Class.forName方式
Class feeClass = null;
try {
    feeClass = Class.forName("pojo.Fee");
} catch (ClassNotFoundException e) {
    e.printStackTrace();
```

## 获取属性

```java
// 获取属性  
Field[] field01 = clazz.getFields(); // 返回属性为public的字段  
Field[] field02 = clazz.getDeclaredFields(); // 返回所有的属性  
Field field03 = clazz.getDeclaredField("id"); // 获取属性为id的字段  
```

### Field方法:

```java
String name = field.getName();
Class<?> type = field.getType();
```
```java
// 获取对象属性
Fields[] fields = clazz.getDeclaredFields();
for(Field field: fields){
    String name = field.getName();
    field.setAccessible(true); // 私有属性必须设置访问权限
    Object resultValue = field.get(obj); 
    // 这里可以编写你的业务代码
    System.out.println(name + ": " + resultValue);
}
```



## 获取方法

```java
// 获取普通方法  
Method[] Method01 = clazz.getDeclaredMethods(); // 返回public方法 
Method method = clazz.getDeclaredMethod("getId", null); // 返回getId这个方法，如果没有参数，就默认为null   
```

Method使用

```java
method.invoke(obj, new Object[]{});

```

> obj: 调用方法的对象
>
> the object the underlying method is invoked from

```java
Method method = clazz.getMethod(methodName, new Class[]{});
```

> 方法名+参数列表找到指定方法

```java
Object obj = clazz.newInstance();
// 获取对象属性
Fields[] fields = clazz.getDeclaredFields();
for(Field field: fields){
    String fieldName = field.getName();
    String upperChar = fieldName.substring(0,1).toUpperCase();
    String anotherStr = fieldName.substring(1).;
    String methodName = "get" + upperChar + anotherStr;
    Method method = clazz.getMethod(methodName, new Class[]{});
    method.setAccessiable(true);
    Object resultValue = method.invoke(obj, new Object[]{});
    // 这里可以编写你的业务代码
    System.out.println(fieldName + ": " + resultValue);
}
```

反射对象实例化  
	Class类的无参构造方法：public T newInstance()  
	new是耦合的主要元凶，当出现高耦合时大多数时能用反射降低  
		工厂模式  
构造方法调用  
	最好保留无参构造方法，以便构造  
	取得指定构造方法  
		public Constructor<T> getConstructor(Class<?>... parameterTypes)  
			public类型构造方法  
		public Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)  
			所有构造方法  
	取得一些构造方法    

# 函数式编程（Lambda）、流式编程

## 函数式接口

`java.util.function` 包旨在创建一组完整的目标接口，使得我们一般情况下不需再定义自己的接口。这主要是因为基本类型会产生一小部分接口。 如果你了解命名模式，顾名思义就能知道特定接口的作用。

 以下是基本命名准则：

1. 如果**只处理对象而非基本类型**，名称则为 `Function`，`Consumer`，`Predicate` 等。参数类型通过泛型添加。
2. 如果接收的参数是基本类型，则由名称的第一部分表示，如 `LongConsumer`，`DoubleFunction`，`IntPredicate` 等，但基本 `Supplier` 类型例外。
3. 如果返回值为基本类型，则用 `To` 表示，如 `ToLongFunction <T>` 和 `IntToLongFunction`。
4. 如果返回值类型与参数类型一致，则是一个运算符：单个参数使用 `UnaryOperator`，两个参数使用 `BinaryOperator`。
5. 如果接收两个参数且返回值为布尔值，则是一个谓词（Predicate）。
6. 如果接收的两个参数类型不同，则名称中有一个 `Bi`。

下表描述了 `java.util.function` 中的目标类型（包括例外情况）：

| **特征**                                            |                       **函数式方法名**                       |                           **示例**                           |
| :-------------------------------------------------- | :----------------------------------------------------------: | :----------------------------------------------------------: |
| 无参数； <br> 无返回值                              |         **Runnable** <br> (java.lang)  <br>  `run()`         |                         **Runnable**                         |
| 无参数； <br> 返回类型任意                          |         **Supplier** <br> `get()` <br> `getAs类型()`         | **Supplier`<T>`  <br> BooleanSupplier  <br> IntSupplier  <br> LongSupplier  <br> DoubleSupplier** |
| 无参数； <br> 返回类型任意                          |   **Callable** <br> (java.util.concurrent)  <br> `call()`    |                      **Callable`<V>`**                       |
| 1 参数； <br> 无返回值                              |                 **Consumer** <br> `accept()`                 | **`Consumer<T>` <br> IntConsumer <br> LongConsumer <br> DoubleConsumer** |
| 2 参数 **Consumer**                                 |                **BiConsumer** <br> `accept()`                |                    **`BiConsumer<T,U>`**                     |
| 2 参数 **Consumer**； <br> 1 引用； <br> 1 基本类型 |             **Obj类型Consumer** <br> `accept()`              | **`ObjIntConsumer<T>` <br> `ObjLongConsumer<T>` <br> `ObjDoubleConsumer<T>`** |
| 1 参数； <br> 返回类型不同                          | **Function** <br> `apply()` <br> **To类型** 和 **类型To类型** <br> `applyAs类型()` | **Function`<T,R>` <br> IntFunction`<R>` <br> `LongFunction<R>` <br> DoubleFunction`<R>` <br> ToIntFunction`<T>` <br> `ToLongFunction<T>` <br> `ToDoubleFunction<T>` <br> IntToLongFunction <br> IntToDoubleFunction <br> LongToIntFunction <br> LongToDoubleFunction <br> DoubleToIntFunction <br> DoubleToLongFunction** |
| 1 参数； <br> 返回类型相同                          |               **UnaryOperator** <br> `apply()`               | **`UnaryOperator<T>` <br> IntUnaryOperator <br> LongUnaryOperator <br> DoubleUnaryOperator** |
| 2 参数类型相同； <br> 返回类型相同                  |              **BinaryOperator** <br> `apply()`               | **`BinaryOperator<T>` <br> IntBinaryOperator <br> LongBinaryOperator <br> DoubleBinaryOperator** |
| 2 参数类型相同; <br> 返回整型                       |         Comparator <br> (java.util) <br> `compare()`         |                     **`Comparator<T>`**                      |
| 2 参数； <br> 返回布尔型                            |                 **Predicate** <br> `test()`                  | **`Predicate<T>` <br> `BiPredicate<T,U>` <br> IntPredicate <br> LongPredicate <br> DoublePredicate** |
| 参数基本类型； <br> 返回基本类型                    |         **类型To类型Function** <br> `applyAs类型()`          | **IntToLongFunction <br> IntToDoubleFunction <br> LongToIntFunction <br> LongToDoubleFunction <br> DoubleToIntFunction <br> DoubleToLongFunction** |
| 2 参数类型不同                                      |                 **Bi操作** <br> (不同方法名)                 | **`BiFunction<T,U,R>` <br> `BiConsumer<T,U>` <br> `BiPredicate<T,U>` <br> `ToIntBiFunction<T,U>` <br> `ToLongBiFunction<T,U>` <br> `ToDoubleBiFunction<T>`** |

此表仅提供些常规方案。通过上表，你应该或多或少能自行推导出更多行的函数式接口。

可以看出，在创建 `java.util.function` 时，设计者们做出了一些选择。 

例如，为什么没有 `IntComparator`，`LongComparator` 和 `DoubleComparator` 呢？有 `BooleanSupplier` 却没有其他表示 **Boolean** 的接口；有通用的 `BiConsumer` 却没有用于 **int**，**long** 和 **double** 的 `BiConsumers` 变体（我对他们放弃的原因表示同情）。这些选择是疏忽还是有人认为其他组合的使用情况出现得很少（他们是如何得出这个结论的）？

## 流常用处理方法

匹配，遍历中遇到function返回true中断

anyMatch

`anyMatch(Predicate)`：如果流中的任意一个元素根据提供的 **Predicate** 返回 true 时，结果返回为 true。这个操作将会在第一个 true 之后短路；也就是不会在发生 true 之后继续执行计算。



forEach 遍历，全部遍历，不能中断

itearte 自定义序列

根据第一的seed参数应用于第二个function**产生序列**

```java
public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f) {
```

> Returns an infinite sequential ordered `Stream` produced by iterative 
> application of a function `f` to an initial element 
> `seed`, producing a `Stream` consisting of 
> `seed`, `f(seed)`, `f(f(seed))`, etc. 

```java
List<String> list = java.util.Arrays.asList("a","b","c");
Stream.iterate(0, i -> i + 1).limit(list.size()).forEach(i -> {
    System.out.println(String.valueOf(i) + list.get(i));
});
```

## 流元素排序

sorted()的默认比较器

```java
Stream<T> sorted();
Stream<T> sorted(Comparator<? super T> comparator);
```

```java
sorted(Comparator.reverseOrder())
```



# 共享设计模式  

	在JVM的底层实际上会存在有一个对象池（不一定只保存String），当String通过直接赋值创建一个String类对象时，会将此匿名对象如此保存，而后若果有新的String通过直接复制并且赋值内容和之前入池的相同，则不会开辟新的堆内存，而是使用之前对象池的引用。  

# 4种代码块  

1、普通代码块  
	代码块写在了方法里面  
	改变变量作用域，能防止重名？（但是什么没有）  
2、构造块  
	代码写在了类里  
	构造块优先于构造块执行，每次构造执行一次（没什么用）  
3、静态代码块  
	非主类  
	主类  
		静态块在主方法前运行  
	为了静态变量的初始化，一个类只执行一次（也没什么用）  
4、同步代码块  
	多线程同步使用  
尽量不要使用代码块



# io流



# transient

java语言的关键字，[变量](https://baike.baidu.com/item/变量/3956968)[修饰符](https://baike.baidu.com/item/修饰符/4088564)，如果用transient声明一个[实例变量](https://baike.baidu.com/item/实例变量/3386159)，当对象存储时，它的值不需要维持。换句话来说就是，用transient关键字标记的成员变量不参与序列化过程。

Java的[serialization](https://baike.baidu.com/item/serialization)提供了一种持久化对象实例的机制。当持久化对象时，可能有一个特殊的对象数据成员，我们不想用serialization机制来保存它。为了在一个特定对象的一个域上关闭serialization，可以在这个域前加上关键字transient。当一个对象被序列化的时候，transient型变量的值不包括在序列化的表示中，然而非transient型的变量是被包括进去的。