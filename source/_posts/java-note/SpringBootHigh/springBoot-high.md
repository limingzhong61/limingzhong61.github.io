---
title: springBoot高级
date: 2019-10-09 15:43:44
categories: java 
tags: [java,spring,springBoot]
---
**spring boot高级**

[TOC]

# 一.Spring Boot与缓存

## 1、JSR107

Java Caching定义了5个核心接口，分别是**CachingProvider**, **CacheManager**, **Cache**, **Entry** 和 **Expiry**。

•**CachingProvider**定义了创建、配置、获取、管理和控制多个**CacheManager**。一个应用可以在运行期访问多个CachingProvider。

•**CacheManager**定义了创建、配置、获取、管理和控制多个唯一命名的**Cache**，这些Cache存在于CacheManager的上下文中。一个CacheManager仅被一个CachingProvider所拥有。

•**Cache**是一个类似Map的数据结构并临时存储以Key为索引的值。一个Cache仅被一个CacheManager所拥有。

•**Entry**是一个存储在Cache中的key-value对。

•**Expiry** 每一个存储在Cache中的条目有一个定义的有效期。一旦超过这个时间，条目为过期的状态。一旦过期，条目将不可访问、更新和删除。缓存有效期可以通过ExpiryPolicy设置。





| **Cache**           | **缓存接口，定义缓存操作。实现有：RedisCache、EhCacheCache、ConcurrentMapCache等** |
| ------------------- | ------------------------------------------------------------ |
| **CacheManager**    | **缓存管理器，管理各种缓存（****Cache****）组件**            |
| **@Cacheable**      | **主要针对方法配置，能够根据方法的请求参数对其结果进行缓存** |
| **@****CacheEvict** | **清空缓存**                                                 |
| **@CachePut**       | **保证方法被调用，又希望结果被缓存。**                       |
| **@EnableCaching**  | **开启基于注解的缓存**                                       |
| **keyGenerator**    | **缓存数据时key生成策略**                                    |
| **serialize**       | **缓存数据时value序列化策略**                                |

- 一、搭建基本环境
- 1、导入数据库文件 创建出department和employee表
- 2、创建javaBean封装数据
- 3、整合MyBatis操作数据库
- 1.配置数据源信息
- 2.使用注解版的MyBatis；
- 1）、@MapperScan指定需要扫描的mapper接口所在的包

## 2、快速体验缓存

**==注意：cache注解（CachePut、Cacheable、@CachePut）的key保持一致，这样才能在cachemap中拿到同一个数据==**

### @CacheConfig注解

**抽取缓存的公共配置**

```java
@CacheConfig(cacheNames="emp",cacheManager = "employeeCacheManager") //抽取缓存的公共配置
@Service
public class EmployeeService {
```

### 步骤：

- ==**1、开启基于注解的缓存 @EnableCaching**==

- ```java
  @EnableCaching
  @MapperScan(value = "com.yoj.web.dao")
  @SpringBootApplication
  public class YojApplication {
      public static void main(String[] args) {
          SpringApplication.run(YojApplication.class, args);
      }
  
  }
  ```

- 

- 2、标注缓存注解即可

- @Cacheable

- @CacheEvict

- @CachePut

```java
/*
 * 默认使用的是ConcurrentMapCacheManager==ConcurrentMapCache；将数据保存在	ConcurrentMap<Object, Object>中
 * 开发中使用缓存中间件；redis、memcached、ehcache；
 * 
 *
 */
```



- service层使用cache注解

- 缓存中能使用的spel表达式

- **Cache*** ***SpEL*** ***available metadata***

  

| **名字**        | **位置**           | **描述**                                                     | **示例**             |
| --------------- | ------------------ | ------------------------------------------------------------ | -------------------- |
| methodName      | root object        | 当前被调用的方法名                                           | #root.methodName     |
| method          | root object        | 当前被调用的方法                                             | #root.method.name    |
| target          | root object        | 当前被调用的目标对象                                         | #root.target         |
| targetClass     | root object        | 当前被调用的目标对象类                                       | #root.targetClass    |
| args            | root object        | 当前被调用的方法的参数列表                                   | #root.args[0]        |
| caches          | root object        | 当前方法调用使用的缓存列表（如@Cacheable(value={"cache1",   "cache2"})），则有两个cache | #root.caches[0].name |
| *argument name* | evaluation context | 方法参数的名字. 可以直接 #参数名 ，也可以使用 #p0或#a0 的形式，0代表参数的索引； | #iban 、 #a0 、  #p0 |
| result          | evaluation context | 方法执行后的返回值（仅当方法执行之后的判断有效，如‘unless’，’cache put’的表达式 ’cache evict’的表达式beforeInvocation=false） | #result              |

### @Cacheable注解

#### 原理：

- 1、自动配置类；CacheAutoConfiguration

- 2、缓存的配置类(11)

  ```java
  org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration
  org.springframework.boot.autoconfigure.cache.JCacheCacheConfiguration
  org.springframework.boot.autoconfigure.cache.EhCacheCacheConfiguration
  org.springframework.boot.autoconfigure.cache.HazelcastCacheConfiguration
  org.springframework.boot.autoconfigure.cache.InfinispanCacheConfiguration
  org.springframework.boot.autoconfigure.cache.CouchbaseCacheConfiguration
  org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration
  org.springframework.boot.autoconfigure.cache.CaffeineCacheConfiguration
  org.springframework.boot.autoconfigure.cache.GuavaCacheConfiguration
  org.springframework.boot.autoconfigure.cache.SimpleCacheConfiguration【默认】
  org.springframework.boot.autoconfigure.cache.NoOpCacheConfiguration
  ```

  - 3、哪个配置类默认生效：SimpleCacheConfiguration；

- 4、给容器中注册了一个CacheManager：ConcurrentMapCacheManager

- 

  - 5、可以获取和创建ConcurrentMapCache类型的缓存组件；他的作用将数据保存在ConcurrentMap中；

#### 运行流程：(ConcurrentMapCacheManager.class)

   @Cacheable：
   1、方法运行之前，先去**查询Cache（缓存组件）**，按照cacheNames指定的名字获取；
   （CacheManager先获取相应的缓存），第一次获取缓存如果没有Cache组件会自动创建。

```java
@Override
@Nullable
public Cache getCache(String name) {
    Cache cache = this.cacheMap.get(name);
    if (cache == null && this.dynamic) {
        synchronized (this.cacheMap) {
            cache = this.cacheMap.get(name);
            if (cache == null) {
                cache = createConcurrentMapCache(name);
                this.cacheMap.put(name, cache);
            }
        }
    }
    return cache;
}
```



   2、去Cache中查找缓存的内容，使用一个key，默认就是方法的参数；

```java
protected Object lookup(Object key) {
    return this.store.get(key);
}
```

   key是按照某种策略生成的；默认是使用keyGenerator生成的，默认使用SimpleKeyGenerator生成key；

