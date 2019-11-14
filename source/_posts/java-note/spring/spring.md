---
title: spring
date: 2019-10-19 15:23:24
categories: java
tags: [java,spring]
---

2002 Rod Johnon <Expoer One-on-one j2eedevelopment and Design>
Spring  2003  ,IOC  Aop
Spring data,spring boot,spring cloud,spring framework ,spring social 

[TOC]

# IOC :控制反转

（DI：依赖注入）

## 1.搭建Spring环境

下载jar
http://maven.springframework.org/release/org/springframework/spring/
spring-framework-4.3.9.RELEASE-dist.zip
开发spring至少需要使用的jar(5个+1个):
spring-aop.jar		开发AOP特性时需要的JAR
spring-beans.jar	处理Bean的jar			<bean>
spring-context.jar	处理spring上下文的jar		<context>
spring-core.jar		spring核心jar
spring-expression.jar	spring表达式 
三方提供的日志jar
commons-logging.jar	日志

## 2.编写配置文件

为了编写时有一些提示、自动生成一些配置信息：
方式一：增加sts插件
可以给eclipse增加 支持spring的插件：spring tool suite(https://spring.io/tools/sts/all)
下载springsource-tool-suite-3.9.4.RELEASE-e4.7.3a-updatesite.zip,然后在Eclipse中安装：Help-Install new SoftWare.. - Add

方式二：
	直接下载sts工具（相当于一个集合了Spring tool suite的Eclipse）: https://spring.io/tools/sts/

新建：bean configuration .. - applicationContext.xml

## 3.开发Spring程序(IOC)

```java
ApplicationContext conext = new ClassPathXmlApplicationContext("applicationContext.xml") ;
	//执行从springIOC容器中获取一个 id为student的对象
Student student = (Student)conext.getBean("student") ;
```
可以发现，springioc容器 帮我们new了对象，并且给对象赋了值

SpringIOC发展史：
1.
Student student = new Student();
student.setXxx();

2.
简单工厂

3.ioc （超级工厂）



IOC（控制反转,Inversion of Control）也可以称之为DI（依赖注入,Dependency Injection）：
控制反转：将 创建对象、属性值 的方式 进行了翻转，从new、setXxx()  翻转为了 从springIOC容器getBean()
依赖注入：将属性值 注入给了属性，将属性 注入给了bean，将bean注入给了ioc容器；


​	
总结：ioc/di ，无论要什么对象，都可以直接去springioc容器中获取，而不需要自己操作（new\setXxx()）

因此之后的ioc分为2步：1 先给springioc中存放对象并赋值   2 拿


DI:依赖注入 ，
Teacher  

Course  : cname  teacher

IOC容器赋值：如果是简单类型（8个基本+String），value； 
	如果是对象类型，ref="需要引用的id值"，因此实现了 对象与对象之间的依赖关系


conext.getBean(需要获取的bean的id值)



### 依赖注入3种方式：
### 1.set注入：通过setXxx()赋值

赋值，默认使用的是 set方法();
**依赖注入底层是通过反射实现的。**
<property...>

### 2.构造器注入：通过构造方法赋值
 <constructor-arg value="ls" type="String" index="0" name="name"></constructor-arg>
需要注意：如果  <constructor-arg>的顺序 与构造方法参数的顺序不一致，则需要通过type或者index或name指定。

### 3.p命名空间注入

```xml
引入p命名空间
	xmlns:p="http://www.springframework.org/schema/p"

<bean id="course" class="org.lanqiao.entity.Course" p:courseHour="300" p:courseName="hadoop" p:teacher-ref="teacher">
```

本质也是反射

简单类型：
	p:属性名="属性值"
引用类型（除了String外）：
	p:属性名-ref="引用的id"
注意多个 p赋值的时候 要有空格。

注意：
**无论是String还是Int/short/long，在赋值时都是 value="值" ，当参数重载时默认为String优先**
因此建议 此种情况 需要配合 name\type进行区分



示例：
	注入各种集合数据类型: List  Set map properties

set、list、数组   各自都有自己的标签<set> <list> <array>，但是也可以混着用

###**value与<value>注入方式的区别：**

|                                         | 使用子元素<value>注入                                        | 而使用value属性注入                     |
| --------------------------------------- | :----------------------------------------------------------- | --------------------------------------- |
| 参数值位置                              | 写在首尾标签（<value></value>）的中间(不加双引号)            | 写在value的属性值中（必须加双引号）     |
| type属性                                | 有（可选）   可以通过type属性指定数据类型                    | 无                                      |
| 参数值包含特殊字符（<， &）时的处理方法 | 两种处理方法。  **一、使用<![CDATA[]]>标记**  二、使用XML预定义的实体引用 | 一种处理方法。即使用XML预定义的实体引用 |

 

其中，XML预定义的实体引用，如表所示。

| 实体引用 | 表示的符号 |
| -------- | ---------- |
| &lt;     | <          |
| &amp;    | &          |
| &gt;     | >          |

 

给对象类型赋值null ：

```xml
<property name="name" >  
				<null/>       -->注意 没有<value>
		</property>
```



赋空值 ""  

```xml
<property name="name" >  
				<value></value>  
		</property>
```

在ioc中定义bean的前提：该bean的类 必须提供了 无参构造



### 4.**自动装配（只适用于 ref类型 ）**：
​	约定优于配置

自动装配：
<bean ... class="org.lanqiao.entity.Course"  autowire="byName|byType|constructor|no" >  byName本质是byId
byName:  自动寻找：其他bean的id值=该Course类的属性名
byType:  其他bean的类型(class)  是否与 该Course类的ref属性类型一致  （注意，此种方式 必须满足：当前Ioc容器中 只能有一个Bean满足条件  ）
constructor： 其他bean的类型(class)  是否与 该Course类的构造方法参数 的类型一致；此种方式的本质就是byType


可以在头文件中 一次性将该ioc容器的所有bean  统一设置成自动装配：
<beans xmlns="http://www.springframework.org/schema/beans"
...
default-autowire="byName">

自动装配虽然可以减少代码量，但是会降低程序的可读性，使用时需要谨慎。

## 使用注解定义bean：

通过注解的形式 将bean以及相应的属性值 放入ioc容器

```xml
<context:component-scan base-package="pers.nicolas.aop"></context:component-scan>
```

在启动的时候，会根据base-package在 该包中扫描所有类，查找这些类是否有注解@Component("studentDao"),如果有，则将该类 加入spring Ioc容器。

**@Component细化：**

**dao层注解：@Repository**
**service层注解：@Service**
**控制器层注解：@Controller**

```java
@Service("StudentService") //注意添加scan扫描包
public class StudentServiceImpl implements IStudentService{
	@Autowired //自动装配，byType
	@Qualifier("stuDao") // byIdName
	IStudentDao studentDao = new StudentDaoImp();
	
	public void setStudentDao(IStudentDao studentDao) {
		this.studentDao = studentDao;
	}
}
```





# 使用注解实现事务

（声明式事务）

目标：通过事务 使以下方法 要么全成功、要么全失败
public void addStudent()
{
	//增加班级
	//增加学生
	//crdu
}

a. jar包
spring-tx-4.3.9.RELEASE
ojdbc.jar
commons-dbcp.jar  连接池使用到数据源
commons-pool.jar  连接池
spring-jdbc-4.3.9.RELEASE.jar
aopalliance.jar 

b.配置
jdbc\mybatis\spring
**增加事务tx的命名空间**

```xml
<!-- 配置数据库 -->
	<bean id="dataSource"
		class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName"
			value="oracle.jdbc.OracleDriver"></property>
		<property name="url"
			value="jdbc:oracle:thin:@localhost:1521:MLDN"></property>
		<property name="username" value="scott"></property>
		<property name="password" value="tigger"></property>
		<property name="maxActive" value="10"></property>
		<property name="maxIdle" value="6"></property>
	</bean>
	<!-- 配置事务管理器txManager -->
	<bean id="txManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	<!-- 增加对事务的支持 -->
	<tx:annotation-driven
		transaction-manager="txManager" />
```



c.使用

将需要 成为事务的方法 前增加注解：
@Transactional(readOnly=false,propagation=Propagation.REQUIRED)

# AOP：面向方面编程


一个普通的类	->	有特定功能的类  
	a.继承类  b.实现接口  c.注解  d.配置

public class MyFilter exntends/implements Xx
{

}


类 -> “通知” ：实现接口

## 前置通知实现步骤：

ThrowsAdvice

a.jar
	aopaliance.jar
	aspectjweaver.jar

b.配置

c.编写
	aop：每当之前add()之前 自动执行一个方法log();

	addStudent();  业务方法（IStudentService.java中的  addStudent()）
	before();  自动执行的通知，即aop前置通知

```xml
<!-- 前置通知类 -->
	<bean id="LogBefore" class="pers.nicolas.aop.LogBefore">
	</bean>
	<!-- 将方法和通知进行关联 -->
<aop:config>
	<!-- 配置切入点（在哪里执行通知） -->
	<aop:pointcut
		expression="execution(public void pers.nicolas.service.StudentServiceImpl.deleteStudentByNo(int)) or execution(public void pers.nicolas.service.StudentServiceImpl.addStudent(pers.nicolas.entity.Student))"
			id="pointcut" />
	<!-- advisor相当于连接切入点和切面的线 -->
	<aop:advisor advice-ref="LogBefore"
		pointcut-ref="pointcut" />
</aop:config>
```

## 表达式expression的常见示例如表所示。 

expression=*"execution(…)"* 

| 举例                                                         | 含义                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| public   boolean addStudent(org.lanqiao.entity.Student))     | 所有返回类型为boolean、参数类型为org.lanqiao.entity.Student的addStudent()方法。 |
| public   boolean org.lanqiao.service.IStudentService.   addStudent(org.lanqiao.entity.Student) | org.lanqiao.service.IStudentService类（或接口）中的addStudent()方法，并且返回类型是boolean、参数类型是org.lanqiao.entity.Student |
| public   * addStudent(org.lanqiao.entity.Student)            | “*”代表任意返回类型                                          |
| public   void *( org.lanqiao.entity.Student)                 | “*”代表任意方法名                                            |
| public   void addStudent(..)                                 | “..”代表任意参数列表                                         |
| *   org.lanqiao.service.*.*(..)                              | org.lanqiao.service.IStudentService包中，包含的所有方法（不包含子包中的方法） |
| * org.lanqiao.service..*.*(..)                               | org.lanqiao.service.IStudentService包中，包含的所有方法（包含子包中的方法） |

