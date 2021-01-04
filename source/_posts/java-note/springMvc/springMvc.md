---
title: springMvc
date: 2019-10-20 15:24:32
categories: java 
tags: [java,spring,springMvc]
---

[TOC]

# springmvc:

1.jar
spring-aop.jar
spring-bean.jar
spring-context.jar
spring-core.jar
spring-web.jar

spring-webmvc.jar
commons-logging.jar

报错NoClassDefFoundError：缺少jar


2.第一个SpringMVC程序
Servet - Springmvc
jsp ->Servlet (Springmvc)->Jsp

url

## springmvc配置文件 springmvc.xml
选中常用的命名空间：beans  aop context  mvc

普通的servlet流程：
请求-url-pattern -交给对应的servlet去处理

如果现在想用springmvc，而不是普通的servlet，如何告知程序？-如何让springmvc 介入程序：
需要配置一个 Springmvc自带的servlet

通过以下配置，**拦截所有请求，交给SpringMVC处理**：

 ```xml
 <servlet>
  	<servlet-name>springDispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  			<param-name>contextConfigLocation</param-name>
  			<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  	<load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>springDispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
 ```



其中：
<url-pattern>.action</url-pattern>

/:一切请求  ，注意不是 /*
/user:拦截以 /user开头的请求
/user/abc.do  :只拦截该请求
.action:只拦截 .action结尾的请求

项目中同时兼容 springMVC和Servlet

```xml
<servlet-mapping>
  	<servlet-name>springDispatcherServlet</servlet-name>
  	<url-pattern>.action</url-pattern>
  </servlet-mapping>

  <servlet>
  	<servlet-name>springDispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  			<param-name>contextConfigLocation</param-name>
  			<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  	<load-on-startup>1</load-on-startup>
  </servlet>
```

通过

```xml
 <init-param>
  			<param-name>contextConfigLocation</param-name>
  			<param-value>classpath:springmvc.xml</param-value>
  </init-param>
```

```xml
<context:component-scan
		base-package="pers.nicolas.handler"></context:component-scan>
	<!-- 配置视图解析器(InternalResourceViewResolver) -->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/views/"></property>
		<property name="suffix" value=".jsp"></property>
```

**指定springmvc配置文件的路径**，如果要省略，必须放到 默认路径：
/WEB-INF/**（web.xml配置中）servetname的值**-servlet.xml

**可以提示工具自动生成alt+/ ,选**

![1561949336976](E:\pictures\Language\spring\提示自动生成springMvc配置.png)

## 映射是 去匹配@RequestMapping注解

可以和方法名、类名不一致
**通过method指定 请求方式（get  post  delete put）**
	@RequestMapping(value="welcome",method=RequestMethod.POST)//映射

设置name="xxxx"的情况：
params= {"name2=zs","age!=23"}

name2:必须有name="name2"参数

age!=23 :    a.如果有name="age"，则age值不能是23
	     b.没有age
!name2  ：不能name="name2"的属性





## ant风格的请求路径
?  单字符

*  任意个字符（0或多个）
** 任意目录

@RequestMapping(value="welcome3/**/test")
接受示例：

a href="welcome3/abc/xyz/abccc/test"

<a href="handler/welcome5/zs">...</a>

## 通过@PathVariable获取动态参数**
```java
@RequestMapping(value = "welcome5/{name}")
public String  welcome5(@PathVariable("name") String name ) {
	System.out.println(name);
	return "success" ;
}
```





```java
//接口/类、注解、配置
@Controller
@RequestMapping("handler") // 映射
public class SpringMVCHandler {
    @RequestMapping(value = "welcome", method = RequestMethod.POST, params = { "name=zs", "age!=23", "!height" }) // 映射
 public String welcome() {
      return "success"; // /views/success.jsp 
}
    @RequestMapping(value = "welcome2", headers = {
        "Accept=text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8", "Accept-Encoding=gzip, deflate" }) // 映射
    public String welcome2() {
        return "success"; // /views/success.jsp 
    }
    @RequestMapping(value = "welcome3/**/test") 
public String welcome3() {
	return "success"; // /views/success.jsp 
}
@RequestMapping(value = "welcome4/{name}") // 映射
public String welcome4(@PathVariable("name") String name) {
  	System.out.println(name);
	return "success"; // /views/success.jsp
    }
}
```

