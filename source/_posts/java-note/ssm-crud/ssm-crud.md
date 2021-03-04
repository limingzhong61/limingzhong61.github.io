---
title: ssm-crud
date: 2019-10-21 15:25:49
categories: java 
tags: [java,ssm]
---
[TOC]

# web路径

 **web路径：**
	1.不**以/开始的相对路径**，找资源，**以当前资源的路径为基准**，经常容易出问题
	2.**以/开始的开始的相对路径**，找资源，**以服务器为标准（http//localhost/端口号)需要加项目名**;
	就是http//localhost/端口号/crud/...

```html
<%
	pageContext.setAttribute("WEB_PATH", request.getContextPath());
%>
	${WEB_PATH}/static/bootstrap-3.3.7-dist/css/bootstrap.min.css
  -->
```

注：pageContext.setAttribute需要jsp_api.jar

# maven配置文件

D:\apache-maven-3.6.1\conf\settings.xml

设置镜像阿里mirrors

```xml
<mirrors>
 <mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
   <url>http://maven.aliyun.com/nexus/content/groups/public</url>
	<mirrorOf>central</mirrorOf>
  </mirror>
</mirrors>
```

配置maven的jdk

```xml
<profile>
	<id>jdk18</id>

	<activation>
		<activaByDefault>true</activaByDefault>
		<jdk>1.8</jdk>
	</activation>
	<properties>
	  <maven.compiler.source>1.8</maven.compiler.source>
	  <maven.compiler.target>1.8</maven.compiler.target>
	  <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
```

# maven使用spring单元测试

test写在src/test/mapper/目录下

推荐Spring的项目就可以使用spring的单元测试，可以依赖注入我们需要的组件

 1.导入springTest模块(meven导包)
 2.@ContextConfiguration指定spring配置文件内容,@RunWith(SpringJUnit4ClassRunner.class)
 3.直接autoWrited要使用的组件即可

```java
**
 * 测试dao工作层
 * @author test
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {"classpath:applicationContext.xml"})
public class MapperTest {
```

## mybatis不在同一目录下

如果需要mapper和mapper.xml文件不在一个文件夹里还能映射成功的话，需要在spring配置文件中，**分别将mapper和mapper.xml的包添加扫描；**

```xml
<!-- ============配置和mybatis的整合============== -->
	<bean id="sqlSessionFactory"
		class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="configLocation"
			value="classpath:mybatis-config.xml"></property>
		<property name="dataSource" ref="pooledDataSource"></property>
		<!-- 指定mybatis的mapper文件位置 -->
		<property name="mapperLocations"
			value="classpath:mapper/*.xml"></property>
	</bean>

	<!-- 配置扫描器，将mapper接口实现加入ioc容器 -->
	<!-- 批量生成mapper代理对象 批量产生的mapper对象在SpringIOC容器中的id值默认就是接口mapper名(首字母小写） -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="basePackage" value="com.atguigu.crud.dao"></property>
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>
	</bean>

```

# maven中classpath路径

在Maven工程中，classpath的路径指java resources路径下的目录，即

src/main/java、 src/main/resource、src/test/java 、src/test/resource   四个目录

所以在配置路径时需要注意文件名是否重名，导致加载文件失败

# 数据校验   

## jsr303数据校验

```xml
<!--jsr303数据检验支持 （Hibernate Validator Engine Relocation Artifact） -->
    <!-- jrs303数据校验支持：tomcat7以上的服务器，
tomcat7以下的服务器：el表达式，不是最新的。额外给服务器的lib包中替换新的标准的el
-->
<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
<dependency>
	<groupId>org.hibernate</groupId>
	<artifactId>hibernate-validator</artifactId>
	<version>5.4.1.Final</version>
</dependency>
```

JSR303  
Hibernate Validator 

**JSR 303提供的标准注解如表所示。**

| 注解                          | 简介                                                   |
| ----------------------------- | ------------------------------------------------------ |
| @Null                         | 被注释的元素必须为 null。                              |
| @NotNull                      | 被注释的元素必须不为 null。                            |
| @AssertTrue                   | 被注释的元素必须为 true。                              |
| @AssertFalse                  | 被注释的元素必须为 false。                             |
| @Min(value)                   | 被注释的元素必须是一个数字，其值必须大于或等于value。  |
| @Max(value)                   | 被注释的元素必须是一个数字，其值必须小于或等于value。  |
| @DecimalMin(value)            | 被注释的元素必须是一个数字，其值必须大于或等于value。  |
| @DecimalMax(value)            | 被注释的元素必须是一个数字，其值必须小于或等于value。  |
| @Size(max,   min)             | 被注释的元素的取值范围必须是介于min和max之间。         |
| @Digits   (integer, fraction) | 被注释的元素必须是一个数字，其值必须在可接受的范围内。 |
| @Past                         | 被注释的元素必须是一个过去的日期。                     |
| @Future                       | 被注释的元素必须是一个将来的日期。                     |
| @Pattern(value)               | 被注释的元素必须符合指定的正则表达式。                 |

