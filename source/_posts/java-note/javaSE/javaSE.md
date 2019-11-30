---
title: javaSE  
p: java-note/javaSE/javaSE  
date: 2019-11-12 10:32:06  
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
Matcher类  
	Pattern类获得  

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
	三种实例化方式  
		第一种：调用Object类中的getClass()  
			需要对象实例化  
		第二种：类.class属性  
			不需要对象实例化，需要import  
			Spring、Hibernate  
		第三种：Class提供的forName()方法  
			不需要import导入类，类用String描述  
			public static Class<?> forName(String className)  
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
调用普通方法	  

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