# 请求方式和获取参数

REST风格 ：软件编程风格

Springmvc:  
GET  :查
POST  ：增
DELETE ：删
PUT ：改

普通浏览器 只支持get post方式 ；其他请求方式 如 delelte|put请求是通过 过滤器新加入的支持。

springmvc实现 ：put|post请求方式的步骤
a.增加过滤器
```xml
<!-- 增加HiddenHttpMethodFilte过滤器：目的是给普通浏览器 增加 put|delete请求方式 -->
<filter>
	<filter-name>HiddenHttpMethodFilte</filter-name>
	<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
	<filter-name>HiddenHttpMethodFilte</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

b.表单

```xml
<form action="handler/testRest/1234" method="post">
	<input type="hidden"  name="_method" value="DELETE"/>
	<input type="submit" value="删">
</form>
```
i:必须是post方式
ii:**通过隐藏域 的value值 设置实际的请求方式 DELETE|PUT**

c.控制器

```java
@RequestMapping(value="testRest/{id}",method=RequestMethod.DELETE)
public String  testDelete(@PathVariable("id") Integer id) {
	System.out.println("delete：删 " +id);
	//Service层实现 真正的增
	return "success" ;//默认使用了请求转发的跳转方式
}
```

通过	method=RequestMethod.DELETE	匹配具体的请求方式



**此外，可以发现 ，当映射名相同时@RequestMapping(value="testRest)，可以通过method处理不同的请求。**


过滤器中 处理put|delete请求的部分源码：


```java
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {
HttpServletRequest requestToUse = request;
	if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {
		String paramValue = request.getParameter(this.methodParam);
		if (StringUtils.hasLength(paramValue)) {
			requestToUse = new HttpMethodRequestWrapper(request, paramValue);
		}
	}
	filterChain.doFilter(requestToUse, response);
}
```
原始请求：request，改请求默认只支持get post  header
但是如果 是"POST"  并且有隐藏域		<input type="hidden"  name="_method" value="DELETE"/>
则，过滤器 将原始的请求 request加入新的请求方式DELETE，并将原始请求 转为 requestToUse 请求（request+Delete请求）
最后将requestToUse 放入 请求链中， 后续再事情request时  实际就使用改造后的 requestToUse

## 获取参数方法2

@RequestParam("uname") String name,@RequestParam(value="uage",required=false,defaultValue="23")

@RequestParam("uname"):接受前台传递的值，等价于request.getParameter("uname");

required=false:该属性 不是必须的。
defaultValue="23"：默认值23





## 获取请求头信息 @RequestHeader
public String  testRequestHeader(@RequestHeader("Accept-Language")  String al  ) {
		
通过@RequestHeader("Accept-Language")  String al   获取请求头中的Accept-Language值，并将值保存再al变量中 

### 通过mvc获取cookie值（JSESSIONID）
@CookieValue 
(前置知识： 服务端在接受客户端第一次请求时，会给该客户端分配一个session （该session包含一个sessionId）),并且服务端会在第一次响应客户端时 ，请该sessionId赋值给JSESSIONID 并传递给客户端的cookie中

小结：
## SpringMVC处理各种参数的流程/逻辑：
请求：  前端发请求a-> @RequestMappting("a") 
处理请求中的参数xyz：
	@RequestMappting("a") 
	public String  aa(@Xxx注解("xyz")  xyz)
	{

	}

## 使用对象（实体类Student）接受请求参数

```java
	@RequestMapping(value = "testObjectProperties") // 映射
public String testObjectProperties(Student student) { //student属性必须和from表单中的属性name值一致（支持级联属性）
	System.out.println(student);
	return "success"; 
}
```

```xml
<form action="handler/testObjectProperties" method="post">
	id:<input type="text" name="id"><br/>
	name:<input type="text" name="name"><br/>
	homeAddress:<input type="text" name="address.homeAddress"><br/>
	schoolAddress:<input type="text" name="address.schoolAddress"><br/>
	<input type="submit" value="submit"><br/>
