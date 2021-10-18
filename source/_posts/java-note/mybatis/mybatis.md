---
title: mybatis
p: java-note/mybatis/mybatis
date: 2019-10-12 10:40:06
categories: java
tags: [java,mybatis]
---
[TOC]

# 入门

## mybatis约定：

输入参数parameterType 和 输出参数resultType ，在形式上都只能有一个

如果输入参数 ：是简单类型（8个基本类型+String） 是可以使用任何占位符,#{xxxx}
	       如果是对象类型，则必须是对象的属性 #{属性名}

输出参数：  如果返回值类型是一个 对象（如Student），则无论返回一个、还是多个，
		再resultType都写成org.lanqiao.entity.Student
		即 resultType="org.lanqiao.entity.Student"



## 注意事项：

如果使用的 事务方式为 jdbc,则需要 手工commit提交，即session.commit();

##	

## conf.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 通过environment的default值和environment的id值指定Mybatis数据库的运行环境 -->
	<environments default="development">
		<!-- development environment -->
		<environment id="development">
		
		<!-- 事务提交方式：
			JDBC：使用JDBC方式提交事务（commit rollback close）
			MANAGED:将事务交给其他组件去托管（spring，jobss）默认自动关闭
			设置不关闭：
			<transactionManager type="MANAGED" />
				property name="closeConnection" value="false" />
		 -->
			<transactionManager type="JDBC" />
			<!-- 数据源类型
				UNPOOLED:传统jdbc方式(每次访问数据，均需要打开和关闭等数据库操作，但是打开和关闭都是很浪费资源和性能的）
				POLLED：使用数据库连接池
				JNDI：从tomcat中获取一个内置数据库连接池（数据库连接池-数据源）
			 -->
			<dataSource type="POOLED">
				<!-- 配置数据库信息 -->
				<property name="driver" value="oracle.jdbc.OracleDriver" />
				<property name="url" value="jdbc:oracle:thin:@localhost:1521:MLDN" />
				<property name="username" value="scott" />
				<property name="password" value="tigger" />
			</dataSource>
		</environment>
		<!--  test environment-->
		<environment id="test">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<!-- 配置数据库信息 -->
				<property name="driver" value="oracle.jdbc.OracleDriver" />
				<property name="url" value="jdbc:oracle:thin:@111:11:11:1521:MLDN" />
				<property name="username" value="scott" />
				<property name="password" value="tigger" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<!-- 加载映射文件 -->
		<mapper resource="pers/nicolas/entity/studentMapper.xml" />
	</mappers>
</configuration>
```

## mapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?> 
<!DOCTYPE mapper  
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace：该mapper.xml映射文件的唯一标识 -->
<mapper namespace="pers.nicolas.entity.studnetMapper">
	<!-- 后续通过namespace.Id 定位该SQL语句-->
	<!-- parameterType：输入参数的类型
		resultType：查询结果的返回类型
	 -->
	<select id="queryStudentBysno" parameterType="int" resultType="pers.nicolas.entity.Student">
		select * from student where stuno = #{stuno}<!-- 不能有分号！！！无效字符 -->
	</select>
	<insert id="addStudent" parameterType="pers.nicolas.entity.Student" >
		insert into student(stuno,stuname,stuage,graname) values(#{stuNo},#{stuName},#{stuAge},#{graName})
	</insert>
	<delete id="deleteStudentBySno" parameterType="int">
		delete from student where stuno=#{stuno}
	</delete>
	<update id="updateStudentBySno" parameterType="pers.nicolas.entity.Student">
		update student set stuno=#{stuNo}, stuname = #{stuName}, stuage=#{stuAge},graname=#{graName}
	</update>
	<select id="queryAllStudents" resultType="pers.nicolas.entity.Student">
		select * from student
	</select>
</mapper>
```

## mybatis约定：

输入参数parameterType 和 输出参数resultType ，在形式上都只能有一个


如果输入参数 ：是简单类型（8个基本类型+String） 是可以使用任何占位符,#{xxxx}
	       如果是对象类型，则必须是对象的属性 #{属性名}

输出参数：  如果返回值类型是一个 对象（如Student），则无论返回一个、还是多个，
		再resultType都写成org.lanqiao.entity.Student
		即 resultType="org.lanqiao.entity.Student"



注意事项：
如果使用的 事务方式为 jdbc,则需要 手工commit提交，即session.commit();

## mapper动态代理方式的crud 

又称**（MyBatis接口开发）**

原则：约定优于配置 

硬编码方式
	abc.java
		Configuration conf = new Configuration();
		con.setName("myProject") ;

配置方式：
	abc.xml   
		<name>myProject</name>

约定：默认值就是myProject

## 具体实现的步骤：
1.基础环境：mybatis.jar/ojdbc.jar、conf.xml、mapper.xml
2.（不同之处）
	约定的目标： 省略掉statement,即根据约定 直接可以定位出SQL语句

  a.接口，接口中的方法必须遵循以下约定：
		

 1.方法名和mapper.xml文件中标签的id值相同
		 

2 .方法的 输入参数 和mapper.xml文件中标签的 parameterType类型一致 (如果mapper.xml的标签中没有 parameterType，则说明方法没有输入参数)
		 

3.方法的返回值  和mapper.xml文件中标签的 resultType类型一致 （无论查询结果是一个 还是多个（student、List<Student>），在mapper.xml标签中的resultType中只写 一个（Student）；如果没有resultType，则说明方法的返回值为void）

除了以上约定，要实现 接口中的方法  和  Mapper.xml中SQL标签一一对应，还需要以下1点：
	**namespace的值 ，就是  接口的全类名（ 接口 - mapper.xml 一一对应）**


匹配的过程：（约定的过程）
1.根据 接口名 找到 mapper.xml文件（根据的是namespace=接口全类名）
2.根据 接口的方法名 找到 mapper.xml文件中的SQL标签 （方法名=SQL标签Id值）

以上2点可以保证： 当我们调用接口中的方法时，
程序能自动定位到 某一个Mapper.xml文件中的sqL标签


习惯：SQL映射文件（mapper.xml） 和 接口放在同一个包中 （注意修改conf.xml中加载mapper.xml文件的路径）


以上，可以通过接口的方法->SQL语句

执行：
		StudentMapper studentMapper = session.getMapper(StudentMapper.class) ;
		studentMapper.方法();

通过session对象获取接口（session.getMapper(接口.class);），再调用该接口中的方法，程序会自动执行该方法对应的SQL。

# 配置

## 别名

https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases

```xml
<typeAliases>
    <typeAlias alias="Problem" type="com.yoj.web.bean.Problem"/>
    <package name="com.yoj.web.bean"/>
</typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean，比如：

```
<typeAliases>
  <package name="domain.blog"/>
</typeAliases>
```

每一个在包 `domain.blog` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 `domain.blog.Author` 的别名为 `author`；若有注解，则别名为其注解值。见下面的例子：

```java
@Alias("author")
public class Author {
    ...
}
```



# 类型处理器（类型转换器）

1.MyBatis自带一些常见的类型处理器
	int  - number

2.自定义MyBatis类型处理器

```
java -数据库(jdbc类型)
```

示例：
实体类Student :  boolean   stuSex  	
			true:男
			false：女

表student：	number  stuSex
			1:男
			0：女
自定义类型转换器（boolean -number）步骤：
a.创建转换器：需要实现TypeHandler接口
	通过阅读源码发现，此接口有一个实现类 BaseTypeHandler ，因此 要实现转换器有2种选择：
	i.实现接口TypeHandler接口
	ii.继承BaseTypeHandler
b.配置conf.xml

需要注意的问题：  INTEGER(需要大写)



```xml
insert into student(stuno,stuname,stuage,graname,stusex) values(#{stuNo},#{stuName},#{stuAge},#{graName} ,#{stuSex ,javaType=boolean  ,jdbcType=INTEGER   } ) 
```

注意#{stuNo} 中存放的是 属性值，需要严格区分大小写。



resultMap可以实现2个功能：
1.类型转换
2.属性-字段的映射关系



<select id="queryStudentByStuno" 	parameterType="int"  	resultMap="studentMapping" >
		select * from student where stuno = #{stuno}
	</select>

```xml
<resultMap type="student" id="studentMapping">
		<!-- 分为主键id 和非主键 result-->
		<id property="id"  column="stuno"  />
		<result property="stuName"  column="stuname" />
		<result property="stuAge"  column="stuage" />
		<result property="graName"  column="graname" />
		<result property="stuSex"  column="stusex"  javaType="boolean" jdbcType="INTEGER"/>
 </resultMap>
```

# mybatis调用存储过程

```xml

<select id="queryCountByGradeWithProcedure" statementType="CALLABLE"  parameterType="HashMap" >
	{
		CALL queryCountByGradeWithProcedure(
			#{gName,jdbcType=VARCHAR,mode=IN},
			#{scount,jdbcType=INTEGER,mode=OUT}
		) 
	}	
</select>
```
其中 通过statementType="CALLABLE"设置SQL的执行方式是存储过程。 存储过程的输入参数gName需要通过HashMap来指定
在使用时，通过hashmap的put方法传入输入参数的值；通过hashmap的Get方法 获取输出参数的值。
要注意Jar问题：ojdbc6.jar不能在 调存储过程时  打回车、tab，但是ojdbc7.jar可以。


如果报错： No enum constant org.apache.ibatis.type.JdbcType.xx，则说明mybatis不支持xx类型，需要查表。

存储过程 无论输入参数是什么值，语法上都需要 用map来传递该值；

只要 是  <transactionManager type="JDBC" />，则增删改都需要手工commit ;

mapper.xml->mapper接口->测试方法

# 输入和输出参数

## 输入参数parameterType

## 1.类型为 简单类型（8个基本类型+String）

### **#{}、${}的区别**
a.
**#{任意值}**
**${value} ，其中的标识符只能是value**

**b.#{}自动给String类型加上''  （自动类型转换）**

  ${} 原样输出，但是适合于 动态排序（动态字段）


select stuno,stuname,stuage  from student where stuname = #{value}

select stuno,stuname,stuage  from student where stuname = '${value}'

动态排序：
select stuno,stuname,stuage  from student  order by ${value} asc

**c.#{}可以防止SQL注入**
  ${}不防止



${}、#{}相同之处：
a.都可以 获取对象的值 （嵌套类型对象）



i.获取对象值：
模糊查询，方式一：

```sql
select stuno,stuname,stuage  from student where stuage= #{stuAge}  or stuname like #{stuName} 
```

```java
			Student student = new Student();
 			student.setStuAge(24);
 			student.setStuName("%w%");
 			List<Student> students = studentMapper.queryStudentBystuageOrstuName(student) ;//接口的方法->SQL
```


模糊查询，方式二：
	student.setStuName("w");
	select stuno,stuname,stuage  from student where stuage= #{stuAge}  or stuname like '%${stuName}%'

ii.嵌套类型对象

## 2.对象类型
#{属性名}
${属性名}




输入对象为HashMap：
where stuage= #{stuAge}

用map中key的值 匹配 占位符#{stuAge}，如果匹配成功 就用map的value替换占位符

## 输出参数resultType

输出参数resultType
1.简单类型（8个基本+String）
2.输出参数为实体对象类型
3.输出参数为实体对象类型的集合 ：虽然输出类型为集合，但是resultType依然写 集合的元素类型（resyltType="Student"）
4.输出参数类型为HashMap
	--HashMap本身是一个集合，可以存放多个元素，
	  但是根据提示发现  返回值为HashMap时  ，查询的结果只能是1个学生（no,name）；
-->结论：一个HashMap 对应一个学生的多个元素（多个属性）  【一个map，一个学生】

二维数组
{
	{1,zs,23,xa},    -一个HashMap对象
	{2,ls,24,bj}, 
	{3,ww,25,tj}
}


resultType
resultMap:实体类的属性、数据表的字段： 类型、名字不同时（stuno,id）
注意：当属性名 和字段名 不一致时，除了使用resultMap以外，还可以使用resultType+HashMap:

a.resultMap
	<resultMap type="student" id="queryStudentByIdMap">
			<!-- 指定类中的属性 和 表中的字段 对应关系 -->
			<id property="stuNo"  column="id" />
			<result property="stuName" column="name" />
	</resultMap>
	
b.resultType+HashMap
select  表的字段名 "类的属性名" from... 来制定字段名 和属性名的对应关系
	<select id="queryStudentByIdWithHashMap" 	 parameterType="int"	resultType="student" >
		select id "stuNo",name "stuName" from student where id = #{id}
	</select>

注意:  如果如果10个字段，但发现 某一个字段结果始终为默认值（0，0.0，null），则可能是 表的字段  和 类的属性名字写错。

//查询全部
String statement = "select stuno,stuname from student";

//根据年龄查询学生
	
String statement = "select stuno,stuname from student where stuage = #{stuage}"; 


//根据姓名和年龄查询学生


String statement = "select stuno,stuname from student where stuage = #{stuage} and stuage = #{stuage} "; 

select stuno,stuname from student where  stuname = #{stuName}and  stuage = #{stuAge}







查询使用了类型转换器 1.如果类中属性和表中字段类型能够合理识别（String-varchar2），则可以使用resultType，(boolean-number)否则使用resultMap； 
		2.如果类中属性名和表中字段名能够合理识别（stuNo-stuno），则可以使用resultType，（stuno-id）否则使用resultMap；

```xml
<!-- 别名作为HashMap的Key -->
	<select id="queryStudentOutByHashMap" resultMap="HashMap">
		select stuno "no", stuname "name" from student 
	</select>
```

```xml
<!--resultMap  -->
	<select id="queryStudentById" parameterType="int"
		resultMap="queryStudentByIdMap">
		select id,name from student where id = #{id}
	</select>
	<resultMap type="Student" id="queryStudentByIdMap">
		<!-- 将类中属性和表中字段对应起来 -->
		<!-- 分为主键id和非主键 result -->
		<id property="stuNo" column="id"/>
		<result property="stuName" column="name"/>
	</resultMap>
```

```xml
<!--resultType  HashMap方式起别名-->
	<select id="queryStudentByIdWithHashMap" parameterType="int"
		resultType="Student">
		select id "stuNo",name "stuName" from student where id = #{id}
	</select>
```

# 动态sql

**[官网地址](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)**

## where

*where* 元素只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“WHERE”子句。而且，若语句的开头为“AND”或“OR”，*where* 元素也会将它们去除。

如果 *where* 元素没有按正常套路出牌，我们可以通过自定义 trim 元素来定制 *where* 元素的功能。比如，和 *where* 元素等价的自定义 trim 元素为：

```xml
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ...
</trim>
```

```xml
<!-- where标签 -->
<select id="queryStudentBySqlTag" parameterType="student" resultType="student">
    select stuno,stuname,stuage,graname from student
    <where>
        <if test="stuName != null and stuName !='' ">
            and stuName = #{stuName}
        </if>
        <if test="stuAge != null and stuAge !=0 ">
            and stuAge = #{stuAge}
        </if>
    </where>
</select>
```



简单类型的数组:
无论编写代码时，传递的是什么参数名(stuNos)，在mapper.xml中 必须用array代替该数组


集合：
无论编写代码时，传递的是什么参数名(stuNos)，在mapper.xml中 必须用list代替该数组


对象数组：
Student[] students = {student0,student1,student2}  每个studentx包含一个学号属性
注意的几点：
	parameterType="Object[]" 
	 	<foreach collection="array" open=" and  stuno in (" close=")" 
	  		 		item="student" separator=",">   
	  		 		#{student.stuNo}
	  	</foreach>



SQL片段：
	java：方法
	数据库：存储过程、存储函数
	Mybatis :SQL片段 

a.提取相似代码
b.引用



### if标签

```xml
<!-- 添加虚拟前缀 -->
<select id="queryStudentBySqlTag" parameterType="student" resultType="student">
		select stuno,stuname,stuage,graname from student where 1=1
		<if test="stuName != null and stuName !='' ">
			and stuName = #{stuName}
		</if>
		<if test="stuAge != null and stuAge !=0 ">
			and stuAge = #{stuAge}
		</if>
	</select>
```

### foreach Tag

```xml
<!-- foreach迭代 使用对象属性-->
	<select id="queryStudentInGrade" parameterType="grade"
		resultType="student">
		select stuno,stuname,stuage,graname from student
		<where>
			<if test="stuNos != null and stuNos.size > 0">
				<foreach collection="stuNos" open="and stuno in(" close=")"
					item="stuNo" separator=",">
					#{stuNo}
				</foreach>
			</if>
		</where>
	</select>
```

```xml
<!-- foreach迭代 使用list集合-->
	<select id="queryStudentWithList" parameterType="list"
		resultType="student">
		select stuno,stuname,stuage,graname from student
		<where>
			<if test="list != null and list.size > 0">
				<foreach collection="list" open="and stuno in(" close=")"
					item="stuNo" separator=",">
					#{stuNo}
				</foreach>
			</if>
		</where>
	</select>
```

```xml
<!-- foreach迭代 使用简单数组-->
	<select id="queryStudentWithArray" parameterType="int[]"
		resultType="student">
		select stuno,stuname,stuage,graname from student
		<where>
			<if test="array != null and array.length > 0">
				<foreach collection="array" open="and stuno in(" close=")"
					item="stuNo" separator=",">
					#{stuNo}
				</foreach>
			</if>
		</where>
	</select>
```

```xml
<!-- foreach迭代 使用对象数组-->
	<select id="queryStudentWithObjectArray" parameterType="Object[]"
		resultType="student">
		select stuno,stuname,stuage,graname from student
		<where>
			<if test="array != null and array.length > 0">
				<foreach collection="array" open="and stuno in(" close=")"
					item="stuNo" separator=",">
					#{stuNo}
				</foreach>
			</if>
		</where>
	</select>
```

```java
public List<Student> queryStudentWithObjectArray(Student[] stuNos);
```

### sql片段

```xml
<sql id="ObjectArraySql">
		select stuno,stuname,stuage,graname from student
		<where>
			<if test="array != null and array.length > 0">
				<foreach collection="array" open="and stuno in(" close=")"
					item="student" separator=",">
					#{student.stuNo}
				</foreach>
			</if>
		</where>
	</sql>

<select id="queryStudentWithObjectArray"
		parameterType="Object[]" resultType="student">
			<!-- 同一文件映射文件下可以省略当前namespace：namesapceId -->
			<include refid="pers.nicolas.mapper.StudentMapper.ObjectArraySql"></include>
	</select>
```

# 查询

## 关联查询：

## 一对一:
a.业务扩展类
		核心：用resultType指定类(**该类扩展了连接查询的所有属性**)的属性 包含 多表查询的所有字段
	
b.resultMap
	i.通过 属性成员 将2个类建立起联系 **->**一对一：association
	2.

```xml
<resultMap type="student" id="student_card_map">
			<!-- 学生的信息 -->
			<id  property="stuNo" column="stuNo"/>
			<result property="stuName" column="stuName" />
			<result property="stuAge" column="stuAge" />
			<!-- 一对一时，对象成员使用 association映射;javaType指定该属性的类型-->
			<association property="card" javaType="StudentCard" >
					<id property="cardId" column="cardId"/>
					<result property="cardInfo" column="cardInfo"/>
			</association>
</resultMap>
```

一对一：association
一对多：collection

## 一对多：

表：student studentclass  (关联：classid)
类：student studentClass  (关联：List<Student> students )

```xml
<!-- 利用resultMap实现一对多 -->
	<select id="queryClassAndStudents" parameterType="int"
		resultMap="class_student_map">
		select s.*,c.* from student s inner join studentclass c
		on
		s.classid = c.classid where c.classid = #{classId}
	</select>
	<!-- 类与表一一对应 -->
	<resultMap type="StudentClass" id="class_student_map">
		<id property="classId" column="classId" />
		<result property="className" column="className" />
		<!-- 一对多；属性类型javaType；（集合）属性的元素类型ofType -->
		<collection property="students" ofType="Student">
			<id property="stuNo" column="stuno" />
			<result property="stuName" column="stuname" />
			<result property="stuAge" column="stuAge" />
			<result property="graName" column="graname" />
			<result property="stuSex" column="stusex" />
			<association property="card" javaType="StudentCard">
				<id property="cardId" column="cardid" />
				<result property="cardInfo" column="cardInfo" />
			</association>
		</collection>
	</resultMap>
```


一对多

（MyBatis:多对一，多对多的本质就是  一对多的变化）

## 多对一

可以传入多个参数**column="{prop1=col1,prop2=col2}"

```xml
<association property="userSolved" javaType="INTEGER"
             select="com.yoj.web.dao.SolutionMapper.querySolved"
             column="{userId = user_id,problemId = problem_id}">
</association>
<association property="userSubmitted" javaType="INTEGER"
             select="com.yoj.web.dao.SolutionMapper.querySubmitted"
             column="{userId = user_id,problemId = problem_id}">
</association>
</resultMap>
```

```java
@Select("SELECT solution_id FROM solution WHERE problem_id = #{problemId} and user_id = #{userId} and result = 0 LIMIT 1")
Integer querySolved(Map<String, Object> map);
```



## 关联的嵌套 Select 查询

可以传入多个参数**column="{prop1=col1,prop2=col2}"**

**注意接收属性为Map**

```xml
<association property="userSolved" javaType="INTEGER"
             select="com.yoj.web.dao.SolutionMapper.querySolved"
             column="{userId = user_id,problemId = problem_id}">
</association>
<association property="userSubmitted" javaType="INTEGER"
             select="com.yoj.web.dao.SolutionMapper.querySubmitted"
             column="{userId = user_id,problemId = problem_id}">
</association>
</resultMap>
```



```java
@Select("SELECT solution_id FROM solution WHERE problem_id = #{problemId} and user_id = #{userId} and result = 0 LIMIT 1")
Integer querySolved(Map<String,Object> map);

@Select("SELECT solution_id FROM solution WHERE problem_id = #{problemId} and user_id = #{userId} LIMIT 1")
Integer querySubmitted(Map<String,Object> map);
```





| 属性        | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| `column`    | 数据库中的列名，或者是列的别名。一般情况下，这和传递给 `resultSet.getString(columnName)` 方法的参数一样。 注意：在使用复合主键的时候，你可以使用 `column="{prop1=col1,prop2=col2}"` 这样的语法来指定多个传递给嵌套 Select 查询语句的列名。这会使得 `prop1` 和 `prop2` 作为参数对象，被设置为对应嵌套 Select 语句的参数。 |
| `select`    | 用于加载复杂类型属性的映射语句的 ID，它会从 column 属性指定的列中检索数据，作为参数传递给目标 select 语句。 具体请参考下面的例子。注意：在使用复合主键的时候，你可以使用 `column="{prop1=col1,prop2=col2}"` 这样的语法来指定多个传递给嵌套 Select 查询语句的列名。这会使得 `prop1` 和 `prop2` 作为参数对象，被设置为对应嵌套 Select 语句的参数。 |
| `fetchType` | 可选的。有效值为 `lazy` 和 `eager`。 指定属性后，将在映射中忽略全局配置参数 `lazyLoadingEnabled`，使用属性的值。 |

示例：

```xml
<resultMap id="blogResult" type="Blog">
  <association property="author" column="author_id" javaType="Author" select="selectAuthor"/>
</resultMap>

<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>

<select id="selectAuthor" resultType="Author">
  SELECT * FROM AUTHOR WHERE ID = #{id}
</select>
```

就是这么简单。我们有两个 select 查询语句：一个用来加载博客（Blog），另外一个用来加载作者（Author），而且博客的结果映射描述了应该使用 `selectAuthor` 语句加载它的 author 属性。

其它所有的属性将会被自动加载，只要它们的列名和属性名相匹配。

这种方式虽然很简单，但在大型数据集或大型数据表上表现不佳。这个问题被称为“N+1 查询问题”。 概括地讲，N+1 查询问题是这样子的：

- 你执行了一个单独的 SQL 语句来获取结果的一个列表（就是“+1”）。
- 对列表返回的每条记录，你执行一个 select 查询语句来为每条记录加载详细信息（就是“N”）。

这个问题会导致成百上千的 SQL 语句被执行。有时候，我们不希望产生这样的后果。

好消息是，MyBatis 能够对这样的查询进行延迟加载，因此可以将大量语句同时运行的开销分散开来。 然而，如果你加载记录列表之后立刻就遍历列表以获取嵌套的数据，就会触发所有的延迟加载查询，性能可能会变得很糟糕。

## 模糊查询

使用#{}，${}容易sql注入

使用concat函数拼接

```xml
<select id="getProblemList" parameterType="com.yoj.web.bean.Problem" resultMap="ProblemList">
  select problem_id,title,#{userId} as user_id from problem
  <where>
      <if test="problemId != null">
          and problem_id LIKE CONCAT('%',#{problemId},'%')
      </if>
      <if test="title != null">
          and title LIKE  CONCAT('%',#{title},'%')
      </if>
  </where>
</select>
```
# 日志：Log4j



a.Log4j:	log4j.jar (mybatis.zip中lib中包含此jar)
b.开启日志，conf.xml

```xml
<settings>
		<!-- 开启日志，并指定使用的具体日志 -->
		<setting name="logImpl" value="LOG4J"/>
</settings>
```

如果不指定，Mybatis就会根据以下顺序 寻找日志
SLF4J →Apache Commons Logging →Log4j 2 → Log4j →JDK logging

c.编写配置日志输出文件

log4j.properties，内容

```txt
log4j.rootLogger=DEBUG, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```



### 日志级别：
​	DEBUG<INFO<WARN<ERROR
如果设置为info，则只显示 info及以上级别的信息；
建议：在开发时设置debug，在运行时设置为info或以上。



可以通过日志信息，相信的阅读mybatis执行情况（ 观察mybatis实际执行sql语句 以及SQL中的参数 和返回结果）

# 延迟加载（懒加载）：

一对一、一对多、多对一、多对多
一对多：班级-学生 ，
如果不采用延迟加载  （立即加载），查询时会将 一 和多 都查询，班级、班级中的所有学生。
如果想要  暂时只查询1的一方，  而多的一方 先不查询 而是在需要的时候再去查询 -->延迟加载


一对一：学生、学生证

### mybatis中使用延迟加载，需要先配置：

```xml
<settings>
		<!-- 开启延迟加载 -->
		<setting name="lazyLoadingEnabled" value="true"/>
		<!-- 关闭立即加载 -->
		<setting name="aggressiveLazyLoading" value="false"/>
</settings>
```

**如果增加了mapper.xml ,要修改conf.xml配置文件（将新增的mapper.xml加载进去）**

通过debug可以发现， 如果程序只需要学生，则只向数据库发送了查询学生的SQL；
当我们后续 需要用到学生证的时候，再第二次发送 查询学生证的SQL。

```xml
<mapper namespace="pers.nicolas.mapper.StudentCardMapper">
	<!-- 后续通过namespace.Id 定位该SQL语句 -->
	<!-- parameterType：输入参数的类型 resultType：查询结果的返回类型 -->
	<!--  根据cardId查学生证信息-->
	<select id="qeuryCardById" parameterType="int" resultType="studentCard">
		select * from studentcard where cardid = #{cardId}
	</select>


<!-- 延迟加载 -->
	<select id="queryStudentWithLazyLoad"
	resultMap="student_card_lazyload_map">
		<!-- 先查学生 -->
		select * from student 
	</select>
	<resultMap type="student" id="student_card_lazyload_map">
		<id property="stuNo" column="stuno" />
		<result property="stuName" column="stuname" />
		<result property="stuAge" column="stuAge" />
		<result property="graName" column="graname" />
		<result property="stuSex" column="stusex" />
		<!-- 延迟加载： 在加载学生时，并不立即加载学生证信息 通过调用sql column外键传参 -->
		<association property="card" javaType="StudentCard"
			select="pers.nicolas.mapper.StudentCardMapper.qeuryCardById"
			column="cardid">
		</association>
	</resultMap>
```




一对多：和一对一的延迟加载配置方法相同



延迟加载的步骤：先查班级，按需查询学生
1.开启延迟加载conf.xml配置settings
2.配置mapper.xml
	写2个Mapper:
	班级mapper.xml

```xml
	<select id="queryClassAndStudents"   resultMap="class_student_lazyLoad_map">
        select  c.* from studentclass c
</select>
<resultMap type="studentClass" id="class_student_lazyLoad_map">
		<!-- 因为 type的主类是班级，因此先配置班级的信息-->
		<id  property="classId" column="classId"/>
		<result  property="className" column="className"/>
		<!-- 配置成员属性学生，一对多;属性类型：javaType，属性的元素类型ofType -->
		<!-- 2222222再查班级对应的学生 -->
		<collection property="students" ofType="student" select="org.lanqiao.mapper.StudentMapper.queryStudentsByClassId" column="classid">

​```
	</collection>

​```

</resultMap>

​```
即查询 学生的sql是通过 select属性指定，并且通过column指定外键

​```

学生mapper.xml
<!-- 一对多,延迟加载需要的： 查询班级中的所有学生 -->

<select id="queryStudentsByClassId" parameterType="int" resultType="student">
	select * from student where classId = #{classId}
</select>
```



# 查询缓存

## 一级缓存 ：

同一个SqlSession对象

​	  MyBatis默认开启一级缓存，如果用同样的SqlSession对象查询相同的数据，
​	则只会在第一次 查询时 向数据库发送SQL语句，并将查询的结果 放入到

SQLSESSION中（作为缓存存在）；
	后续再次查询该同样的对象时，
	则直接从缓存中查询该对象即可（即省略了数据库的访问）	

![1561619500501](E:\pictures\Language\mybatis\一级缓存1.png)

## 二级缓存

​	MyBatis默认情况没有开启二级缓存，需要手工打开。
​	a.conf.xml

```xml
<!-- 开启二级缓存 -->
	<setting name="cacheEnabled" value="true"/>
```



​	b.在具体的mapper.xml中声明开启(studentMapper.xml中)

```xml
<mapper namespace="org.lanqiao.mapper.StudentMapper">
    <!-- 声明次namespace开启二级缓存 -->
<cache/>
```


	根据异常提示：NotSerializableException可知，MyBatis的二级缓存 是将对象 放入硬盘文件中	
		序列化：内存->硬盘
		反序列化：硬盘->内存
准备缓存的对象，必须实现了序列化接口 （如果开启的缓存Namespace="org.lanqiao.mapper.StudentMapper"），可知序列化对象为Student，因此需要将Student序列化 （序列化Student类，以及Student的级联属性、和父类）

**触发将对象写入二级缓存的时机：SqlSession对象的close()方法。**




		Mybatis自带二级缓存：【同一个namespace】生成的mapper对象
回顾：namespace的值 就是 接口的全类名（包名.类名）， 通过接口可以产生代理对象（studentMapper对象）

-->namespace决定了studentMapper对象的产生
结论：只要产生的xxxMapper对象 来自于同一个namespace，则 这些对象 共享二级缓存。
注意：二级缓存 的范围是同一个namespace, 如果有多个xxMapper.xml的namespace值相同，则通过这些xxxMapper.xml产生的xxMapper对象 仍然共享二级缓存。

### 禁用 ：

select标签中useCache="false"

### 清理：

a.与清理一级缓存的方法相同

**commit();** （一般执行增删改时 会清理掉缓存；设计的原因 是为了防止脏数据）
在二级缓存中，commit()不能是查询自身的commit。

**commit会清理一级和二级缓存；但是 清理二级缓存时，不能是查询自身的commit；**
  b. 在select标签中 增加属性 flushCache="true"

命中率：
	1:zs :0%  
	2:    50%
	3:   2/3    0.666
	4:  3/4     0.75

### 三方提供的二级缓存：

ehcache、memcache

要想整合三方提供的二级缓存 （或者自定义二级缓存），必须实现org.apache.ibatis.cache.Cache接口，该接口的默认实现类是PerpetualCache

整合ehcache二级缓存：
a.
ehcache-core.jar
mybatis-Ehcache.jar
slf4j-api.jar

b.编写ehcache配置文件 Ehcache.xml

```xml
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
  <!--当二级缓存的对象 超过内存限制时（缓存对象的个数>maxElementsInMemory），存放入的硬盘文件  -->
 <diskStore path="E:\Ehcache"/>
 <!-- 
 	maxElementsInMemory:设置 在内存中缓存 对象的个数
    maxElementsOnDisk：设置 在硬盘中缓存 对象的个数
    eternal：设置缓存是否 永远不过期
    overflowToDisk：当内存中缓存的对象个数 超过maxElementsInMemory的时候，是否转移到硬盘中
    timeToIdleSeconds：当2次访问 超过该值的时候，将缓存对象失效 
    timeToLiveSeconds：一个缓存对象 最多存放的时间（生命周期）
    diskExpiryThreadIntervalSeconds：设置每隔多长时间，通过一个线程来清理硬盘中的缓存
    memoryStoreEvictionPolicy：当超过缓存对象的最大值时，处理的策略；LRU，FIFO,LFU
  -->		     
 
 <defaultCache
  maxElementsInMemory="1000"
  maxElementsOnDisk="1000000"
  eternal="false"
  overflowToDisk="false"
  timeToIdleSeconds="100"
  timeToLiveSeconds="100"
  diskExpiryThreadIntervalSeconds="120"
  memoryStoreEvictionPolicy="LRU">
 </defaultCache>
</ehcache>
```



c.开启EhCache二级缓存

在xxxMapper.xml中开启

```xml
<cache  type="org.mybatis.caches.ehcache.EhcacheCache">
		<!-- 通过property覆盖Ehcache.xml中的值 -->
		<property name="maxElementsInMemory" value="2000"/>
		<property name="maxElementsOnDisk" value="3000"/>
	</cache>
```
# 逆向工程

表、类、接口、mapper.xml四者密切相关，因此 当知道一个的时候  其他三个应该可以自动生成。
**表->其他三个**

实现步骤：

## a. 导入依赖

 mybatis-generator-core.jar、mybatis.jar、ojdbc.jar

```xml
<!-- https://mvnrepository.com/artifact/org.mybatis.generator/mybatis-generator-core -->
		<dependency>
			<groupId>org.mybatis.generator</groupId>
			<artifactId>mybatis-generator-core</artifactId>
			<version>1.3.7</version>
		</dependency>
```



b.  逆向工程的配置文件generator.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<context id="DB2Tables" targetRuntime="MyBatis3">
		<commentGenerator>
			<!-- suppressAllComments属性值： true:自动生成实体类、SQL映射文件时没有注释 false:自动生成实体类、SQL映射文件，并附有注释 -->
			<property name="suppressAllComments" value="true" />
		</commentGenerator>


		<!-- 数据库连接信息 -->
		<jdbcConnection driverClass="oracle.jdbc.OracleDriver"
			connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:ORCL" userId="scott"
			password="tigger">
		</jdbcConnection>
	<!-- forceBigDecimals属性值： true:把数据表中的DECIMAL和NUMERIC类型， 解析为JAVA代码中的java.math.BigDecimal类型 
			false(默认):把数据表中的DECIMAL和NUMERIC类型， 解析为解析为JAVA代码中的Integer类型 -->
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>
		<!-- targetProject属性值:实体类的生成位置 targetPackage属性值：实体类所在包的路径 -->
		<javaModelGenerator
			targetPackage="org.lanqiao.entity" targetProject=".\src">
			<!-- trimStrings属性值： true：对数据库的查询结果进行trim操作 false(默认)：不进行trim操作 -->
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!-- targetProject属性值:SQL映射文件的生成位置 targetPackage属性值：SQL映射文件所在包的路径 -->
		<sqlMapGenerator targetPackage="org.lanqiao.mapper"
			targetProject=".\src">
		</sqlMapGenerator>
		<!-- 生成动态代理的接口 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="org.lanqiao.mapper" targetProject=".\src">
		</javaClientGenerator>

		<!-- 指定数据库表 -->
		<table tableName="Student">
		</table>
		<table tableName="studentCard">
		</table>
		<table tableName="studentClass">
		</table>
	</context>
</generatorConfiguration>
```

c.  执行

```java
File file = new File("src/generator.xml"); // 配置文件
		List<String> warnings = new ArrayList<String>();
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(file);
		DefaultShellCallback callback = new DefaultShellCallback(true);
		// 逆向工程核心类
		MyBatisGenerator generator = new MyBatisGenerator(config, callback, warnings);
		generator.generate(null);
```

```java
import java.io.File;
import java.util.ArrayList;
import java.util.List;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

public class MybatisGeneratorTest {
	public static void main(String[] args) throws Exception {
		   List<String> warnings = new ArrayList<String>();
		   boolean overwrite = true;
		   File configFile = new File("mybatis-generator.xml");
		   ConfigurationParser cp = new ConfigurationParser(warnings);
		   Configuration config = cp.parseConfiguration(configFile);
		   DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		   MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
		   myBatisGenerator.generate(null);
	}
}
```

# 常用方法

## 取到最后生成的主键

```xml
<!--取到最后生成的主键--><insert id="insertSelective" useGeneratedKeys="true" keyColumn="problem_id" keyProperty="problemId"        parameterType="com.yoj.web.bean.Problem">
```

## [用*Criteria*实现多条件查询]

(https://blog.csdn.net/qjlhlh/article/details/6599557)

逆向工程产生的bean（entity class）的扩展类中的静态内部类。

# mybatis注解版

## 插入时返回主键id

```java
@Mapper
public interface UserMapper
{
	@Insert("insert into tbl_user (name, age) values (#{name}, #{age})")
	@Options(useGeneratedKeys=true, keyProperty="userId", keyColumn="id")
	void insertUser(User user);
} 
```

## 调用其他方法

```java
@Select("SELECT * FROM solution ORDER BY solution_id DESC")
@Results({
    @Result(id = true, column = "solution_id", property = "solutionId"),
    @Result(column = "problem_id", property = "problem", one = @One(select = "com.yoj.web.dao.ProblemMapper.queryProblemTitleAndIdById", fetchType = FetchType.EAGER)),
    @Result(column = "language", property = "language"),
    @Result(column = "code", property = "code"),
    @Result(column = "result", property = "result"),
    @Result(column = "runtime", property = "runtime"),
    @Result(column = "memory", property = "memory"),
    @Result(column = "error_message", property = "errorMessage"),
    @Result(column = "submit_time", property = "submitTime"),
    @Result(column = "user_id", property = "user", one = @One(select = "com.yoj.web.dao.UserMapper.getUserById", fetchType = FetchType.EAGER))
})
List<Solution> getAllWithUserAndProblemName();
```

# other

### 自动映射

当自动映射查询结果时，MyBatis 会获取结果中返回的列名并在 Java 类中查找相同名字的属性（忽略大小写）。 这意味着如果发现了 *ID* 列和 *id* 属性，MyBatis 会将列 *ID* 的值赋给 *id* 属性。

通常数据库列使用大写字母组成的单词命名，单词间用下划线分隔；而 Java 属性一般遵循驼峰命名法约定。为了在这两种命名方式之间启用自动映射，需要将 `mapUnderscoreToCamelCase` 设置为 true。

## 批量操作

https://blog.csdn.net/mahoking/article/details/46811865

批量增加操作

```xml
<!-- 批量增加操作 -->
<insert id="batchInsertUsers" parameterType="java.util.List">
    insert into mhc_user(userName,password) values
    <foreach collection="list" item="item" index="index" separator=",">
        (#{item.userName},#{item.password})
    </foreach>
</insert>
```

## 插入后取到最后生成的主键

```xml
<!--取到最后生成的主键--><insert id="insertSelective" useGeneratedKeys="true" keyColumn="problem_id" keyProperty="problemId"        parameterType="com.yoj.web.bean.Problem">
```

## 