```java
public abstract class CacheAspectSupport extends AbstractCacheInvoker
protected Object generateKey(@Nullable Object result) {
    if (StringUtils.hasText(this.metadata.operation.getKey())) {
        EvaluationContext evaluationContext = createEvaluationContext(result);
        return evaluator.key(this.metadata.operation.getKey(), this.metadata.methodKey, evaluationContext);
    }
    return this.metadata.keyGenerator.generate(this.target, this.metadata.method, this.args);
}
```

   SimpleKeyGenerator生成key的默认策略；
   如果没有参数；key=new SimpleKey()；
   如果有一个参数：key=参数的值
   如果有多个参数：key=new SimpleKey(params)；

```java
public class SimpleKeyGenerator implements KeyGenerator {  
/**
* Generate a key based on the specified parameters.
*/
public static Object generateKey(Object... params) {
    if (params.length == 0) {
        return SimpleKey.EMPTY;
    }
    if (params.length == 1) {
        Object param = params[0];
        if (param != null && !param.getClass().isArray()) {
            return param;
        }
    }
    return new SimpleKey(params);
}
```



   3、没有查到缓存就调用目标方法；
	 4、将目标方法返回的结果，放进缓存中

```java
public void put(Object key, @Nullable Object value) {
    this.store.put(key, toStoreValue(value));
}

```



   @Cacheable标注的方法执行之前先来检查缓存中有没有这个数据，默认按照参数的值作为key去查询缓存，
	 如果没有就运行方法并将结果放入缓存；以后再来调用就可以直接使用缓存中的数据；
   ==**核心：**==
   1）、使用CacheManager【ConcurrentMapCacheManager】按照名字得到Cache【ConcurrentMapCache】组件
	 2）、key使用keyGenerator生成的，默认是SimpleKeyGenerator

### 几个属性：

- cacheNames/value：指定缓存组件的名字;将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存；

- key：缓存数据使用的key；可以用它来指定。默认是使用方法参数的值  1-方法的返回值
  	   编写SpEL； #i d;参数id的值   #a0  #p0  #root.args[0]
   		   getEmp[2] ： **key = "#root.methodName+'['+#id+']'"**

  ```java
   @Cacheable(cacheNames = {"emp"},key = "#root.methodName+'['+#id+']'")
  ```

  

- keyGenerator：key的生成器；可以自己指定key的生成器的组件id
  	   key/keyGenerator：二选一使用;

  ```java
  @Configuration
  public class MyCacheConfig {
  
      @Bean("myKeyGenerator")
      public KeyGenerator keyGenerator(){
          return new KeyGenerator(){
              @Override
              public Object generate(Object target, Method method, Object... params) 				{
                  return method.getName()+"["+ Arrays.asList(params).toString()+"]";
              }
          };
      }
  }
  //调用
   @Cacheable(cacheNames = {"emp"},keyGenerator = "myKeyGenerator")
  ```

  

- cacheManager：指定缓存管理器；或者cacheResolver指定获取解析器

- condition：指定符合条件的情况下才缓存；
  	   ,condition = "#id>0"
   	   condition = "#a0>1"：第一个参数的值 >1的时候才进行缓存

- unless:否定缓存；当unless指定的条件为true，方法的返回值就不会被缓存；可以获取到结果进行判断
  	   unless = "#result == null"
  	   unless = "#a0==2":如果第一个参数的值是2，结果不缓存；

- sync：是否使用异步模式,启用sync就不能使用unless属性了

```java
@CacheConfig(cacheNames="emp"/*,cacheManager = "employeeCacheManager"*/) //抽取缓存的公共配置
@Service
public class EmployeeService {

    @Autowired
    EmployeeMapper employeeMapper;

    /**
     * 将方法的运行结果进行缓存；以后再要相同的数据，直接从缓存中获取，不用调用方法；
     * CacheManager管理多个Cache组件的，对缓存的真正CRUD操作在Cache组件中，每一个缓存组件有自己唯一一个名字；

     *   
     * @param id
     * @return
     *
     */
    @Cacheable(value = {"emp"}/*,keyGenerator = "myKeyGenerator",condition = "#a0>1",unless = "#a0==2"*/)
    public Employee getEmp(Integer id){
        System.out.println("查询"+id+"号员工");
        Employee emp = employeeMapper.getEmpById(id);
        return emp;
    }

    // @Caching 定义复杂的缓存规则
    @Caching(
         cacheable = {
             @Cacheable(/*value="emp",*/key = "#lastName")
         },
         put = {
             @CachePut(/*value="emp",*/key = "#result.id"),
             @CachePut(/*value="emp",*/key = "#result.email")
         }
    )
    public Employee getEmpByLastName(String lastName){
        return employeeMapper.getEmpByLastName(lastName);
    }

}


```

### @CachePut注解

@CachePut：既调用方法，又更新缓存数据；同步更新缓存
  修改了数据库的某个数据，同时更新缓存；
  **运行时机：**
1、先调用目标方法
2、将目标方法的结果缓存起来



```java
    /**
     * 测试步骤：
     *  1、查询1号员工；查到的结果会放在缓存中；
     *          key：1  value：lastName：张三
     *  2、以后查询还是之前的结果
     *  3、更新1号员工；【lastName:zhangsan；gender:0】
     *          将方法的返回值也放进缓存了；
     *          key：传入的employee对象  值：返回的employee对象；
     *  4、查询1号员工？
     *      应该是更新后的员工；
     *          key = "#employee.id":使用传入的参数的员工id；
     *          key = "#result.id"：使用返回后的id
     *             @Cacheable的key是不能用#result
     *      为什么是没更新前的？【1号员工没有在缓存中更新】
     *
     */
    @CachePut(/*value = "emp",*/key = "#result.id")
    public Employee updateEmp(Employee employee){
        System.out.println("updateEmp:"+employee);
        employeeMapper.updateEmp(employee);
        return employee;
    }

```

### @CacheEvict注解

**evict：驱逐，逐出**

@CacheEvict：缓存清除

- key：指定要清除的数据
- -allEntries = true：指定清除这个缓存中所有的数据
- beforeInvocation = false：缓存的清除是否在方法之前执行
  默认代表缓存清除操作是在方法执行之后执行;如果出现异常缓存就不会清除
- beforeInvocation = true：
  代表清除缓存操作是在方法运行之前执行，无论方法是否出现异常，缓存都清除



```java
    @CacheEvict(value="emp",beforeInvocation = true/*key = "#id",*/)
    public void deleteEmp(Integer id){
        System.out.println("deleteEmp:"+id);
        //employeeMapper.deleteEmpById(id);
        int i = 10/0;
    }

```

### @Caching注解

定义复杂的缓存规则