</form>
```



## 在SpringMVC中使用原生态的Servlet API  

HttpServletRequest ：直接将 servlet-api中的类、接口等 写在springMVC所映射的方法参数中即可：

```java
@RequestMapping(value="testServletAPI")
public String testServletAPI(HttpServletRequest  request,HttpServletResponse response) {
	System.out.println(request);
	return "success" ;
}
```





# 1.处理模型数据
如果跳转时需要带数据：V、M,则可以使用以下方式：
ModelAndView、ModelMap  、Map(java.tuil.map)、Model   -**数据放在了request作用域** 

@SessionAttributes、@ModelAttribute

示例：
public String testModel(Model model|	Map<String,Object> m) {

m.put(x,".."); 就会将x对象 放入request域中

```java
@RequestMapping(value = "testModelAndView")
public ModelAndView testModelAndView() { // ModelAndView:既有数据，又有视图
    ModelAndView mv = new ModelAndView("success");
    Student student = new Student();
    student.setId(3);
    student.setName("zs");
    mv.addObject("student", student); // 相当于request.setAtrribute();
    return mv;
}
@RequestMapping(value = "testModelMap") // 映射
public String testModelMap(ModelMap mm) {
    Student student1 = new Student();
    student1.setId(3);
    student1.setName("zs");
    mm.put("student1", student1); // request域
    return "success"; // view
}
```



如何将上述数据放入session中？@SessionAttributes(..)**在类上声明**

```java
@SessionAttributes(value = {"student3","student2"}) //如果在request域中存放studen3,student2对象，则同时放在session域中

```

```java
@SessionAttributes(types ={Student.class,Address.class}) //如果在request域中存放student,address类的对象，则同时放在session域中

```



@ModelAttribute 
i.经常在 更新时使用
ii.在不改变原有代码的基础之上，插入一个新方法。

通过**@ModelAttribute修饰的方法 ，会在每次请求前先执行；**
并且该方法的参数map.put()可以将 对象 放入 即将查询的参数中；
必须满足的约定：
map.put(k,v) 其中的k 必须是即将查询的方法参数 的首字母小写
testModelAttribute(Student xxx)  ，即student；
如果不一致，需要通过@ModelAttribute声明。如下：
	
		

```java
@ModelAttribute//在任何一次请求前，都会先执行@ModelAttribute修饰的方法
// 在请求该类的每个方法前均会被调用的设计是基于一个思想：一个控制器一个功能
public void queryStudentById(Map<String,Object> map) {
    //模拟调用三层查询数据库的操作
    Student student = new Student();
    student.setId(31);
    student.setName("zs");
    student.setAge(23);
    map.put("stu", student) ;//约定：map的key 就是方法参数 类型的首字母小写
}
//修改:Zs-ls
@RequestMapping(value="testModelAttribute")
public String testModelAttribute(@ModelAttribute("stu")Student student) {
    student.setName(student.getName());//将名字修改为ls
    System.out.println(student.getId()+","+student.getName()+","+student.getAge());
    return "success";
}
```


一个Servlet 对应一个功能：
增删改查  对应于 4个Servlet


更新：	Servlet  - SpringMVC的Controller

查询
@ModelAttribute
public void query()
{

}

修改
public String update()
{

}

**@ModelAttribute会在 该类的每个方法执行前 均被执行一次，因为使用时需要注意。**

# 2.视图、视图解析器

视图的顶级接口:View
视图解析器：ViewResolver

常见的视图和解析器：
InternalResourceView、InternalResourceViewResolver

public class JstlView extends InternalResourceView：

springMVC解析jsp时 会默认使用InternalResourceView， 
**如果发现Jsp中包含了jstl语言相关的内容，则自动转为JstlView。**



JstlView 可以解析jstl\实现国际化操作

国际化： 针对不同地区、不同国家 ，进行不同的显示 

中国:（大陆、香港）     欢迎
美国：			welcome  



i18n_zh_CN.properties		
resource.welcome=你好
resource.exit=退出

i18n.properties	



具体实现国际化步骤：
a.创建资源文件
基名_语言_地区.properties
基名_语言.properties

**常见的资源文件命名**

| 资源文件名            | 简介                                                         |
| --------------------- | ------------------------------------------------------------ |
| 基名_en.properties    | 所有英文语言的资源                                           |
| 基名_en_US.properties | 针对美国地区、英文语言的资源                                 |
| 基名_zh.properties    | 所有的中文语言的资源                                         |
| 基名_zh_CN.properties | 针对中国大陆的、中文语言的资源                               |
| 基名_zh_HK.properties | 针对中国香港的、中文语言的资源                               |
| 基名.properties       | 默认资源文件。如果请求相应语言的资源文件不存在，将使用此资源文件。例如，若是中国大陆地区用户，应该访问“基名_zh_CN.properties”，而如果不存在此文件，就会去访问默认的“基名.properties”。 |

b.配置springmvc.xml，加载资源文件

```xml
<!-- 加载国际化资源文件 1.将ResourceBundleMessageSource在程序加载时 加入springmvc： springmvc在启动时，会自动查找一个叫messageSource的bean，如果有则自动加载 
		2.如果配置了ResourceBundleMessageSource，程序会在响应时介入 -->