execution(* com.atguigu.crud.service..*(..))

expression常见示例

org.lanqiao.service   

org.lanqiao.service.impl

public class Xxx
{
	@Test
	a(){}
}

**如果出现异常：类似java.lang.NoClassDefFoundError: org/apache/commons/pool/impl/GenericObjectPool**
**则说明缺少jar**



## 后置通知：

AfterReturningAdvice

a.通知类  ，普通实现接口
b.业务类、业务方法
	StudentServiceImpl中的addStudent()
c.配置：
	将业务类、通知 纳入springIOC容器
	定义切入点（一端）、定义通知类（另一端），通过pointcut-ref将两端连接起来



## 异常通知：

ThrowsAdvice

​	根据异常通知接口的定义可以发现，异常通知的实现类 必须编写以下方法：
​	public void afterThrowing([Method, args, target], ThrowableSubclass)：

	a.public void afterThrowing(Method, args, target, ThrowableSubclass)
	b.public void afterThrowing( ThrowableSubclass)



## 环绕通知：

MethodInterceptor

在目标方法的前后、异常发生时、最终等各个地方都可以 进行的通知，最强大的一个通知；
	可以获取目标方法的 全部控制权（目标方法是否执行、执行之前、执行之后、参数、返回值等）

在使用环绕通知时，目标方法的一切信息 都可以通过invocation参数获取到
**环绕通知 底层是通过拦截器实现的。**