```java
   // @Caching 定义复杂的缓存规则
    @Caching(
         cacheable = {
             @Cacheable(/*value="emp",*/key = "#lastName")
         },
         put = {
             @CachePut(/*value="emp",*/key = "#result.id"),
             @CachePut(/*value="emp",*/key = "#result.email")
         }
    )
    public Employee getEmpByLastName(String lastName){
        return employeeMapper.getEmpByLastName(lastName);
    }

```

```java
package com.atguigu.springboot01cache.service;

import com.atguigu.springboot01cache.bean.Employee;
import com.atguigu.springboot01cache.mapper.EmployeeMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.cache.annotation.*;
import org.springframework.stereotype.Service;

@CacheConfig(cacheNames = "emp"/*,cacheManager = "employeeCacheManager"*/) //抽取缓存的公共配置
@Service
public class EmployeeService {

    @Autowired
    EmployeeMapper employeeMapper;

    /**
     * 将方法的运行结果进行缓存；以后再要相同的数据，直接从缓存中获取，不用调用方法；
     * CacheManager管理多个Cache组件的，对缓存的真正CRUD操作在Cache组件中，每一个缓存组件有自己唯一一个名字；
     * <p>
     * <p>
     * <p>
     * 原理：
     * 1、自动配置类；CacheAutoConfiguration
     * 2、缓存的配置类
     * org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.JCacheCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.EhCacheCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.HazelcastCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.InfinispanCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.CouchbaseCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.CaffeineCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.GuavaCacheConfiguration
     * org.springframework.boot.autoconfigure.cache.SimpleCacheConfiguration【默认】
     * org.springframework.boot.autoconfigure.cache.NoOpCacheConfiguration
     * 3、哪个配置类默认生效：SimpleCacheConfiguration；
     * <p>
     * 4、给容器中注册了一个CacheManager：ConcurrentMapCacheManager
     * 5、可以获取和创建ConcurrentMapCache类型的缓存组件；他的作用将数据保存在ConcurrentMap中；
     * <p>
     * 运行流程：
     *
     * @param id
     * @return
     * @Cacheable： 1、方法运行之前，先去查询Cache（缓存组件），按照cacheNames指定的名字获取；
     * （CacheManager先获取相应的缓存），第一次获取缓存如果没有Cache组件会自动创建。
     * 2、去Cache中查找缓存的内容，使用一个key，默认就是方法的参数；
     * key是按照某种策略生成的；默认是使用keyGenerator生成的，默认使用SimpleKeyGenerator生成key；
     * SimpleKeyGenerator生成key的默认策略；
     * 如果没有参数；key=new SimpleKey()；
     * 如果有一个参数：key=参数的值
     * 如果有多个参数：key=new SimpleKey(params)；
     * 3、没有查到缓存就调用目标方法；
     * 4、将目标方法返回的结果，放进缓存中
     * @Cacheable标注的方法执行之前先来检查缓存中有没有这个数据，默认按照参数的值作为key去查询缓存， 如果没有就运行方法并将结果放入缓存；以后再来调用就可以直接使用缓存中的数据；
     * <p>
     * 核心：
     * 1）、使用CacheManager【ConcurrentMapCacheManager】按照名字得到Cache【ConcurrentMapCache】组件
     * 2）、key使用keyGenerator生成的，默认是SimpleKeyGenerator
     * <p>
     * <p>
     * 几个属性：
     * cacheNames/value：指定缓存组件的名字;将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存；
     * <p>
     * key：缓存数据使用的key；可以用它来指定。默认是使用方法参数的值  1-方法的返回值
     * 编写SpEL； #i d;参数id的值   #a0  #p0  #root.args[0]
     * getEmp[2]
     * <p>
     * keyGenerator：key的生成器；可以自己指定key的生成器的组件id
     * key/keyGenerator：二选一使用;
     * <p>
     * <p>
     * cacheManager：指定缓存管理器；或者cacheResolver指定获取解析器
     * <p>
     * condition：指定符合条件的情况下才缓存；
     * ,condition = "#id>0"
     * condition = "#a0>1"：第一个参数的值》1的时候才进行缓存
     * <p>
     * unless:否定缓存；当unless指定的条件为true，方法的返回值就不会被缓存；可以获取到结果进行判断
     * unless = "#result == null"
     * unless = "#a0==2":如果第一个参数的值是2，结果不缓存；
     * sync：是否使用异步模式
     */
    @Cacheable(value = {"emp"}/*,keyGenerator = "myKeyGenerator",condition = "#a0>1",unless = "#a0==2"*/)
    public Employee getEmp(Integer id) {
        System.out.println("查询" + id + "号员工");
        Employee emp = employeeMapper.getEmpById(id);
        return emp;
    }

    /**
     * @CachePut：既调用方法，又更新缓存数据；同步更新缓存 修改了数据库的某个数据，同时更新缓存；
     * 运行时机：
     * 1、先调用目标方法
     * 2、将目标方法的结果缓存起来
     * <p>
     * 测试步骤：
     * 1、查询1号员工；查到的结果会放在缓存中；
     * key：1  value：lastName：张三
     * 2、以后查询还是之前的结果
     * 3、更新1号员工；【lastName:zhangsan；gender:0】
     * 将方法的返回值也放进缓存了；
     * key：传入的employee对象  值：返回的employee对象；
     * 4、查询1号员工？
     * 应该是更新后的员工；
     * key = "#employee.id":使用传入的参数的员工id；
     * key = "#result.id"：使用返回后的id
     * @Cacheable的key是不能用#result 为什么是没更新前的？【1号员工没有在缓存中更新】
     */
    @CachePut(value = "emp", key = "#result.id")
    public Employee updateEmp(Employee employee) {
        System.out.println("updateEmp:" + employee);
        employeeMapper.updateEmp(employee);
        return employee;
    }

    /**
     * @CacheEvict：缓存清除 key：指定要清除的数据
     * allEntries = true：指定清除这个缓存中所有的数据
     * beforeInvocation = false：缓存的清除是否在方法之前执行
     * 默认代表缓存清除操作是在方法执行之后执行;如果出现异常缓存就不会清除
     * <p>
     * beforeInvocation = true：
     * 代表清除缓存操作是在方法运行之前执行，无论方法是否出现异常，缓存都清除
     */
    @CacheEvict(value = "emp", beforeInvocation = true/*key = "#id",*/)
    public void deleteEmp(Integer id) {
        System.out.println("deleteEmp:" + id);
        //employeeMapper.deleteEmpById(id);
        int i = 10 / 0;
    }

    // @Caching 定义复杂的缓存规则
    @Caching(
            cacheable = {
                    @Cacheable(/*value="emp",*/key = "#lastName")
            },
            put = {
                    @CachePut(/*value="emp",*/key = "#result.id"),
                    @CachePut(/*value="emp",*/key = "#result.email")
            }
    )
    public Employee getEmpByLastName(String lastName) {
        return employeeMapper.getEmpByLastName(lastName);
    }


}

```