<bean id="messageSource"	class="org.springframework.context.support.ResourceBundleMessageSource">
		<property name="basename" value="i18n"></property>
</bean>
```
ResourceBundleMessageSource会在springmvc响应程序时 介入（解析国际化资源文件）

c.通过jstl使用国际化
	jstl.jar  standar.jar
	
springmvc在启动时，会自动查找一个id="messageSource"的bean，如果有  则自动加载

# InternalResourceViewResolver其他功能：
## 1.<mvc:view-controller ...>
index.jsp -> Controller(@RequsetMapping("a")) ->succes.jsp

要用SpringMVC实现：index.jsp -> succes.jsp  ：

```xml
<!-- view-name也会被视图解析器添加前缀和后缀 -->
<mvc:view-controller path="testMvcViewController" view-name="success"/>
```

**以上注解 ，会让所有的请求 转入<mvc:..>中匹配映射地址，而会忽略调@RequsetMapping()；**
如果想让 @RequsetMapping("a")  和<mvc:..>共存，则需要加入一个注解：<```>

```xml
<!--此配置是springmvc的基础配置，很多功能都需要该注解来调整  -->
<mvc:annotation-driven></mvc:annotation-driven>
```



## 2.指定请求方式

指定跳转方式：return "forward:/views/success.jsp";    

**forward(请求转发):   redirect（重定向）: ，需要注意 此种方式，不会被视图解析器加上前缀(/views)、后缀(.jsp)**

## 3.处理静态资源：html css js  图片 视频

可以与用户交互、因为时间/地点的不同 而结果不同的内容：动态（百度：天气  ）

**在SpringMVC中，如果直接访问静态资源：404 。**原因：之前将所有的请求 通过通配符“/” 拦截，进而交给 SPringMVC的入口DispatcherServlet去处理：找该请求映射对应的 @requestMapping

http://localhost:8888/SpringMVCProject/img.png

@RequsetMapping("img.png")
return sucess

解决：如果是 需要mvc处理的，则交给@RequsetMapping("img.png")处理；如果不需要springmvc处理，则使用 tomcat默认的Servlet去处理。
tomcat默认的Servlet去处理：如果有 对应的请求拦截,则交给相应的Servlet去处理；**如果没有对应的servlet，则直接访问。**
tomcat默认的Servlet在哪里？**在tomcat配置文件\conf\web.xml中**

	<servlet>
		<servlet-name>abc</servlet-name>
		<servlet-class>xxx.xxx.xx.ABCServlet</servlet-class>
	</servlet>
	
	<servlet-mapping>
		<servlet-name>abc</servlet-name>
		<url-pattern>/abc</url-pattern>
	</servlet-mapping>

解决静态资源方案：如果有springmvc对应的@requestMapping则交给spring处理；如果没有对应@requestMapping,则交给服务器tomcat默认的servlet去处理  ：实现方法，只需要增加2个注解即可 springmvc.xml： 

```xml
<!-- 该注解会让springmvc在接受请求没有对应映射时，将该请求交给服务器默认的servlet处理(直接访问) -->
<mvc:default-servlet-handler/>
<!--此配置是springmvc的基础配置，很多功能都需要该注解来调整  -->
<mvc:annotation-driven></mvc:annotation-driven>
```




总结：要让springmvc访问静态资源，只需要加入以下2个注解：

