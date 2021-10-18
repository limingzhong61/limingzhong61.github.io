# Sentinel 规则持久化







## 一、修改order-service服务



修改OrderService，让其监听Nacos中的sentinel规则配置。

具体步骤如下：

### 1.引入依赖

在order-service中引入sentinel监听nacos的依赖：

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```



### 2.配置nacos地址

在order-service中的application.yml文件配置nacos地址及监听的配置信息：

```yaml
spring:
  cloud:
    sentinel:
      datasource:
        flow:
          nacos:
            server-addr: localhost:8848 # nacos地址
            dataId: orderservice-flow-rules
            groupId: SENTINEL_GROUP
            rule-type: flow # 还可以是：degrade、authority、param-flow
#        degrade:
#          nacos:
#            server-addr: localhost:8848 # nacos地址
#            dataId: orderservice-degrade-rules
#            groupId: SENTINEL_GROUP
#            rule-type: degrade # 还可以是：degrade、authority、param-flow
```





## 二、修改sentinel-dashboard源码

SentinelDashboard默认不支持nacos的持久化，需要修改源码。



### 1. 解压

解压课前资料中的sentinel源码包：

![image-20210618201340086](assets/image-20210618201340086.png)

然后并用IDEA打开这个项目，结构如下：

![image-20210618201412878](assets/image-20210618201412878.png)

### 2. 修改nacos依赖

在sentinel-dashboard源码的pom文件中，nacos的依赖默认的scope是test，只能在测试时使用，这里要去除：

![image-20210618201607831](assets/image-20210618201607831.png)

将sentinel-datasource-nacos依赖的scope去掉：

```xml
<dependency>
    <groupId>com.alibaba.csp</groupId>
    <artifactId>sentinel-datasource-nacos</artifactId>
</dependency>
```



### 3. 添加nacos支持

在sentinel-dashboard的test包下，已经编写了对nacos的支持，我们需要将其拷贝到main下。

![image-20210618201726280](assets/image-20210618201726280.png)



### 4. 修改nacos地址

然后，还需要修改测试代码中的NacosConfig类：

![image-20210618201912078](assets/image-20210618201912078.png)

修改其中的nacos地址，让其读取application.properties中的配置：

![image-20210618202047575](assets/image-20210618202047575.png)

在sentinel-dashboard的application.properties中添加nacos地址配置：

```properties
nacos.addr=localhost:8848
```



### 5. 配置nacos数据源

另外，还需要修改com.alibaba.csp.sentinel.dashboard.controller.v2包下的FlowControllerV2类：

![image-20210618202322301](assets/image-20210618202322301.png)

让我们添加的Nacos数据源生效：

![image-20210618202334536](assets/image-20210618202334536.png)



### 6. 修改前端页面

接下来，还要修改前端页面，添加一个支持nacos的菜单。

修改src/main/webapp/resources/app/scripts/directives/sidebar/目录下的sidebar.html文件：

![image-20210618202433356](assets/image-20210618202433356.png)



将其中的这部分注释打开：

![image-20210618202449881](assets/image-20210618202449881.png)



修改其中的文本：

![image-20210618202501928](assets/image-20210618202501928.png)



### 7. 重新编译、打包项目

运行IDEA中的maven插件，编译和打包修改好的Sentinel-Dashboard：

![image-20210618202701492](assets/image-20210618202701492.png)

**在资料中有配置好了的，sentinel-dashboard.jar**，直接使用亦可



### 8.启动

启动方式跟官方一样：

```sh
java -jar sentinel-dashboard.jar
```

如果要修改nacos地址，需要添加参数：

```sh
java -jar -Dnacos.addr=localhost:8848 sentinel-dashboard.jar
```

进入http://localhost:8080/发现有-NACOS就成功了

![image-20210914194510204](assets/image-20210914194510204.png)

nacos页面中也有配置管理

![image-20210914194616961](assets/image-20210914194616961.png)