```java
public Object invoke(MethodInvocation invocation) throws Throwable {
		Object result = null;
		try {
			System.out.println("around advice[before advice]");
			// invocation.proceed()之前后的代码：前后置通知
			result = invocation.proceed();// 控制目标方法的执行 xxx(),result就是目标方法的返回值
			// invocation.proceed()之后的代码：后置通知
			System.out.println("");
			System.out
					.println("around advice[after advice]: traget object :" + invocation.getThis() + ", function name:="
							+ invocation.getMethod().getName() + ",returnValue=" + result + ",args=" + invocation.getArguments());
		} catch (Exception e) {
			// 异常通知
			System.out.println("around advice[exception advice]");
		}
		return result;
	}
```





## 二、实现注解实现 通知 aop

a.jar
	与 实现接口 的方式相同
b.配置
	将业务类、通知 纳入springIOC容器
	开启注解对AOP的支持<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
	业务类 addStudent -  通知 

c.编写
	
	
通知：
@Aspect  //声明该类 是一个 通知
public class LogBeforeAnnotation  {

}

注意：通过注解形式 将对象增加到 ioc容器时，需要设置 扫描器
<context:component-scan base-package="org.lanqiao.aop"></context:component-scan>
	
扫描器 会将 指定的包 中的  @Componet @Service  @Respository   @Controller修饰的类产生的对象 增加到IOC容器中
@Aspect不需要 加入扫描器，只需要开启即可：<aop:aspectj-autoproxy></aop:aspectj-autoproxy>