```xml
<!-- 该注解会让springmvc在接受请求没有对应映射时，将该请求交给服务器默认的servlet处理(直接访问) -->
<mvc:default-servlet-handler/>
<!--此配置是springmvc的基础配置，很多功能都需要该注解来调整  -->
<mvc:annotation-driven></mvc:annotation-driven>
```



## 4.类型转换

a.Spring自带一些 常见的类型转换器：
public String  testDelete(@PathVariable("id") String id) ，即可以接受int类型数据id  也可以接受String类型的id

b.可以自定义类型转换器
i.编写 自定义类型转器的类 （实现Converter接口）

```java
public class MyConverter  implements Converter<String,Student>{
@Override
    public Student convert(String source) {//source:2-zs-23
        //source接受前端传来的String:2-zs-23
        String[] studentStrArr = source.split("-") ;
        Student student = new Student();
        student.setId(  Integer.parseInt(  studentStrArr[0]) );
      student.setName(studentStrArr[1]);
      student.setAge(Integer.parseInt(studentStrArr[2] ));
      return student;
  }
}
```




ii.配置：将MyConverter加入到springmvc中
```xml
<!-- 1将 自定义转换器 纳入SpringIOC容器 -->
	<bean  id="myConverter" class="org.lanqiao.converter.MyConverter"></bean>
<!-- 2将myConverter再纳入 SpringMVC提供的转换器Bean -->
<bean id="conversionService"  class="org.springframework.context.support.ConversionServiceFactoryBean">
	<property name="converters">
		<set>
			<ref bean="myConverter"/>
		</set>
	</property>
</bean>

<!-- 3将conversionService注册到annotation-driven中 -->
<!--此配置是SpringMVC的基础配置，很功能都需要通过该注解来协调  -->
<mvc:annotation-driven conversion-service="conversionService"></mvc:annotation-driven>
```

测试转换器：
	
			
```java
@RequestMapping(value="testConverter")
public String testConverter(@RequestParam("studentInfo")  Student student) {// 前端：2-zs-23  		System.out.println(student.getId()+","+student.getName()+","+student.getAge());	
		return "success";
	}
```



其中@RequestParam("studentInfo")是触发转换器的桥梁：
@RequestParam("studentInfo")接受的数据 是前端传递过来的：2-zs-23  ，但是 需要将该数据 复制给 修饰的目的对象Student；因此SPringMVC可以发现 接收的数据 和目标数据不一致，并且 这两种数据分别是 String、Student,正好符合public Student convert(String source)转换器。

## 5.数据格式化
​	SimpleDateForamt sdf = new SimpleDateFormat("yyyy-MM-dd  hh:mm:ss");
SPringMVC提供了很多注解，方便我们数据格式化
实现步骤：
a.配置

```xml
<!-- 	配置数据格式化注解所依赖的bean
FormattingConversionServiceFactoryBean既能实现数据日期格式化，又能实现类型转换 -->
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters" >
        <set>
            <ref bean="myConverter"/>
        </set>
    </property>
</bean> 
```

b.通过注解使用

```java
@DateTimeFormat(pattern = "yyyy-MM-dd") //接受前台传来的数据，转换为日期
private Date birthday;//相应实体bean的属性
```

```java
@RequestMapping(value="testDateTimeStringFormat")
//如果student的格式化错误，会将错误转入BindingResult中
public String testDateTimeStringFormat(Student student,BindingResult result) {
    System.out.println(student);
    if(result.getErrorCount() > 0) {
    for (FieldError error : result.getFieldErrors()) {
           System.out.println(error.getDefaultMessage());
        }
    }
    return "success";
}
```

@NumberFormat(parttern="###,#")  

