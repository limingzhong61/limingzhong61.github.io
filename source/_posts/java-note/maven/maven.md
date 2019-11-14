---
title: maven
date: 2019-11-13 15:09:35
categories: java
tags: [java,maven]
---

[TOC]

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
	  <maven.compiler.target>1.8</maven.compiler.target> 	              <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
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

## maven的pom.xml结构

```xml
<!--maven的pom.xml结构-->
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <!--组织名称 一般是公司名称的倒写，像包名-->
  <groupId>com.sz</groupId>
  <!--项目，模块名称-->
  <artifactId>helloworld1</artifactId>
  <!--版本号-->
  <version>1.0-SNAPSHOT</version>
  <!--项目名-->
  <name>helloworld1</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <!--属性的定义-->
    <!--项目构建使用字符编码-->
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--编译源代码的版本-->
    <maven.compiler.source>1.8</maven.compiler.source>
    <!--目标代码的版本-->
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  <!--依赖集
      你所需要的jar包
  -->
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <!--要什么jar全部到maven中央仓库去找仓库中进行搜索即可-->
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <!--这是一个依赖-->
    <dependency>
      <!--通过坐标来描述-->
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.38</version>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>

```

# maven目录结构

**测试类一般是放在src/test/java**,而不是放在src/main/java下.maven在编译的时候,src/main/java下是不引用<scope>test</scope>的jar,而编译src/test/java下的测试这会引用<scope>test</scope>的jar

## maven中classpath路径

在Maven工程中，classpath的路径指java resources路径下的目录，即

src/main/java、 src/main/resource、src/test/java 、src/test/resource   四个目录
所以在配置路径时需要注意文件名是否重名，导致加载文件失败