## 3.整合redis作为缓存

 Redis 是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。



 原理：CacheManager===Cache 缓存组件来实际给缓存中存取数据
 1）、引入redis的starter，容器中保存的是 RedisCacheManager；
 2）、RedisCacheManager 帮我们创建 RedisCache 来作为缓存组件；RedisCache通过操作redis缓存数据的
 3）、默认保存数据 kv 都是Object；利用序列化保存；如何保存为json
 1、引入了redis的starter，cacheManager变为 RedisCacheManager；
 2、默认创建的 RedisCacheManager 操作redis的时候使用的是 RedisTemplate<Object, Object>
 3、RedisTemplate<Object, Object> 是 默认使用jdk的序列化机制
 4）、自定义CacheManager；

### 1、安装redis：使用docker；

**启动redis，默认端口6379**

```shell
[root@MiWiFi-R3A-srv ~]# docker run -d -p 6379:6379 --name myredis redis
ba86c7f5d285b74828df3ec4f0179cfcd3682dc58f2cfabe354a63336d94919e
#开启持久化
docker run  -d -p 6379:6379 --name persistent-redis redis --appendonly yes

docker run --name="redis-2" -d -p 6378:6379 -v /home/fr/redis:/opt royfans/redis:v1 /usr/local/redis/bin/redis-server /usr/local/redis/redis.conf --appendonly yes

```

==**注意**==：**如果不开启持久化，会导致一段时间不用缓存之后，连接不上redis**

**start with persistent storage**

```shell
docker run -v /myredis/conf/redis.conf:/home/ubuntu/redis/redis.conf -d -p 6379:6379 --name config-redis redis --appendonly yes

$ docker run --name some-redis -d redis redis-server --appendonly yes

```



```shell
$ docker run -v /myredis/conf/redis.conf:/usr/local/etc/redis/redis.conf --name myredis redis redis-server /usr/local/etc/redis/redis.conf

```

Where /myredis/conf/ is a local directory containing your redis.conf file. Using this method means that there is no need for you to have a Dockerfile for your redis container.



```
这个问题我们在项目中遇到同样的问题，目前已经解决了。最终得到的答案是： 服务器不稳定造成的。
您可以尝试这样解决：
1.推荐使用生产环境的服务器，并且将redis 绑定生产环境的ip;因为云服务器的ip 地址是很稳定的，而本地服务的ip地址经常是变动的；经 
   过测试，这种每过10分就会重新请求连接，还会发生重试失败的情况，就是因为服务器不稳定造成的；
2.如果你在生产环境中，使用docker 部署，建议 不要在docker容器中 安装redis; 因为docker 容器 默认分配的ip 地址，也可能是变化的；
  您可以直接将redis 安装在 服务器目录下，即可；

```



redis desktop manager连接

![1565350310934](redis desktop manager连接.png)

### 2、引入redis的starter

1.引入spring-boot-starter-data-redis

```xml
<!--引入redis-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

```

### 3、配置redis

```properties
#指定主机端口号
spring.redis.host=192.168.31.39

```

### 4、测试缓存

3.使用RestTemplate操作redis

1.redisTemplate.opsForValue();//操作字符串

2.redisTemplate.opsForHash();//操作hash

3.redisTemplate.opsForList();//操作list

4.redisTemplate.opsForSet();//操作set

5.redisTemplate.opsForZSet();//操作有序set

4.配置缓存、CacheManagerCustomizers

5.测试使用缓存、切换缓存、 CompositeCacheManager

**==stringRedisTemplate==**

//操作k-v都是字符串的

```java
@Autowired
StringRedisTemplate stringRedisTemplate;  //操作k-v都是字符串的
/**
     * Redis常见的五大数据类型
     *  String（字符串）、List（列表）、Set（集合）、Hash（散列）、ZSet（有序集合）
     *  stringRedisTemplate.opsForValue()[String（字符串）]
     *  stringRedisTemplate.opsForList()[List（列表）]
     *  stringRedisTemplate.opsForSet()[Set（集合）]
     *  stringRedisTemplate.opsForHash()[Hash（散列）]
     *  stringRedisTemplate.opsForZSet()[ZSet（有序集合）]
     */
@Test
public void test01(){
    //给redis中保存数据
    //stringRedisTemplate.opsForValue().append("msg","hello");
    //获取数据
    //		String msg = stringRedisTemplate.opsForValue().get("msg");
    //		System.out.println(msg);

    //保存list集合数据
    stringRedisTemplate.opsForList().leftPush("mylist","1");
    stringRedisTemplate.opsForList().leftPush("mylist","2");
}

```

**==redisTemplate==**

k-v都是对象

```java
 @Autowired
    RedisTemplate redisTemplate; //k-v都是对象的
    @Autowired
    RedisTemplate<Object,Object> empRedisTemplate; //自定义缓存规则配置的redisTemplate

//测试保存对象
    @Test
    public void test02(){
        Employee empById = employeeMapper.getEmpById(1);
        //默认如果保存对象，使用jdk序列化机制，序列化后的数据保存到redis中
        //redisTemplate.opsForValue().set("emp-01",empById);
        //1、将数据以json的方式保存
        //(1)自己将对象转为json
        //(2)redisTemplate默认的序列化规则；改变默认的序列化规则；
        empRedisTemplate.opsForValue().set("emp-01",empById);
    }
```

### 5.使用Json格式序列化对象

1.使用setKey和value的Serializer方法

```java
redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<Employee>(Employee.class));
        redisTemplate.setKeySerializer(new Jackson2JsonRedisSerializer<Employee>(Employee.class));
        redisTemplate.opsForValue().set("emp-02",empById);
```



```java
@Configuration
public class MyRedisConfig {

    @Bean
    public RedisTemplate<Object, Object> empRedisTemplate(RedisConnectionFactory redisConnectionFactory)
            throws UnknownHostException {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(redisConnectionFactory);
        Jackson2JsonRedisSerializer<Employee> serializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
        template.setDefaultSerializer(serializer);
        return template;
    }

```



## 2.0配置redis的CacheManager

```yaml
spring:
  cache:
    redis:
      timeToLive: 1000000 #毫秒

```



```java
package com.atguigu.springboot01cache.config;

import com.fasterxml.jackson.annotation.JsonAutoDetect;
import com.fasterxml.jackson.annotation.PropertyAccessor;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.cache.CacheManager;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.serializer.Jackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.RedisSerializationContext;
import org.springframework.data.redis.serializer.RedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.time.Duration;

@Configuration
public class MyRedisConfig {

//    @Bean
//    public RedisTemplate<Object, Object> empRedisTemplate(RedisConnectionFactory redisConnectionFactory)
//            throws UnknownHostException {
//        RedisTemplate<Object, Object> template = new RedisTemplate<>();
//        template.setConnectionFactory(redisConnectionFactory);
//        Jackson2JsonRedisSerializer<Employee> serializer = new Jackson2JsonRedisSerializer<Employee>(Employee.class);
//        template.setDefaultSerializer(serializer);
//        return template;
//    }

    private Duration timeToLive = Duration.ZERO;

    public void setTimeToLive(Duration timeToLive) {
        this.timeToLive = timeToLive;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);

        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);

        // 配置序列化（解决乱码的问题）
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(timeToLive)
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();

        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}



```