# 1.错误消息：
public String testDateTimeFormat(Student student, BindingResult result ,Map<String,Object> map) {
需要验证的数据是 Student中的birthday, SPringMVC要求 如果校验失败  **则将错误信息自动放入该对象之后紧挨着的	BindingResult中。**
**即Student student, BindingResult result之间 不能有其他参数。**

如果要将控制台的错误消息 传到jsp中显示，则可以将 错误消息对象放入request域中，然后 在jsp中 从request中获取。



# 2.数据校验   
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

```java
public class Student {

​```
@Past//当前时间以前
private Date birthday ;
​```

}
```



​	

在校验的Controller中 ，给**校验的对象前增加 @Valid**

```java
public String testDateTimeFormat(@Valid Student student, BindingResult result ,Map<String,Object> map) {
    {...}
```



# 3.Ajax请求SpringMVC，并且JSON格式的数据
a.jar
jackson-annotations-2.8.9.jar
jackson-core-2.8.9.jar
jackson-databind-2.8.9.jar


b。
@ResponseBod修饰的方法，会将该方法的返回值 以一个json数组的形式返回给前台


​			
```java
@ResponseBody//告诉SpringMVC，此时的返回 不是一个 View页面，而是一个 ajax调用的返回值（Json数组）
		@RequestMapping(value="testJson")
		public List<Student> testJson() {
			//Controller-Service-dao
			//StudentService studentService = new StudentServiceImp();
//			List<Student> students =  studentService.qeuryAllStudent();
			//模拟调用service的查询操作
...
		List<Student> students = new ArrayList<>();
		students.add(stu1) ;
		students.add(stu2) ;
		students.add(stu3) ;
		
		return students;
	}
```

前台：服务端将返回值结果 以json数组的形式 传给了result。

```javascript
$("#testJson").click(function(){
    //通过ajax请求springmvc
    $.post(
        "handler/testJson",//服务器地址
        //{"name":"zs","age":23}
        function(result){//服务端处理完毕后的回调函数 List<Student> students， 加上@ResponseBody后， students实质是一个json数组的格式
            for(var i=0;i<result.length ;i++){
                alert(result[i].id +"-"+result[i].name +"-"+result[i].age);
            }
        }
    );
```



# 1.SpringMVC实现文件上传：
和Servlet方式的本质一样，都是通过commons-fileupload.jar和commons-io.jar
SpringMVC可以简化文件上传的代码，但是必须满足条件：实现MultipartResolver接口 ；而该接口的实现类SpringMVC也已经提供了CommonsMultipartResolver

具体步骤：（直接使用CommonsMultipartResolver实现上传）
a.jar包
commons-fileupload.jar、commons-io.jar
b.配置CommonsMultipartResolver
将其加入SpringIOC容器

```xml
<!-- 配置CommonsMultipartResolver,用于实现文件上传
  springIoc容器在初始化时，会自动Id="multipartResolver"的bean，并自动加入容器中
  -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="utf-8"></property>
    <!-- 上传单个文件的最大值，单位Byte;如果-1，表示无限制 -->
    <property name="maxUploadSize"  value="102400"></property>
</bean>
```

c.处理方法

```java
//文件上传处理方法
@RequestMapping(value="testUpload") //abc.png
public String testUpload(@RequestParam("desc") String desc  , @RequestParam("file") MultipartFile file  ) throws IOException {
    System.out.println("文件描述信息："+desc);
    //jsp中上传的文件：file
    InputStream input = file.getInputStream() ;//IO
    String fileName = file.getOriginalFilename() ;
    OutputStream out = new FileOutputStream("d:\\"+fileName) ;
    byte[] bs = new byte[1024];
    int len = -1;
    while(( len = input.read(bs)) !=-1 ) {
        out.write(bs, 0, len);
    }
    out.close();
    input.close();
    //将file上传到服务器中的 某一个硬盘文件中
    System.out.println("上传成功！");
    return "success";
}
```

```xml
<!-- enctype="mulipart/form-data"告诉浏览器我是要上传文件不是普通的请求 -->
<form action="handler/testUpload" method="post" enctype="mulipart/form-data">
    describe:<input type="text" name="desc"><br />
    file:<input type="file" name="file"><br />
    <input type="submit" value="upload"><br />
</form>
```


框架：  将原来自己写的1000行代码，变成：框架帮你写900行，剩下100行自己写

控制器：handler  servlet   controller   action  





# 2拦截器

	拦截器的原理和过滤器相同。
SpringMVC：要想实现拦截器，必须实现一个接口HandlerInterceptor



**ctrl+shift+r ：自己编写的代码.java  .jsp .html**
**ctrl+shift+t ：jar中的代码**

a.编写拦截器implements HandlerInterceptor
b.配置：将自己写的拦截器 配置到springmvc中（spring）

```xml
<!-- 将自己写的拦截器 配置到springmvc中（spring）,默认拦截全部请求 -->
<mvc:interceptors >
    <bean class="pers.nicolas.interceptor.MyInterceptor"></bean>
</mvc:interceptors>
```

```xml
<!-- 将自己写的拦截器 配置到springmvc中（spring）,默认拦截全部请求 -->
<mvc:interceptors >
    <!-- 配置具体的拦截路径 -->
    <mvc:interceptor>
        <!--指定拦截的路径，ant风格  -->
        <mvc:mapping path="/**"/>
        <!--指定不拦截的路径，ant风格  -->
        <mvc:exclude-mapping path="/handler/testInterceptor"/>
        <bean class="pers.nicolas.interceptor.MyInterceptor"></bean>
    </mvc:interceptor>
	</mvc:interceptors>
```



拦截器1拦截请求- 拦截器2拦截请求 - 请求方法 - 拦截器2处理相应-拦截器1处理相应-    拦截器2的afterCompletion()拦截-拦截器1的afterCompletion()拦截

如果有多个拦截器，则每个拦截器的preHandle postHandle afterCompletion都会在相应时机各被触发一次.



# 3.异常处理
SpringMVC：  **HandlerExceptionResolver接口，**

**该接口的每个实现类 都是异常的一种处理方式：**

## a.@ExceptionHandler注解
ExceptionHandler,ExceptionResolver： 主要提供了@ExceptionHandler注解，并通过该注解处理异常

```java
//该方法 可以捕获本类中  抛出的ArithmeticException异常
@ExceptionHandler({ArithmeticException.class,ArrayIndexOutOfBoundsException.class  })
public String handlerArithmeticException(Exception e) {
	System.out.println(e +"============");
	return "error" ;
}
```

@ExceptionHandler标识的方法的参数 必须在异常类型(Throwable或其子类) ，不能包含其他类型的参数(Model可以，可以查看官方文档)

**异常处理路径：最短（接近的）优先**  
如果有方法抛出一个ArithmeticException异常，而该类中 有2个对应的异常处理法你发：



```java
@ExceptionHandler({Exception.class  })
public ModelAndView handlerArithmeticException2(Exception e) {}

@ExceptionHandler({ArithmeticException.class  })
public ModelAndView handlerArithmeticException1(Exception e) {}
```
则优先级：  **最短（接近的）优先。**

**@ExceptionHandler默认只能捕获 当前类中的异常方法。**
如果发生异常的方法  和处理异常的方法 **不在同一个类中：@ControllerAdvice**

总结：如果一个方法用于处理异常，并且只处理当前类中的异常：@ExceptionHandler
      如果一个方法用于处理异常，并且处理所有类中的异常： 类前加@ControllerAdvice、 处理异常的方法前加@ExceptionHandler



## b.@ResponseStatus
ResponseStatusExceptionResolver：自定义异常显示页面 @ResponseStatus

```java
@ResponseStatus(value=HttpStatus.FORBIDDEN,reason="数组越界222!!!")
public class MyArrayIndexOutofBoundsException extends Exception {//自定义异常

}
@RequestMapping("testMyException")
    public String testMyException(@RequestParam("i") Integer i) throws MyArrayIndexOutofBoundsException {
    if(i == 3) {
        throw new MyArrayIndexOutofBoundsException();//抛出异常
    }
    return "success" ;
}	
```

```java
@RequestMapping("testMyException2")
public String testMyException2(@RequestParam("i") Integer i) {
	if(i == 3) {
		return "redirect:testResponseStatus" ;//跳转到某一个 异常处理方法里
	}
	return "success" ;
}

@ResponseStatus(value = HttpStatus.CONFLICT, reason = "test exception！")
@RequestMapping("testResponseStatus")
public String testResponseStatus(){
    return "success";
}
```
## c.DefaultHandlerExceptionResolver
异常处理的实现类：DefaultHandlerExceptionResolver:SPringMVC在一些常见异常的基础上（300 500  404），新增了一些异常，例如：

* @see org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler
 * @see #handleNoSuchRequestHandlingMethod
 * @see #handleHttpRequestMethodNotSupported  ：如果springmvc的处理方法限制为post方式，如果实际请求为get,则会触发此异常显示的页面
 * @see #handleHttpMediaTypeNotSupported
 * @see #handleMissingServletRequestParameter
 * @see #handleServletRequestBindingException
 * @see #handleTypeMismatch
 * @see #handleHttpMessageNotReadable
 * @see #handleHttpMessageNotWritable
 * @see #handleMethodArgumentNotValidException
 * @see #handleMissingServletRequestParameter
 * @see #handleMissingServletRequestPartException
 * @see #handleBindException



## d.SimpleMappingExceptionResolver
：通过配置来实现异常的处理


```xml
<!--SimpleMappingExceptionResolver:以配置的方式处理异常 -->
<bean  class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <!-- 如果发生异常，异常对象会被保存在  exceptionAttribute的value值中；并且会放入request域中 ；异常变量的默认值是 exception-->
    <!--<property name="exceptionAttribute" value="exception"></property>-->
    <property name="exceptionMappings">
        <props>
            <!-- 相当于catch(ArithmeticException ex){ 跳转：error } -->
            <prop key="java.lang.ArithmeticException">
                error
            </prop>
            <prop key="java.lang.NullPointerException">
                error
            </prop>
        </props>
    </property>
</bean>
```
# SSM整合：
Spring - SpringMVC -  MyBatis 

1.
Spring -  MyBatis   :	需要整合：将MyBatis的SqlSessionFactory 交给Spring

2
Spring - SpringMVC  ：  就是将Spring - SpringMVC 各自配置一遍






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

Student类 -student表

3.-（与Spring整合时，conf.xml可省）--MyBatis配置文件conf.xml（数据源、mapper.xml） --可省，将该文件中的配置 全部交由spring管理

spring配置文件 applicationContext.xml



4.通过mapper.xml将 类、表建立映射关系

5.
之前使用MyBatis:	conf.xml ->SqlSessionFacotry

```xml
<!-- web项目中，spring介入项目 -->
<!-- needed for ContextLoaderListener -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>

<!-- Bootstraps the root web application context before servlet initialization -->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

现在整合的时候，需要通过Spring管理SqlSessionFacotry ，因此 产生qlSessionFacotry 所需要的数据库信息 不在放入conf.xml  而需要放入spring配置文件中

配置Spring配置文件（applicationContext.xml）  （Web项目）：
web.xml

```xml
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```



6.使用Spring整合MyBatis ：将MyBatis的SqlSessionFactory 交给Spring

**注意xml文件路径有classpath:存在,和整合sm最大不同** 

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

<!-- cof.xml: 数据源、mapper.xml -->
<!-- 在SpringIoc容器中 创建Mybatis的核心类SqlSessionFactory -->
<bean id="sqlSessionFactory"
      class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <!-- 加载mapper.xml配置文件 -->
       <!-- 注意有classpath:存在,和整合sm最大不同 -->  
    <property name="mapperLocations"
        value="classpath:pers/nicolas/mapper/*.xml"></property>
</bean>

<!-- 将mybatis的SqlSessionFactory交给spring -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="sqlSessionFactoryBeanName"
              value="sqlSessionFactory"></property>
    <!-- xxxxMapper -->
    <property name="basePackage" value="pers.nicolas.mapper"></property>
</bean>
```




7.继续整合SpringMVC：将springmvc加入项目即可 
a.加入SpringMVC需要的jar
spring-webmvc.jar

b.给项目加入SpringMVC支持
web.xml: dispatcherServlet（alt+/）

```xml
<!-- web项目整合springmvc -->
<!-- The front controller of this Spring Web application, responsible for handling all application requests -->
<servlet>
    <servlet-name>springDispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext-controller.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<!-- Map all requests to the DispatcherServlet for handling -->
<servlet-mapping>
    <servlet-name>springDispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```



c.编写springmvc配置文件：
applicationContext-controller.xml：视图解析器、基础配置

```xml
<!-- 将控制器所在的包加入springIOC容器  -->
<context:component-scan base-package="pers.nicolas.controller"></context:component-scan>
<!-- 配置视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/views/"></property>
    <property name="suffix" value=".jsp"></property>
</bean>

<!-- springMVC基础配置、标配 -->
<mvc:annotation-driven></mvc:annotation-driven>
```




d.示例