通过注解形式 实现的aop，如果想获取 目标对象的一些参数，则需要使用一个对象：JoinPoint

注解形式的返回值：
a.声明返回值 的参数名：
	@AfterReturning( pointcut= "execution(public * addStudent(..))" ,returning="returningValue" ) 
	public void myAfter(JoinPoint jp,Object returningValue) {//returningValue是返回值，但需要告诉spring
		System.out.println("返回值："+returningValue );
注解形式实现aop时，通知的方法的参数不能多、少

实现接口形式、注解形式 只捕获声明的特定类型的异常，而其他类型异常不捕获。
cath()

```java
@Component("logAnnotaiton")
@Aspect // 声明此类是一个通知
public class LogAopAnnotation {
	//前置通知
	@Before("execution(public * addStudent(..))") // 属性：定义切点
	public void MyBefore(JoinPoint jp) {
		System.out.println("<[Annotation]前置 advice>: target object:" + jp.getTarget() + ",methodName:"
				+ jp.getSignature().getName() + ",arguments:" + Arrays.toString(jp.getArgs()));
	}
	//后置通知
	@AfterReturning(pointcut = "execution(public * addStudent(..))",returning = "returningValue") // 属性：定义切点,要得到返回值必须通过注解声明
	public void AfterReturning(JoinPoint jp,Object returningValue) {
		System.out.println("<[Annotation]后置advice> target object:" + jp.getTarget() + ",methodName:"
				+ jp.getSignature().getName() + ",arguments:" + Arrays.toString(jp.getArgs())+",returnVlaue:"+returningValue);
	}
	//Exception Advice:如果只捕获指定异常，可以通过通过注解throwing指定
	@AfterThrowing(pointcut = "execution(public * addStudent(..))",throwing = "e")
	public void MyException(JoinPoint jp,NullPointerException e) {
		System.out.println("<[Annotation]exception advice> e:"+e.getMessage());
	}
	//环绕通知,参数ProceedingJoinPoint
	@Around("execution(public * addStudent(..))")
	public void MyAroud(ProceedingJoinPoint jp){
		try {
			System.out.println("<[Annotation【Around】]前置 advice>");
			//方法之前，前置通知
			jp.proceed();//方法执行
			//方法之后，后前置通知
			System.out.println("<[Annotation【Around】]后置 advice>");
		}catch(Throwable e) {//方法异常，异常通知
			System.out.println("<[Annotation【Around】]exception advice>");
		}finally {//最终执行，最终通知
			System.out.println("<[Annotation【Around】]最终 advice>");
		}
	}
	//最终通知
	@After("execution(public * addStudent(..))")
	public void MyAfter() {
		System.out.println("<[Annotation]最终advice>");
	}
}
	

```



## 三、通过 配置将 类->通知 
基于Schema配置
类似 于 实现接口的方式

接口方式通知：public class LogAfter implements AfterReturningAdvice
Schema方式通知：
	a.编写一个普通类  public class LogAfter {}  
	b.将该类 通过配置，转为一个“通知”
	

如果要获取目标对象信息：
注解、schema：JoinPoint
接口：Method method, Object[] args, Object target

schema形式 和注解形式相似，不同之处： 注解形式 使用了注册@After，  schmema形式进行了多余的配置

```xml
<aop:config>
    <!-- 配置切入点（在哪里执行通知） -->
    <aop:pointcut
                  expression="execution(public void pers.nicolas.service.StudentServiceImpl.addStudent(pers.nicolas.entity.Student))"
                  id="pointcut_schema" />
    <!-- schema方式 -->
    <aop:aspect ref="LogSchema">
        <aop:before method="before" pointcut-ref="pointcut_schema" />
        <aop:after-returning method="afterReturning"
                             returning="returnValue" pointcut-ref="pointcut_schema" />
        <aop:after-throwing method="MyException"
                            throwing="e" pointcut-ref="pointcut_schema" />
        <aop:around method="around"
                    pointcut-ref="pointcut_schema" />
    </aop:aspect>
</aop:config>
```

# 1.Spring开发Web项目 及 拆分Spring配置文件
## a.Spring开发Web项目
   Web项目如何初始化SpringIOC容器 ：思路：当服务启动时（tomcat），通过监听器将SpringIOC容器初始化一次（该监听器 spring-web.jar已经提供）
   因此用spring开发web项目 至少需要7个jar： spring-java的6个jar + spring-**web.jar，注意：web项目的jar包 是存入到WEB-INF/lib中**

web项目启动时 ，会自动加载web.xml，因此需要在web.xml中加载 监听器（ioc容器初始化）。

Web项目启动时，启动实例化Ioc容器：

```xml
 <!-- 指定 Ioc容器（applicationContext.xml）的位置-->
  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:applicationContext.xml</param-value>
  </context-param>  
  <listener>
  	<!-- 配置spring-web.jar提供的监听器，此监听器 可以在服务器启动时 初始化Ioc容器。
  		初始化Ioc容器（applicationContext.xml） ，
  			1.告诉监听器 此容器的位置：context-param
  			2.默认约定的位置	:WEB-INF/applicationContext.xml
  	 -->
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
```



## b.拆分Spring配置文件
​	java项目：
​		applicationContext1.xml
​		applicationContext2.xml
​		applicationContext3.xml

ApplicationContext conext = new ClassPathXmlApplicationContext("applicationContext3.xml") ;

Web项目：
	根据什么拆分？
	i.三层结构   
		UI(html/css/jsp  、Servlet)  applicationController.xml
		Service :applicationService.xml
		Dao:applicationDao.xml
		公共 数据库:applicationDB.xml

ii.功能结构
	学生相关配置 applicationContextStudent.xml   <bean id=""  class="X...Student">
	班级相关配置 applicationContextClass.xml 

合并：如何将多个配置文件 加载
	（1）

```xml
  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
  <param-name>contextConfigLocation</param-name>
  		<param-value>
  			classpath:applicationContext.xml,
  			classpath:applicationContext-Dao.xml,
  			classpath:applicationContext-Service.xml,
  			classpath:applicationContext-Controller.xml
  		</param-value>
  </context-param>
```



​	（2）推荐

```xml
  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
  		<param-name>contextConfigLocation</param-name>
  		<param-value>
  			classpath:applicationContext.xml,
  			classpath:applicationContext-*.xml
  		</param-value>
  </context-param>
```

（3）只在web.xml中加载主配置文件，

```xml
<param-value>
  	classpath:applicationContext.xml	
  </param-value>
```

​		然后在主配置问加中，加载其他配置文件:

```xml
<import resource="applicationContext-*.xml"/>
```

Web项目：

![1561880276559](E:\pictures\Language\spring\springIoc容器和servlet容器.png)

# 2.Spring整合MyBatis

Spring - MyBatis 

思路：
	SqlSessionFactory -> SqlSession ->StudentMapper ->CRUD
可以发现 ，MyBatis最终是通过SqlSessionFactory来操作数据库，
Spring整合MyBatis 其实就是 将MyBatis的SqlSessionFactory 交给Spring

SM整合步骤：
1.jar
mybatis-spring.jar	spring-tx.jar	spring-jdbc.jar		spring-expression.jar
spring-context-support.jar	spring-core.jar		spring-context.jar
spring-beans.jar	spring-aop.jar	spring-web.jar	commons-logging.jar
commons-dbcp.jar	ojdbc.jar	mybatis.jar	log4j.jar	commons-pool.jar

2.类-表


3.MyBatis配置文件conf.xml

4.通过mapper.xml将 类、表建立映射关系

5.之前使用MyBatis:	conf.xml ->SqlSessionFacotry 

现在整合的时候，需要通过Spring管理SqlSessionFacotry ，因此 产生qlSessionFacotry 所需要的数据库信息 不在放入conf.xml  而需要放入spring配置文件中

配置Spring配置文件（applicationContext.xml）

```xml
<!-- 加载db.properties文件 -->
	<bean id="config"
class="org.springframework.beans.factory.config.PreferencesPlaceholderConfigurer">
		<property name="locations">
			<array>
				<value>classpath:db.properties</value>
			</array>
		</property>
	</bean>
	<!-- 数据库信息(替代了mybatis中的配置文件conf.xml) -->
	<bean id="dataSource"
		class="org.apache.commons.dbcp.BasicDataSource">
		<property name="driverClassName" value="${driver}"></property>
		<property name="url" value="${url}"></property>
		<property name="username" value="${username}"></property>
		<property name="password" value="${password}"></property>
		<property name="maxActive" value="${maxActive}"></property>
		<property name="maxIdle" value="${maxIdle}"></property>
	</bean>

	<!-- 在SpringIoc容器中 创建mybatis的核心类SqlSessionFactory -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
<!-- 加载mybatis配置文件，如果只有映射，不需要写conf.xml文件-->
<!-- 	<property name="configLocation" value="classpath:conf.xml"></property> -->
		<!-- 加载mapper.xml配置文件 -->
		<property name="mapperLocations" value="pers/nicolas/mapper/*.xml"></property>
	</bean>
<!-- 第3种方式批量生成mapper代理对象
批量产生的mapper对象在SpringIOC容器中的id值默认就是接口mapper名(首字母小写）-->
<bean id="mappers" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="pers.nicolas.mapper"></property>
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
</bean>
```



6.使用Spring-MyBatis整合产物开发程序
	目标：通过spring产生mybatis最终操作需要的 动态mapper对象(StudentMapper对象)
	Spring产生 动态mapper对象 有3种方法：
	

a.第一种方式
DAO层实现类  继承 SqlSessionDaoSupport类

```xml
	 SqlSessionDaoSupport类提供了一个属性 SqlSession
	 
<!--  第一种方式生成mapper代理对象 -->
<bean id="studentMapper" class="pers.nicolas.dao.impl.StudentDaoImpl">
<!--   将Spring配置的sqlSessionFactory交给Mapper（Dao) -->
	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>
```

b.第二种方式
	就是省略掉 第一种方式的 实现类
	直接MyBatis提供的 Mapper实现类：org.mybatis.spring.mapper.MapperFactoryBean 
	缺点：每个mapper都需要一个配置一次

```xml
<!-- 第二种方式生成mapper代理对象 -->
<bean id="studentMapper" class="org.mybatis.spring.mapper.MapperFactoryBean">
	<property name="mapperInterface" value="pers.nicolas.mapper.StudentMapper"></property>
	<property name="sqlSessionFactory" ref="sqlSessionFactory"></property>
</bean>
```



**c.第三种方式**
	批量配置 实现类

```xml

<!-- 第3种方式批量生成mapper代理对象
批量产生的mapper对象在SpringIOC容器中的id值默认就是接口mapper名(首字母小写）-->
<bean id="mappers" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="pers.nicolas.mapper"></property>
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
</bean>
```



	# spring注解版

## 一个接口指定实现类

接口定义

```java
package com.yoj.used;

public interface MyInterface {
    void className();
}
```

实现类1

```java
@Service("impl1")
public class Implement1 implements MyInterface{
    @Override
    public void className() {
        System.out.println(Implement1.class);
    }
}
```

实现类2

```java
@Service("impl2")
public class Implement2 implements MyInterface {
    @Override
    public void className() {
        System.out.println(Implement2.class);
    }
}
```

**1.@AutoWired+@Qualifier("impl2")指定**

```java
@Autowired
@Qualifier("impl2")
MyInterface myInterface;
```





**2.使用@Resource注入，根据@Service指定的名称区分**

```java
@Resource(name = "impl2")
MyInterface myInterface;
```



# my

## 同一类中内部调用aop失效