## 4.注意

## 1.@CachePut 获取就是返回的值

所有想要存入缓存的都是返回的值

## my_redis

### 设置缓存时间

```java
stringRedisTemplate.opsForValue().set(email, checkCode,60*10,TimeUnit.SECONDS);//向redis里存入数据和设置缓存时间


```

### 删除缓存byKey

```java
stringRedisTemplate.delete(user.getEmail());

```

### 检查时间

```java
stringRedisTemplate.hasKey("546545");//检查key是否存在，返回boolean值

```

## 问题

### redis一段时间之后不连接就连不上

**==内存原因，设置maxmemory和替换算法==**



  在Linux上，如果开了redis的守护进程，kill -9和redis-cli shutdown 命令是无法杀掉 redis 进程的 ，杀掉就会重新启动一个新的进程

最后在网上找到这个命令：

```
/etc/init.d/redis-server stop  

```

# 二.Spring Boot与消息

JMS、AMQP、RabbitMQ

## 一、概述

1.大多应用中，可通过消息服务中间件来提升系统异步通信、扩展解耦能力

2.消息服务中两个重要概念：

​       **消息代理（message broker）和目的地（destination）**

当消息发送者发送消息以后，将由消息代理接管，消息代理保证消息传递到指定目的地。

3.消息队列主要有两种形式的目的地

 1.队列（queue）：点对点消息通信（point-to-point）

 2.主题（topic）：发布（publish）/订阅（subscribe）消息通信

4.点对点式：

–消息发送者发送消息，消息代理将其放入一个队列中，消息接收者从队列中获取消息内容，消息读取后被移出队列

–消息只有唯一的发送者和接受者，但并不是说只能有一个接收者

5.发布订阅式：

–发送者（发布者）发送消息到主题，多个接收者（订阅者）监听（订阅）这个主题，那么就会在消息到达时同时收到消息

6.JMS（Java Message Service）JAVA消息服务：

–基于JVM消息代理的规范。ActiveMQ、HornetMQ是JMS实现

7.AMQP（Advanced Message Queuing Protocol）

–高级消息队列协议，也是一个消息代理的规范，兼容JMS

–RabbitMQ是AMQP的实现

|              | JMS                                                          | AMQP                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 定义         | Java   api                                                   | 网络线级协议                                                 |
| 跨语言       | 否                                                           | 是                                                           |
| 跨平台       | 否                                                           | 是                                                           |
| Model        | 提供两种消息模型：   （1）、Peer-2-Peer   （2）、Pub/sub     | 提供了五种消息模型：   （1）、direct   exchange   （2）、fanout   exchange   （3）、topic   change   （4）、headers   exchange   （5）、system   exchange   本质来讲，后四种和JMS的pub/sub模型没有太大差别，仅是在路由机制上做了更详细的划分； |
| 支持消息类型 | 多种消息类型：   TextMessage   MapMessage   BytesMessage   StreamMessage   ObjectMessage   Message   （只有消息头和属性） | byte[]   当实际应用时，有复杂的消息，可以将消息序列化后发送。 |
| 综合评价     | JMS   定义了JAVA   API层面的标准；在java体系中，多个client均可以通过JMS进行交互，不需要应用修改代码，但是其对跨平台的支持较差； | AMQP定义了wire-level层的协议标准；天然具有跨平台、跨语言特性。 |

8.Spring支持

**–spring-jms提供了对JMS的支持**

**–spring-rabbit提供了对AMQP的支持**

**–需要ConnectionFactory的实现来连接消息代理**

**–提供JmsTemplate、RabbitTemplate来发送消息**

**–@JmsListener（JMS）、@RabbitListener（AMQP）注解在方法上监听消息代理发布的消息**

**–@EnableJms、@EnableRabbit开启支持**

9.Spring Boot自动配置

–**JmsAutoConfiguration**

–**RabbitAutoConfiguration**

## 二、RabbitMQ简介

**RabbitMQ简介：**

RabbitMQ是一个由erlang开发的AMQP(Advanved Message Queue Protocol)的开源实现。

**核心概念**

**Message**

消息，消息是不具名的，它由消息头和消息体组成。消息体是不透明的，而消息头则由一系列的可选属性组成，这些属性包括routing-key（路由键）、priority（相对于其他消息的优先权）、delivery-mode（指出该消息可能需要持久性存储）等。

**Publisher**

消息的生产者，也是一个向交换器发布消息的客户端应用程序。

**Exchange**

交换器，用来接收生产者发送的消息并将这些消息路由给服务器中的队列。

Exchange有4种类型：direct(默认)，fanout, topic, 和headers，不同类型的Exchange转发消息的策略有所区别





三、Spring Boot与检索

# 四.Spring Boot与任务

## 异步任务、定时任务、邮件任务

## 一、异步任务

在Java应用中，绝大多数情况下都是通过同步的方式来实现交互处理的；但是在处理与第三方系统交互的时候，容易造成响应迟缓的情况，之前大部分都是使用多线程来完成此类任务，其实，在Spring 3.x之后，就已经内置了@Async来完美解决这个问题。



**两个注解：**

@EnableAysnc、@Aysnc

**springbootApplication添加@EnableAysnc注解**

```java
@EnableAsync  //开启异步注解功能
@SpringBootApplication
public class Springboot04TaskApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot04TaskApplication.class, args);
    }

}

```

**编写异步方法，同时执行**，并不会等3s才有success

```java
@Service
public class AsyncService {

    //告诉Spring这是一个异步方法
    @Async
    public void hello(){
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("处理数据中...");
    }
}

```

## 二、定时任务 

@EnableScheduling //开启基于注解的定时任务

```java
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class Springboot04TaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot04TaskApplication.class, args);
    }

}

```

**编写定时方法**

```java
@Service
public class ScheduledService {

    /**
     * second(秒), minute（分）, hour（时）, day of month（日）, month（月）, day of week（周几）.
     * 0 * * * * MON-FRI
     *  【0 0/5 14,18 * * ?】 每天14点整，和18点整，每隔5分钟执行一次
     *  【0 15 10 ? * 1-6】 每个月的周一至周六10:15分执行一次
     *  【0 0 2 ? * 6L】每个月的最后一个周六凌晨2点执行一次
     *  【0 0 2 LW * ?】每个月的最后一个工作日凌晨2点执行一次
     *  【0 0 2-4 ? * 1#1】每个月的第一个周一凌晨2点到4点期间，每个整点都执行一次；
     */
//    @Scheduled(cron = "0 * * * * MON-SAT")
//    @Scheduled(cron = "0,1,2,3,4 * * * * MON-SAT")
   // @Scheduled(cron = "0-4 * * * * MON-SAT")
    @Scheduled(cron = "0/4 * * * * MON-SAT")  //每4秒执行一次
    public void hello(){
        System.out.println("hello ... ");
    }
}
```

