---
title: pythonMongoDb
date: 2019-11-14 22:19:09
tags: python
---
[TOC]

python操作mongodb

[菜鸟教程](https://www.runoob.com/python3/python-mongodb.html)

[官方中文文档](http://www.mongoing.com/docs/tutorial/query-documents.html#specify-query-filter-conditions)

## 连接mongodb

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

from pymongo import MongoClient

conn = MongoClient('192.168.0.113', 27017)
db = conn.mydb  #连接mydb数据库，没有则自动创建
my_set = db.test_set　　#使用test_set集合，没有则自动创建
```

## 插入数据

# 查询数据

https://www.runoob.com/python3/python-mongodb-query-document.html

## 查询一条数据

我们可以使用 **find_one()** 方法来查询集合中的一条数据。

```python
#!/usr/bin/python3
 
import pymongo
 
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
mycol = mydb["sites"]
 
x = mycol.find_one()
 
print(x)
```

## 查询集合中所有数据

**find()** 方法可以查询集合中的所有数据，类似 SQL 中的 **SELECT \*** 操作。

以下实例查找 **sites** 集合中的所有数据：

```python
#!/usr/bin/python3
 
import pymongo
 
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
mycol = mydb["sites"]
 
for x in mycol.find():
  print(x)
```

## $exists查找字段是否存在

判断字段是否存在

```python
db.users.find({age: {"$exists": True}});
```

## $or

```
{ $or: [ { <expression1> }, { <expression2> }, ... , { <expressionN> } ] }
```

```
db.inventory.find( { $or: [ { quantity: { $lt: 20 } }, { price: 10 } ] } )
```

## $ne不等于

查询x 的值不等于3 的数据

```python
query = {Problem.DATA_STATUS: {"$ne": StateValue.FILE_SUCCESS}}
mongo_util.problem_collection.find(query)
```

举例如下:

C1 表的数据如下:

```
> db.c1.find()
{ "_id" : ObjectId("4fb4af85afa87dc1bed94330"), "age" : 7, "length_1" : 30 }
{ "_id" : ObjectId("4fb4af89afa87dc1bed94331"), "age" : 8, "length_1" : 30 }
{ "_id" : ObjectId("4fb4af8cafa87dc1bed94332"), "age" : 6, "length_1" : 30 }
```

查询age 的值不等于7 的数据

```
> db.c1.find( { age : { $ne : 7 } } );
{ "_id" : ObjectId("4fb4af89afa87dc1bed94331"), "age" : 8, "length_1" : 30 }
{ "_id" : ObjectId("4fb4af8cafa87dc1bed94332"), "age" : 6, "length_1" : 30 }
```

可以看出只显示出了age 不等于7 的数据，其它不符合规则的数据并没有显示出来

在users文档中查询"age"不等于20的记录：

```
db.users.find({age:{$ne:20}});
```

## 高级查询

查询的条件语句中，我们还可以使用修饰符。

以下实例用于读取 name 字段中第一个字母 ASCII 值大于 "H" 的数据，大于的修饰符条件为 **{"$gt": "H"}** :

```python
#!/usr/bin/python3
 
import pymongo
 
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
mycol = mydb["sites"]
 
myquery = { "name": "RUNOOB" }
 
mydoc = mycol.find(myquery)
 
for x in mydoc:
  print(x)
```

## 使用正则表达式查询

我们还可以使用正则表达式作为修饰符。

正则表达式修饰符只用于搜索字符串的字段。

以下实例用于读取 name 字段中第一个字母为 "R" 的数据，正则表达式修饰符条件为 **{"$regex": "^R"}** :

```python
#!/usr/bin/python3
 
import pymongo
 
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
mycol = mydb["sites"]
 
myquery = { "name": { "$regex": "^R" } }
 
mydoc = mycol.find(myquery)
 
for x in mydoc:
  print(x)
```

## 返回指定条数记录

如果我们要对查询结果设置指定条数的记录可以使用 **limit()** 方法，该方法只接受一个数字参数。

# 计数count

```python
problem_table.count_documents(query)
```



```python
query = {"id": {"$regex": "^" + tag_dict[name]}}
print(query)
query_cnt = problem_table.count_documents(query)
```

# 查询值为Null或不存在的字段

The `{ name : null }` query matches documents that either contain the `name` field whose value is `null` *or* that do not contain the `name` field.

给出如下查询:

```python
db.users.find( { name: null } )
```

# 更新数据

https://www.runoob.com/python3/python-mongodb-update-document.html

```python
import pymongo
 
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
mycol = mydb["sites"]
 
myquery = { "alexa": "10000" }
newvalues = { "$set": { "alexa": "12345" } }
 
mycol.find_one_update(myquery, newvalues)
 
# 输出修改后的  "sites"  集合
for x in mycol.find():
  print(x)
```

## 删除字段

```python
db.yourcollection.update_one({}, {"$unset": {"name": ""}})
```

```
db.yourcollection.update_many({}, {"$unset": {"name": ""}})
```

## 添加字段

直接使用update即可

```python
test_collection.update_one({"id":"test1"}, {"$set": {"new_field": "xxx"}})
```



# 删除数据

# 排序

**sort()** 方法可以指定升序或降序排序。

**sort()** 方法第一个参数为要排序的字段，第二个字段指定排序规则，**1** 为升序，**-1** 为降序，默认为升序。

对字段 alexa 按升序排序：

```python
#!/usr/bin/python3
 
import pymongo
 
myclient = pymongo.MongoClient("mongodb://localhost:27017/")
mydb = myclient["runoobdb"]
mycol = mydb["sites"]
 
mydoc = mycol.find().sort("alexa")
for x in mydoc:
  print(x)
```