表JSR 303注解

 

 

Hibernate Validator 是JSR 303的扩展。Hibernate Validator 提供了 JSR 303中所有内置的注解，以及自身扩展的4个注解，如表所示。

| 注解      | 简介                                     |
| --------- | ---------------------------------------- |
| @Email    | 被注释的元素值必须是合法的电子邮箱地址。 |
| @Length   | 被注释的字符串的长度必须在指定的范围内。 |
| @NotEmpty | 被注释的字符串的必须非空。               |
| @Range    | 被注释的元素必须在合适的范围内。         |

**表  Hibernate Validator扩展注解**

使用Hibernate Validator步骤：

a.jar（注意各个jar之间可能存在版本不兼容）
hibernate-validator-5.0.0.CR2.jar 	classmate-0.8.0.jar 	jboss-logging-3.1.1.GA.jar
validation-api-1.1.0.CR1.jar 	hibernate-validator-annotation-processor-5.0.0.CR2.jar



b配置

```xml
<mvc:annotation-driven ></mvc:annotation-driven>
```

此时mvc:annotation-driven的作用：要实现Hibernate Validator/JSR303 校验（或者其他各种校验），必须实现SpringMVC提供的一个接口：ValidatorFactory

LocalValidatorFactoryBean是ValidatorFactory的一个实现类。
<mvc:annotation-driven ></mvc:annotation-driven>会在**springmvc容器中 自动加载一个LocalValidatorFactoryBean类，因此可以直接实现数据校验。**

c.直接使用注解

~~~java
public class Student {

```
@Past//当前时间以前
private Date birthday ;
```

}
~~~

在校验的Controller中 ，给**校验的对象前增加 @Valid**

```java
public String testDateTimeFormat(@Valid Student student, BindingResult result ,Map<String,Object> map) {
    {...}
```

# mybatis分页插件PageHleper

```java
/**
	 * 导入jackson包
	 * 
	 * @author lmz
	 * @date 2019年7月15日-上午8:59:36
	 * @param pageNumber
	 * @return
	 */
	@RequestMapping("/emps")
	@ResponseBody // 告诉SpringMVC，此时的返回 不是一个 View页面，而是一个 ajax调用的返回值（Json数组）
	public Msg getEmpsWithJson(@RequestParam(value = "pageNumber", defaultValue = "1") Integer pageNumber) {

		// 引入分页插件PageHleper
		// 在查询之前调用即可
		PageHelper.startPage(pageNumber, 5);
		// startPage之后紧跟着的查询就是分页查询
		List<Employee> emps = employeeService.getAll();
		// 用PageInfo对结果进行包装
		// 将pagaInfo交给页面就可以了，封装了详细的分页信息，包括有我们的查询数据
		// 传入连续显示的页数
		PageInfo<Employee> page = new PageInfo<Employee>(emps, 5);
		return Msg.success().add("pageInfo", page);
	}
```

在mybatis中配置合理化参数，超过页数，返回最后一页

```xml
<configuration>
	<plugins>
		<!-- com.github.pagehelper为PageHelper类所在包名 -->
		<plugin interceptor="com.github.pagehelper.PageInterceptor">
	<!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->
			<!-- 分页参数合理化 -->
			<property name="reasonable" value="true" />
		</plugin>
	</plugins>
</configuration>
```



# sprigMVC自带HttpputFormContentFilter

```java
/**
           * 解决方案
           * 要能支持直接发送PUT之类的请求，还要封装请求体中的数据
     * 1、配置上HttpputFormContentFilter；
     * 2、作用：将请求体中的数据解析包装成一个map。
     * 3、request被重新包装，request.getParameter()被重写，就会从自己封装的map中取数据
     * 员工更新方法
     * @param employee
     * @return
     */
@PutMapping("/emp/{empId}")
@ResponseBody
public Msg saveEmp(Employee employee,HttpServletRequest request){
    System.out.println("requestScope中的值："+request.getParameter("email"));
    System.out.println(employee);
    employeeService.updateEmp(employee);
    return Msg.success();
}
    }
```

![](E:\pictures\Language\ssm\ssm整合\ssm总结.png)