**两个注解：@EnableScheduling、@Scheduled**

**cron表达式：**

| **字段** | **允许值**              | **允许的特殊字符** |
| -------- | ----------------------- | ------------------ |
| 秒       | 0-59                    | , -   * /          |
| 分       | 0-59                    | , -   * /          |
| 小时     | 0-23                    | , -   * /          |
| 日期     | 1-31                    | , -   * ? / L W C  |
| 月份     | 1-12                    | , -   * /          |
| 星期     | 0-7或SUN-SAT   0,7是SUN | , -   * ? / L C #  |

| **特殊字符** | **代表含义**               |
| ------------ | -------------------------- |
| ,            | 枚举                       |
| -            | 区间                       |
| *            | 任意                       |
| /            | 步长                       |
| ?            | 日/星期冲突匹配            |
| L            | 最后                       |
| W            | 工作日                     |
| C            | 和calendar联系后计算过的值 |
| #            | 星期，4#2，第2个星期四     |

## 三、邮件任务 

•邮件发送需要引入spring-boot-starter-mail

•Spring Boot 自动配置MailSenderAutoConfiguration

•定义MailProperties内容，配置在application.yml中

•自动装配JavaMailSender

•测试邮件发送

**1.导入依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>

```

**2.配置信息**

1)首先在qq邮箱开通相关的服务

![1569337676687](E:\Desktop\springboot高级\imgs\1569337676687.png)

![1569337715201](E:\Desktop\springboot高级\imgs\1569337715201.png)

**拿到的授权码即为password**

```properties
spring.mail.username=1162314270@qq.com
spring.mail.password=kykperaulxvigjbh
spring.mail.host=smtp.qq.com
#503错误，我的没有这个错
#spring.mail.properties.mail.smtp.ssl.enable=true
#ggjhtotqoeahihgb
#cnfyhrjcbacvgjaf
#kykperaulxvigjbh

```





```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class Springboot04TaskApplicationTests {

    @Autowired
    JavaMailSenderImpl mailSender;

    @Test
    public void contextLoads() {
        SimpleMailMessage message = new SimpleMailMessage();
        //邮件设置
        message.setSubject("通知-今晚开会");
        message.setText("今晚7:30开会");

        message.setTo("407820388@qq.com");
        message.setFrom("1162314270@qq.com");

        mailSender.send(message);
    }


    @Test
    public void test02() throws  Exception{
        //1、创建一个复杂的消息邮件
        MimeMessage mimeMessage = mailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);

        //邮件设置
        helper.setSubject("通知-今晚开会");
        helper.setText("<b style='color:red'>今天 7:30 开会</b>",true);

        helper.setTo("407820388@qq.com");
        helper.setFrom("1162314270@qq.com");

        //上传文件
        helper.addAttachment("1.jpg",new File("E:\\pictures\\desktop view.png"));
        helper.addAttachment("2.jpg",new File("E:\\pictures\\e.png"));

        mailSender.send(mimeMessage);

    }
}

```





# 五.Spring Boot与安全

## 一、安全

Spring Security是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型。他可以实现强大的web安全控制。对于安全控制，我们仅需引入spring-boot-starter-security模块，进行少量的配置，即可实现强大的安全管理。

 几个类：

WebSecurityConfigurerAdapter：自定义Security策略

AuthenticationManagerBuilder：自定义认证策略

@EnableWebSecurity：开启WebSecurity模式

### “认证”和“授权”

•应用程序的两个主要区域是“认证”和“授权”（或者访问控制）。这两个主要区域是Spring Security 的两个目标。

•**“认证”（Authentication）**，是建立一个他声明的主体的过程（一个“主体”一般是指用户，设备或一些可以在你的应用程序中执行动作的其他系统）。

•“**授权”（Authorization）**指确定一个主体是否允许在你的应用程序执行一个动作的过程。为了抵达需要授权的店，主体的身份已经有认证过程建立。

•这个概念是通用的而不只在Spring Security中。

## 二、Web&安全 

1.登陆/注销

–HttpSecurity配置登陆、注销功能

2.Thymeleaf提供的SpringSecurity标签支持

–需要引入thymeleaf-extras-springsecurity5（版本要一致）

–sec:authentication=“name”获得当前用户的用户名

–sec:authorize=“hasRole(‘ADMIN’)”当前用户必须拥有ADMIN权限时才会显示标签内容

3.remember me

–表单添加remember-me的checkbox

–配置启用remember-me功能

4.CSRF（Cross-site request forgery）跨站请求伪造

HttpSecurity启用功能，会为表单添加csrfCSRF

## 使用SpringSecurity

[官方文档](https://spring.io/projects/spring-security)

### 1、引入SpringSecurity；

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

```

### 2、编写SpringSecurity的配置类；

**使用之间需要PasswordEncoder的bean存在**

使用springboot，权限管理使用spring security，使用内存用户验证，但无响应报错：
java.lang.IllegalArgumentException: There is no PasswordEncoder mapped for the id "null"
解决方法：
这是因为Spring boot 2.0.3引用的security 依赖是 spring security 5.X版本，此版本需要提供一个PasswordEncorder的实例，否则后台汇报错误：
java.lang.IllegalArgumentException: There is no PasswordEncoder mapped for the id "null"
并且页面毫无响应。
因此，需要创建PasswordEncorder的实现类。

```java
package springboot05security.nicolas.config;

import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Component;

@Component
public class MyPasswordEncoder implements PasswordEncoder {
    @Override
    public String encode(CharSequence charSequence) {
        return charSequence.toString();
    }

    @Override
    public boolean matches(CharSequence charSequence, String s) {
        return s.equals(charSequence.toString());
    }
}


```

**自定义securityConfig需要继承WebSecurityConfigurerAdapter**

3、控制请求的访问权限：
configure(HttpSecurity http) {
http.authorizeRequests().antMatchers("/").permitAll()
.antMatchers("/level1/**").hasRole("VIP1")
}

### 4、定义认证规则：

configure(AuthenticationManagerBuilder auth){
auth.inMemoryAuthentication()
.withUser("zhangsan").password("123456").roles("VIP1","VIP2")
}

```java
   //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //super.configure(auth);
        auth.inMemoryAuthentication()
                .withUser("zhangsan").password("123456").roles("VIP1", "VIP2")
                .and()
                .withUser("lisi").password("123456").roles("VIP2", "VIP3")
                .and()
                .withUser("wangwu").password("123456").roles("VIP1", "VIP3");
                /**有以下几种形式，使用第3种*/
        //inMemoryAuthentication 从内存中获取
        //auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder()).withUser("user1").password(new BCryptPasswordEncoder().encode("123123")).roles("USER");
 
        //jdbcAuthentication从数据库中获取，但是默认是以security提供的表结构
        //usersByUsernameQuery 指定查询用户SQL
        //authoritiesByUsernameQuery 指定查询权限SQL
        //auth.jdbcAuthentication().dataSource(dataSource).usersByUsernameQuery(query).authoritiesByUsernameQuery(query);
 
        //注入userDetailsService，需要实现userDetailsService接口
        auth.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());

```



==**使用注入userDetailsService，需要实现userDetailsService接口**==

```java
@Service
public class UserService implements UserDetailsService {
    @Autowired
    UserMapper userMapper;

    @Autowired
    PrivilegeService privilegeService;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userMapper.getUserByName(username);
        if(user == null){
            throw new UsernameNotFoundException("没有该用户");
        }
        return new UserDetailsImpl(user, privilegeService.queryByUserId(user.getUserId()));
    }
}

```



```java
@ToString
public class UserDetailsImpl implements UserDetails{
    private User user;
    private String username;
    private String password;
    //包含着用户对应的所有Privilege，在使用时调用者给对象注入Privileges
    private List<Privilege> privilege;

    @Autowired
    private com.yoj.web.service.PrivilegeService PrivilegeService;

    public void setPrivilege(List<Privilege> privilege) {
        this.privilege = privilege;
    }

    //无参构造
    public UserDetailsImpl() {
    }

    //用User构造
    public UserDetailsImpl(User user) {
        this.username = user.getUserName();
        this.password = user.getPassword();
        this.user = user;
    }

    //用User和List<Privilege>构造
    public UserDetailsImpl(User user,List<Privilege> Privileges) {
        this.user = user;
        this.username = user.getUserName();
        this.password = user.getPassword();
        this.privilege = Privileges;
    }

    public List<Privilege> getPrivilege()
    {
        return privilege;
    }

    @Override
    //返回用户所有角色的封装，一个Privilege对应一个GrantedAuthority
    public Collection<? extends GrantedAuthority> getAuthorities() {
        List<GrantedAuthority> authorities = new ArrayList<>();
        for(Privilege Privilege : privilege) {
            authorities.add(new SimpleGrantedAuthority(Privilege.getRight()));
        }
        return authorities;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }

    @Override
    //判断账号是否已经过期，默认没有过期
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    //判断账号是否被锁定，默认没有锁定
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    //判断信用凭证是否过期，默认没有过期
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    //判断账号是否可用，默认可用
    public boolean isEnabled() {
        return true;
    }
}
```



### 5、开启自动配置的登陆功能：

1、/login来到登陆页
2、重定向到/login?error表示登陆失败
3、更多详细规定
4、默认post形式的 /login代表处理登陆
5、一但定制loginPage；那么 loginPage的post请求就是登陆

```java
@EnableWebSecurity
public class MySecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
 		http.formLogin().usernameParameter("user").passwordParameter("pwd")
                .loginPage("/userlogin");

```



configure(HttpSecurity http){
http.formLogin();
}

### 6、注销：http.logout();

**注意logout时需要表单中的按钮**

1、访问 /logout 表示用户注销，清空session
2、注销成功会返回 /login?logout 页面；

```java
http.logout().logoutSuccessUrl("/");//注销成功以后来到首页

```

```html
<form th:action="@{/logout}" method="post">
    <input type="submit" value="注销"/>
</form>

```



7、记住我：Remeberme()

```html
<form th:action="@{/userlogin}" method="post">
    用户名:<input name="user"/><br>
    密码:<input name="pwd"><br/>
    <input type="checkbox" name="remember"> 记住我<br/>
    <input type="submit" value="登陆">
</form>

```

8获取UserDetails

```java
UserDetailsImpl userDetails = (UserDetailsImpl) SecurityContextHolder.getContext().getAuthentication().getPrincipal();
        System.out.println(userDetails);

```

```java
@EnableWebSecurity
public class MySecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //super.configure(http);
        //定制请求的授权规则
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("VIP1")
                .antMatchers("/level2/**").hasRole("VIP2")
                .antMatchers("/level3/**").hasRole("VIP3");

        //开启自动配置的登陆功能，效果，如果没有登陆，没有权限就会来到登陆页面
        //http.formLogin();
        //1、/login来到登陆页
        //2、重定向到/login?error表示登陆失败
        //3、更多详细规定
        //4、默认post形式的 /login代表处理登陆
        //5、一但定制loginPage；那么 loginPage的post请求就是登陆
        http.formLogin().usernameParameter("user").passwordParameter("pwd")
                .loginPage("/userlogin");

        //开启自动配置的注销功能。
//        http.logout();
        //1、访问 /logout 表示用户注销，清空session
        //2、注销成功会返回 /login?logout 页面；
        http.logout().logoutSuccessUrl("/");//注销成功以后来到首页

        //开启记住我功能
//        http.rememberMe();
        //登陆成功以后，将cookie发给浏览器保存，以后访问页面带上这个cookie，只要通过检查就可以免登录
        //点击注销会删除cookie
        http.rememberMe().rememberMeParameter("remember");
    }

    //定义认证规则
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        //super.configure(auth);
        auth.inMemoryAuthentication()
                .withUser("zhangsan").password("123456").roles("VIP1", "VIP2")
                .and()
                .withUser("lisi").password("123456").roles("VIP2", "VIP3")
                .and()
                .withUser("wangwu").password("123456").roles("VIP1", "VIP3");
                /**有以下几种形式，使用第3种*/
        //inMemoryAuthentication 从内存中获取
        //auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder()).withUser("user1").password(new BCryptPasswordEncoder().encode("123123")).roles("USER");
 
        //jdbcAuthentication从数据库中获取，但是默认是以security提供的表结构
        //usersByUsernameQuery 指定查询用户SQL
        //authoritiesByUsernameQuery 指定查询权限SQL
        //auth.jdbcAuthentication().dataSource(dataSource).usersByUsernameQuery(query).authoritiesByUsernameQuery(query);
 
        //注入userDetailsService，需要实现userDetailsService接口
        auth.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());

    }
}
```

**定义认证用户信息获取来源，密码校验规则等**



## Thymeleaf Extras Springsecurity

注意：thymeleaf和springsecurity版本一致，Thymeleaf Extras **Springsecurity5**

```xml
<!-- https://mvnrepository.com/artifact/org.thymeleaf.extras/thymeleaf-extras-springsecurity5 -->
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity5</artifactId>
    <version>3.0.4.RELEASE</version>
</dependency>
```

```html
div sec:authorize="!isAuthenticated()">
	<h2 align="center">游客您好，如果想查看武林秘籍 <a th:href="@{/userlogin}">请登录</a></h2>
</div>
<div sec:authorize="isAuthenticated()">
	<h2><span sec:authentication="name"></span>，您好,您的角色有：
		<span sec:authentication="principal.authorities"></span></h2>
	<form th:action="@{/logout}" method="post">
		<input type="submit" value="注销"/>
	</form>
</div>
<hr>
<div sec:authorize="hasRole('VIP1')">
	<h3>普通武功秘籍</h3>
	<ul>
		<li><a th:href="@{/level1/1}">罗汉拳</a></li>
		<li><a th:href="@{/level1/2}">武当长拳</a></li>
		<li><a th:href="@{/level1/3}">全真剑法</a></li>
	</ul>
</div>
```

## 在Spring Security中使用AJAX向后台传送数据

本文链接：https://blog.csdn.net/bnrmaster/article/details/52939212

环境：spring 4.2.3

  spring security 4.1.3

表现：

2016-10-26 22:44:02 [http-apr-9080-exec-10] DEBUG org.springframework.security.web.csrf.CsrfFilter - Invalid CSRF token found for XXX
2016-10-26 22:44:02 [http-apr-9080-exec-10] DEBUG org.springframework.security.web.header.writers.HstsHeaderWriter - Not injecting HSTS header since it did not match the requestMatcher org.springframework.security.web.header.writers.HstsHeaderWriter$SecureRequestMatcher@c3339ef
2016-10-26 22:44:02 [http-apr-9080-exec-10] DEBUG org.springframework.security.web.context.SecurityContextPersistenceFilter - SecurityContextHolder now cleared, as request processing completed

前台使用AJAX向后台传输数据时候控制台报出上述错误，再未集成Spring Security时不会出现此现象

解决方法：

如果前端使用的JSP

可以在前端页面的<head>标签中增加两个<meta>标签

如下



```
<html>
<head>
	<meta name="_csrf" content="${_csrf.token}"/>
	<!-- default header name is X-CSRF-TOKEN -->
	<meta name="_csrf_header" content="${_csrf.headerName}"/>
	<!-- ... -->
</head>
```





如果前端使用的是Thymeleaf分两种情况

1.前端无form表单,也要再头部增加两个meta标签，形式为



```
<html>
<head>
	<meta name="_csrf" th:content="${_csrf.token}" content=""/>
	<!-- default header name is X-CSRF-TOKEN -->
	<meta name="_csrf_header" th:content="${_csrf.headerName}" content=""/>
	<!-- ... -->
</head>
```





2.前端有form表单

  Spring Security为Thymeleaf中的表单中自动添加一个<input type="hidden" name = "_csrf" value="xxxxxxxxxx">  (xxxx为crrf.token)



添加完meta之后不妨运行下，在页面代码中搜索_csrf，可以看看附近代码的样子，应该就会明白了



这样在使用AJAX时，需要增加一个头部



```
var token = $("meta[name='_csrf']").attr("content");
var header = $("meta[name='_csrf_header']").attr("content");
	 $.ajax({  
                   type: "POST",  
                   url: "myposturl",  
                   data: entID, 
                   contentType:"application/json; charset=utf-8",
                   headers : {header:token},
                   async:false,
                   success:function(data){  
	       		//do something
                   },
                   error: function () {
                	//deal width error    
                   }
            });  
```

实际上，这里的header使用为值"X-CSRF-TOKEN" 这样就可以成功向后台请求了

**spring security reference**

```java
var csrfHeader = $("meta[name='_csrf_header']").attr("content");
var csrfToken = $("meta[name='_csrf']").attr("content");
var headers = {};
headers[csrfHeader] = csrfToken;
console.log(problem);
$.ajax({
    url: "/p/add",
    type: "POST",
    headers : headers,
    data: problem,
    success(res){
        // $("id").add(res);
        console.log(res);
    },
    error(res){
        console.log(res);
    }
})
```



```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSRF Protected JavaScript Page</title>
        <meta name="description" content="This is the description for this page" />
        <sec:csrfMetaTags />
        <script type="text/javascript" language="javascript">

            var csrfParameter = $("meta[name='_csrf_parameter']").attr("content");
            var csrfHeader = $("meta[name='_csrf_header']").attr("content");
            var csrfToken = $("meta[name='_csrf']").attr("content");

            // using XMLHttpRequest directly to send an x-www-form-urlencoded request
            var ajax = new XMLHttpRequest();
            ajax.open("POST", "https://www.example.org/do/something", true);
            ajax.setRequestHeader("Content-Type", "application/x-www-form-urlencoded data");
            ajax.send(csrfParameter + "=" + csrfToken + "&name=John&...");

            // using XMLHttpRequest directly to send a non-x-www-form-urlencoded request
            var ajax = new XMLHttpRequest();
            ajax.open("POST", "https://www.example.org/do/something", true);
            ajax.setRequestHeader(csrfHeader, csrfToken);
            ajax.send("...");

            // using JQuery to send an x-www-form-urlencoded request
            var data = {};
            data[csrfParameter] = csrfToken;
            data["name"] = "John";
            ...
            $.ajax({
                url: "https://www.example.org/do/something",
                type: "POST",
                data: data,
                ...
            });

            // using JQuery to send a non-x-www-form-urlencoded request
            var headers = {};
            headers[csrfHeader] = csrfToken;
            $.ajax({
                url: "https://www.example.org/do/something",
                type: "POST",
                headers: headers,
                ...
            });

        <script>
    </head>
    <body>
        ...
    </body>
</html>
```

























六、Spring Boot与分布式

七、Spring Boot与监控管理

八、Spring Boot与部署



# 七.开发热部署

## 一、热部署

在开发中我们修改一个Java文件后想看到效果不得不重启应用，这导致大量时间花费，我们希望不重启应用的情况下，程序可以**自动部署（热部署）**。有以下四种情况，如何能实现热部署。



•1、模板引擎

–在Spring Boot中开发情况下禁用模板引擎的cache

–页面模板改变ctrl+F9可以重新编译当前页面并生效

2、Spring Loaded

Spring官方提供的热部署程序，实现修改类文件的热部署

–下载Spring Loaded（项目地址https://github.com/spring-projects/spring-loaded）

–添加运行时参数；

-javaagent:C:/springloaded-1.2.5.RELEASE.jar –noverify

  

3、JRebel

–收费的一个热部署软件

–安装插件使用即可

## 4、Spring Boot Devtools（推荐）

–引入依赖

```xml
<dependency>  
       <groupId>org.springframework.boot</groupId>  
       <artifactId>spring-boot-devtools</artifactId>   
</dependency> 
```

–**IDEA使用ctrl+F9**

–或做一些小调整

  *Intellij* *IEDA**和**Eclipse**不同，**Eclipse**设置了自动编译之后，修改类它会自动编译**，而**IDEA**在非**RUN**或**DEBUG**情况下才会自动编译（前提是你已经设置了**Auto-Compile**）。*

•设置自动编译（settings-compiler-make project automatically）

•ctrl+shift+alt+/（maintenance）

•勾选compiler.automake.allow.when.app.running