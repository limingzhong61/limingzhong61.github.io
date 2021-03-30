---
title: jquery
p: js-note/jquery/jquery
date: 2019-10-12 10:25:41
categories: js
tags: [js,前端]
---

[TOC]



# jQuery入口函数

## jQuery与JavaScript加载模式对比

- **多个window.onload只会执行一次, 后面的会覆盖前面的**

```javascript
<script>
        window.onload = function () {
            alert("hello lnj1"); // 不会显示
        }
        window.onload = function () {
            alert("hello lnj2"); // 会显示
        }
</script>
```

- **多个$(document).ready()会执行多次,后面的不会覆盖前面的**

```javascript
<script>
        $(document).ready(function () {
            alert("hello lnj1"); //会显示
        });
        $(document).ready(function () {
            alert("hello lnj2"); // 会显示
        });
</script>
```

- *不会覆盖的本质(了解,后面jQuery原理会详细讲解)*
  - jQuery框架本质是一个闭包,每次执行我们都会给ready函数传递一个新的函数,不同函数内部的数据不会相互干扰

```javascript
<script>
        // 相当于这样写
        var test1 = function () {
            alert("hello lnj1");
        }
        var test2 = function () {
            alert("hello lnj2");
        }
        $(document).ready(test1);
        $(document).ready(test2);
</script>
<script>
        var test3 = function () {
            var abc = "123";
//            因为在同一个函数中,所以456覆盖123
//            var abc = "456"; 
            alert(abc);
        }
        test3();
        var test4 = function () {
            // 因为在不同函数中,所以不会影响
            var abc = "456"; 
            alert(abc);
        }
        test4();
</script>
```

|          | window.onload                                            | $(document).ready()                                   |
| -------- | -------------------------------------------------------- | ----------------------------------------------------- |
| 执行时机 | 必须等待网页全部加载完毕(包括 图片等),然后再执行包裹代码 | 只需要等待网页中的DOM结构 加载完毕,就能执行包裹的代码 |
| 执行次数 | 只能执行一次,如果第二次,那么 第一次的执行会被覆盖        | 可以执行多次,第N次都不会被上 一次覆盖                 |
| 简写方案 | 无                                                       | $(function () { });                                   |

------

- **为什么我们能访问$符号?**
  - 因为$符号jQuery框架对外暴露的一个全局变量
- **JavaScript中如何定义一个全局变量?**
- 所有全局变量是 window 对象的属性

```javascript
        function test () {
            var customValue = 998;
            alert(customValue);
//            1.没有如下代码customValue就不是一个全局变量,函数执行完毕之后
//            customValue会被自动释放,test函数以外的地方访问不到customValue
//            2.加上如下代码之后customValue就会变成一个全局变量,函数执行完毕也不
//            会被释放,test函数以外的地方可以访问customValue
//            window.customValue = customValue;
        }
        test();
        alert(customValue);
```

- 所以jQuery框架源码实现

```
window.jQuery = window.$ = jQuery;
```

- 所以想要使用jQuery框架只有两种方式,一种是通过$,一种是通过jQuery

------

### **jQuery入口函数**

的其它编写方式如下

```javascript
<script>
        jQuery(function () {
            alert("hello lnj");
        });
</script>
```



```javascript
<script>
        // 方式一
        $(document).ready(function () {
            alert("hello lnj");
        });
        // 方式二
        $(function () {
            alert("hello lnj");
        });
        // 方式三
        jQuery(document).ready(function () {
            alert("hello lnj");
        });
        // 方式四
        jQuery(function () {
            alert("hello lnj");
        });
</script>
```

------

### 解决$符号冲突问题

- **为什么是window.jQuery = window.$ = jQuery;,而不是window.jQuery  = jQuery;**
  - jQuery框架之所以提供了jQuery访问还提供$访问,就是为了提升开发者的编码效率
- **$符号冲突怎么办?**
  - 很多js的框架都提供了类似jQuery这样的便捷访问方式,所以很有可能某一天我们在使用多个框架的时,多个框架作者提供的便捷访问方式冲突(A框架通过$访问,B框架也通过$访问)
- ***释放$使用权
  - 当便捷访问符号发生冲突时,我们可以释放$使用权, 释放之后只能使用jQuery

```javascript
<script>
        // 在使用jQuery之前指定自定义符号
        jQuery.noConflict();
        // 使用 jQuery
        jQuery("div p").hide();
        // 使用其他库的 $()
        $("content").style.display = 'none';
</script>
```

- *自定义便捷访问符号*
  - 当便捷访问符号发生冲突时,我们可以自定义便捷访问符号

```javascript
<script>
        // 在使用jQuery之前指定自定义符号
        var nj = jQuery.noConflict();
        // 和使用$一样通过自定义符号调用jQuery
        nj(function () {
            alert("hello lnj");
        });
</script>
```



# 核心函数和静态方法

### jQuery核心函数

- **从jQuery文档中可以看出,jQuery核心函数一共3大类4小类**
- *jQuery(callback)*
  - 当DOM加载完成后执行传入的回调函数

```javascript
<script>
        $(function () {
            alert("123");
        });
</script>
```

------

- *jQuery([sel,[context]])*
  - 接收一个包含 CSS 选择器的字符串，然后用这个字符串去匹配一组元素,并包装成jQuery对象

```javascript
<script>
        $(function () {
            // 利用jquery获取所有div,得到的是一个jQuery对象
            var $box = $("div");
            console.log($box);

            // 利用js原生语法获取所有div,得到的是一个js对象
            var box = document.getElementsByTagName("div");
            console.log(box);
        });
</script>
```

- 原生JS对象和jQuery对象相互转换

```javascript
<script>
        $(function () {
            var $box = $("#box");
//            $box.text("新的数据");
//            jQuery对象不能使用原生js对象的方法
//            $box.innerText = "新的数据";
//            将jQuery对象转换为原生js对象
//            注意: 不是eq(0),eq函数返回的是jQuery类型对象,get函数返回的是原生类型对象
//            var box = $box.get(0);
            var box = $box[0];
            box.innerText = "新的数据";

            var box2 = document.getElementById("box");
//            原生js对象不能使用jQuery对象的方法
//            box2.text("新的数据2");
//            原生js对象只能使用原生的js方法
//            box2.innerText = "新的数据2";

//            将原生js对象转换为jQuery对象
            var $box2 = $(box);
           $box2.text("新的数据2");
        });
</script>
```

> Tips:为了方便开发者之间沟通和阅读,一般情况下所有jQuery操作相关的变量前面加上$

------

- *jQuery(html,[ownerDoc])*
  - 根据 HTML 标记字符串，动态创建DOM 元素

```javascript
<script>
        $(function () {
            var $eles = $("<p>我是span</p><u>我是u</u>");
            // 无论是jQuery找到的还是创建的,我们最终拿到的永远都是jQuery对象
            console.log($eles);
            // 将创建好的DOM元素添加到body中
            $("body").append($eles);
        });
</script>
```

------

### jQuery对象

- jQuery对象的本质是什么? 
  - jQuery对象的本质是一个伪数组

```javascript
var $div = $("div");
console.log($div);

var arr = [1, 3, 5];
console.log(arr);
```

- 什么是伪数组? 
  - 有0到length-1的属性
  - 并且有length属性

```javascript
var obj = {0:"lnj", 1:"33", 2:"male", length: 3}
```

## jQuery静态方法



- 什么是静态方法? 
  - **静态方法对应的是对象方法**,对象方法用实例对象调用,而静态方法用类名调用

```javascript
 <script>
        window.onload = function () {
            function AClass(){}
            AClass.staticMethof = function(){
                alert('静态方法');
            }
            AClass.prototype.instaceMethod = function(){
                alert('实例方法');
            }
            //静态方法用类名直接调用
            AClass.staticMethof(); 

            //实例方法必须用类的实例对象调用
            var instace = new AClass();
            instace.instaceMethod();
        }
</script>
```

- *jQuery.holdReady(hold)*
  - 暂停或者恢复jQuery.ready()事件
  - 传入true或false

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04-jQuery静态方法</title>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        // 使用$直接调用,是静态方法
        $.holdReady(true);
        $(function () {
            $("#first").click(function () {
                alert("我是你想要的弹窗");
            });
        });
    </script>
</head>
<body>
<button id="first">点击测试弹出</button>
<button id="second">解除延迟</button>
<script>
    $("#second").click(function(){
        $.holdReady(false);
    });
</script>
</body>
</html>
```

------

- *$.each(object,[callback])*
  - 遍历对象或数组
  - 优点统一遍历对象和数组的方式
  - 回调参数的顺序更符合我们的思维模式

```javascript
<script>
        $(function () {
            // 3.1遍历数组
            var arr = [1, 3, 5, 7, 9];
            // 3.1.1通过原生方法遍历数组
            // 第一个回调函数参数是遍历到的元素
            // 第二个回调函数参数是当前遍历的索引
            // 返回值: 没有返回值
            var res = arr.forEach(function (ele, idx) {
                console.log(idx, ele);
            });
            console.log(res);

            // 3.1.2通过jQuery静态方法遍历数组
            // 第一个回调函数参数是当前遍历的索引
            // 第二个回调函数参数是遍历到的元素
            // 返回值: 被遍历的数组
            var $res2 = $.each(arr, function (idx, ele) {
                console.log(idx, ele);
            });
            console.log($res2);

            // 3.2遍历对象
            var obj = {name: "lnj", age:"33", gender:"male"};
            // 3.2.1js对象没有forEach方法,所以通过forin方法遍历对象
            for(var key in obj){
                console.log(key, obj[key]);
            }
            // 3.2.2通过jQuery静态方法遍历对象
            $.each(obj,function (key, value) {
                console.log(key, value);
            });
        });
    </script>
```

------

- *$.map(arr|obj,callback)*
  - 遍历对象或数组,将回调函数的返回值组成一个新的数组返回

```javascript
        $(function () {
            // 4.1遍历数组
            var arr = [1, 3, 5, 7, 9];
            // 4.1.1通过原生方法遍历数组
            // 第一个回调函数参数是遍历到的元素
            // 第二个回调函数参数是当前遍历的索引
            // 第三个回调函数参数是当前被遍历的数组
            // 返回值: 将回调函数返回值收集起来组成一个新的数组
            var res = arr.map(function (ele, idx, arr) {
                console.log(idx, ele, arr);
                return ele + idx;
            });
            console.log(res);
            
            // 4.1.2通过jQuery静态方法遍历数组
            // 第一个回调函数参数是遍历到的元素
            // 第二个回调函数参数是当前遍历的索引
            // 返回值: 将回调函数返回值收集起来组成一个新的数组
            var $res2 = $.map(arr, function (ele,idx) {
                console.log(idx, ele);
                return ele + idx;
            });
            console.log($res2);

            // 4.2遍历对象
            var obj = {name: "lnj", age:"33", gender:"male"};
            /*
            obj.map(function (ele, idx, obj) {
                // 报错,原生JS没有map方法
                console.log(idx, ele, obj);
            });
            */
            var $res = $.map(obj, function (value, key) {
                console.log(key, value);
                return key + value;
            });
            console.log($res);
        });
```

------

- *$.trim(str)*
  - 去掉字符串起始和结尾的空格。

```javascript
<script>
        $(function () {
            var str = "   lnj   ";
            console.log("---"+str+"---");
            var $res = $.trim(str);
            console.log("---"+$res+"---");
        });
</script>
```

------

- **$.isArray(obj)**

- 判断是否是数组

```javascript
<script>
        $(function () {
            // 对象
            var obj = {name:"lnj",age: "33", gender:"male"};
            // 真数组
            var arr = [1, 3, 5, 7, 9];
            var $res = $.isArray(obj);
            console.log($res);// false
            var $res2 = $.isArray(arr);
            console.log($res2);// true
        });
</script>
```

------

- *$.isFunction(obj)*
  - 判断是否是函数
  - jQuery框架本质是一个匿名函数

```javascript
<script>
        $(function () {
            var obj = {name:"lnj",age: "33", gender:"male"};
            var arr = [1, 3, 5, 7, 9];
            var fn = function () {}
            var $res = $.isFunction(obj);
            console.log($res);// false
            $res = $.isFunction(arr);
            console.log($res);
            $res = $.isFunction(fn);
            console.log($res);
            // 通过该方法验证了我们前面所说的,jQuery框架本质是一个匿名函数
    		(function(window,undefined){
                xxxx....
            })(window);
            $res = $.isFunction($);
            console.log($res);
        });
</script>
```

------

- *$.isWindow(obj)*
  - 判断是否是window对象

```javascript
<script>
        $(function () {
            var obj = window;
            var arr = [1, 3, 5, 7, 9];
            var arrlike = {0:"zs", 1:"ls", length:2};
            var $res = $.isWindow(obj);
            console.log($res); // true
            $res = $.isWindow(arr);
            console.log($res); // false
            $res = $.isWindow(arrlike);
            console.log($res); // false
        });
</script>
```

> 为什么要讲解以上极度简单的工具方法?
>  江哥提示: 这是为后面放大招做铺垫,一定要认真记住以上方法哦

# 基础选择器

- **视频参考第十章-CSS选择器**

| 选择器                        | 名称         | 描述                                   | 返回     | 示例                                                         |
| ----------------------------- | ------------ | -------------------------------------- | -------- | ------------------------------------------------------------ |
| #id                           | id选择器     | 根据给定的id匹配一个元素               | 单个元素 | $("#box");选取id为box元素                                    |
| .class                        | 类选择器     | 根据给定的类名匹配元素                 | 集合元素 | $(".box");选取所有类名为box元素                              |
| element                       | 元素选择器   | 根据给定的元素名称匹配元素             | 集合元素 | $("p");选取所有<p>元素                                       |
| *                             | 通配符选择器 | 匹配所有元素                           | 集合元素 | $("*");选取所有元素                                          |
| selector1,selector2,selectorN | 并集选择器   | 将所有选择器匹配到的元素合并后一起返回 | 集合元素 | $("div,p,.box");选取所有<div>元素,所有<p>元素和所有类名为box元素 |

## 层次选择器

| 选择器                   | 名称           | 描述                                                         | 返回     | 示例                                                   |
| ------------------------ | -------------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------ |
| $("ancestor descendant") | 后代选择器     | 选取ancestor元素的所有descendant后代标签(不光是儿子,包括孙子/重孙子等) | 集合元素 | $("div span");选取<div>元素里所有的<span>元素          |
| $("parent > child")      | 子元素选择器   | 找到选取parent 元素中所有直接子元素child(只有儿子,不包括孙子/重孙子等) | 集合元素 | $("div>span");选取<div>元素下元素名称是<span>的子元素  |
| $("prev + next")         | 相邻兄弟选择器 | 选取prev元素后面紧跟的那个next元素                           | 集合元素 | $(".one+div");选取类名为one的下一个同级的<div>元素     |
| $("prev ~ siblings")     | 通用兄弟选择器 | 选取prev元素后面的所有next元素                               | 集合元素 | $("#two~div");选取id名为two元素后面所有同级的<div>元素 |

### 序选择器

- **视频参考第十章-CSS选择器**

> 如上内容不再一一赘述,观看第十章-CSS选择器,使用时查询文档即可
>  做开发是脑力活,我们需要掌握的是解决问题的方法,而不是死记硬背

## 属性选择器

- **视频参考第十章-CSS选择器**

> 如上内容不再一一赘述,观看第十章-CSS选择器,使用时查询文档即可
>  做开发是脑力活,我们需要掌握的是解决问题的方法,而不是死记硬背

### 内容过滤选择器

| 选择器          | 描述                             | 返回     |
| --------------- | -------------------------------- | -------- |
| :empty          | 选取不包含子元素或文本为空的元素 | 集合元素 |
| :parent         | 选取含有子元素或文本的元素       | 集合元素 |
| :contains(text) | 选取含有文本内容为text的元素     | 集合元素 |
| :has(selector)  | 选取含有选择器所匹配的元素的元素 | 集合元素 |

- **:empty**

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-jQuery选择器</title>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
           // 找到所有div中不包含文本内容或子元素的元素
            var $res = $("div:empty");
            console.log($res.length); // 找到1个元素
            $res.each(function (idx,ele) {
                console.log(idx, ele); // one
            });
        });
    </script>
</head>
<body>
<div class="one"></div>
<div class="two">zs</div><!--包含内容不会被找到-->
<div class="three"><!--包含子元素不会被找到-->
    <span>lnj</span>
</div>
<span class="five"></span><!--不是指定元素不会被找到-->
</body>
</html>
```

- **:parent**

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-jQuery选择器</title>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
           // 找到所有div中包含文本内容或子元素的元素
            var $res = $("div:parent");
            console.log($res.length);
            $res.each(function (idx, ele) {
                console.log(idx, ele);
            });
        });
    </script>
</head>
<body>
<div class="one"></div>
<div class="two">zs</div><!--有文本内容会被找到-->
<div class="three"><!--有子元素会被找到-->
    <span>lnj</span>
</div>
<span class="five"></span>
</body>
</html>
```

- **:contains(text)**

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-jQuery选择器</title>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 找到所有div中包含文本内容为lnj的元素
            var $res = $("div:contains('lnj')");
            console.log($res.length);// 找到2个元素
            $res.each(function (idx, ele) {
                console.log(idx, ele);// one,three,four
            })
        });
    </script>
</head>
<body>
<div class="one">lnj</div>
<div class="two">zs</div>
<div class="three">lnj</div>
<div class="four"><!--子元素中包含该文本也会被找到-->
    <span>lnj</span>
</div>
<span class="five"></span>
</body>
</html>
```

- *:has(selector)*
  - 和:parent区别,parent只要有子元素就会被找到,:has(selector)不仅要有子元素,而且子元素还必须满足我们的条件

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>05-jQuery选择器</title>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            // 找到所有div中包含后代元素为span的元素
            var $res = $("div:has('span')");
            console.log($res.length);
            $res.each(function (idx, ele) {
                console.log(idx, ele);
            });
        });
    </script>
</head>
<body>
<div class="one"> <!--后代中包含span元素会被找到-->
    <span>jjj</span>
</div>
<div class="two"><!--后代中不包含span元素不会被找到-->
    <p>zs</p>
</div>
<div class="three"><!--后代中包含span元素会被找到-->
    <p>
        <span>lnj</span>
    </p>
</div>
</body>
</html>
```

# 属性相关

##属性和属性节点

- *什么是属性?*
  - 属性就是对象身上的变量
  - 只要对象身上都可以添加属性(*无论是自定义对象,还是DOM对象*)

```
<script>
            // 1.自定义一个对象
            var obj = {};
            console.log(obj);
            // 2.动态给自定义对象添加属性
            obj.name = "lnj"; // name就是对象obj的一个属性
            obj.age = 33; // age就是对象obj的一个属性
            console.log(obj);
</script>
```



![img](https:////upload-images.jianshu.io/upload_images/647982-d31bfc904163474f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

image.png

- *什么是属性节点?*
  - 在html中编写的所有标签，里面的属性都是属性节点 
    - `<span class = 'box' nj = '666'></span> // 这里的class和nj就是属性节点`



![img](https:////upload-images.jianshu.io/upload_images/647982-7035a4affa5465ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

image.png

- *如果操作属性?*
  - 添加或修改属性(没有就会添加,有就会修改) 
    - `对象.属性名称 = 值;`
    - `对象["属性名称"] = 值;`
  - 获取属性 
    - `对象.属性名称`
    - `对象["属性名称"]`

- *如何操作属性节点?*
  - 获取属性节点 
    - `DOM对象.getAttribute("属性节点名称")`
  - 设置属性节点 
    - `DOM对象.setAttribute("属性节点名称", "值");`

------

### jQuery中的attr和prop方法

- *attr(name|pro|key,val|fn)方法*
  - 用于设置或获取属性节点的值

```javascript
<script>
        $(function () {
            // 1.获取指定属性节点值
            var $res = $(".span1").attr("nj");
            console.log($res);
            // 2.设置属性节点
            $(".span1").attr("nj", "666");
            $(".span2").attr("id", "box1 box2");

            // 3.注意点:
            // 3.1.获取属性节点时,只会获取找到所有元素中第一个元素的属性节点
            $res = $("span").attr("class");
            console.log($res);
             $("span").attr("class", "lnj");
        });
</script>
```

- *removeAttr(name)方法*
  - 用于删除指定属性节点

```javascript
<script>
        $(function () {
            // 1.设置属性节点时,会给所有找到元素设置属性节点
            $("span").attr("test", "jonathan");
            // 2.删除属性节点时,会删除所有找到元素的属性节点
            $("span").removeAttr("test");
        });
</script>
```

- *prop(n|p|k,v|f)方法*
  - 用于设置或者获取元素的属性值

```javascript
<script>
        $(function () {
            // 1.设置属性
            // 1.1.设置属性时,会设置所有找到元素的属性
            $("span").prop("demo", "lnj");
            // 2.获取属性
            // 2.1.获取属性时,只会获取找到第一个元素的属性
            console.log($("span").prop("demo"));
        });
</script>
```

- **removeProp(name)方法**

```javascript
<script>
        $(function () {
            // 删除所有找到元素的demo属性
            $("span").removeProp("demo");
        });
</script>
```

- *attr方法和prop方法区别*
  - 既然所有的DOM对象，都有一个attributes属性,而prop可以操作属性,所以也可以操作属性节点
  - 官方推荐在操作属性节点时,**具有 true 和 false 两个属性的属性节点，如 checked, selected 或者 disabled 使用prop()，其他的使用 attr()**
  - 因为如果具有 true 和 false 两个属性的属性节点,如果没有编写默认attr返回undefined,而prop返回false

```javascript
<script>
        $(function () {
            // 1.可以通过prop获取属性节点
            console.log($("input").prop("class"));
            // 2.可以通过prop设置属性节点
            $("input").prop("class", "tag");

            // 3.如果没有默认值,那么attr获取返回undefined
//            console.log($("input[type=checkbox]").attr("checked"));
            // 4.如果没有默认值,那么prop获取返回false
            console.log($("input[type=checkbox]").prop("checked"));
            // 5.通过attr设置选中
//            $("input[type=checkbox]").attr("checked", true);
            
            // 6.通过prop设置选中
            $("input[type=checkbox]").prop("checked", true)

        });
</script>
```

### jQuery增删Class

- **jQuery CSS类相关方法都是用于操作DOM对象的class属性节点的值**
- *addClass(class|fn)*
  - 给元素添加一个或多个类

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>08-jQueryCSS类</title>
    <style>
        .class1{
            width: 200px;
            height: 200px;
            background-color: red;
        }
        .class2{
            border: 5px solid #000;
        }
    </style>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
           $("button").eq(0).click(function () {
               // 1.添加一个类
//               $("div").addClass("class1");
               // 2.再添加一个类
//               $("div").addClass("class2");
               // 3.一次性添加多个类(用空格隔开)
               $("div").addClass("class1 class2");
           });
        });
    </script>
</head>
<body>
<button>添加</button>
<button>删除</button>
<button>切换</button>
<div></div>
</body>
</html>
```

- *removeClass([class|fn])*
  - 删除元素的一个或多个类

```javascript
<script>
        $(function () {
            $("button").eq(1).click(function () {
                // 4.删除一个类
//                $("div").removeClass("class2");
                // 5.再删除一个类
//                $("div").removeClass("class1");
                // 6.一次性删除多个类(用空格隔开)
                $("div").removeClass("class1 class2");

            });
        });
</script>
```

- *toggleClass(class|fn[,sw])*
  - 添加或删除一个类(存在就删除不存在就添加)

```javascript
<script>
        $(function () {
            $("button").eq(2).click(function () {
                // 7.切换一个类
//                $("div").toggleClass("class2");
                // 8.切换多个类
                $("div").toggleClass("class1 class2");
            });
        });
    </script>
```

------

### jQuery代码/文本/值

- *html([val|fn])*
  - 添加或获取元素中的HTML

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09-jQuery代码文本值</title>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            var $btns = $("button");
            var $div = $("div");
            $btns.eq(0).click(function () {
                // 1.添加html, 相当于innerHTML
//                $div.html("<p>我是p标签</p>");
//                $div.html("<p><span>我是span标签</span></p>");
                $div.html("我是文本");
            });
            $btns.eq(1).click(function () {
                // 2.获取html
                console.log($div.html());
            });
        });
    </script>
</head>
<body>
<button>添加html</button>
<button>获取html</button>
<button>添加文本</button>
<button>获取文本</button>
<div></div>
</body>
</html>
```

- *text([val|fn])*
  - 添加或获取元素中的文本
  - **text方法能做的html方法都能做,所以一般使用html方法即可**

```javascript
<script>
        $(function () {
            $btns.eq(2).click(function () {
                // 3.添加文本, 相当于innerText
                // 如下内容不会被转换为标签
//                $div.text('<p>我是段落</p>');
               $div.text('我是文本');
            });
            $btns.eq(3).click(function () {
                // 4.获取文本
                console.log($div.text());
            });
</script>
```

- *val([val|fn|arr])*
  - 添加或获取元素value属性的值

```javascript
<script>
        $(function () {
            $btns.eq(4).click(function () {
                // 4.添加value值
                $("input").val("我是一个输入框");
            });
            $btns.eq(5).click(function () {
                // 4.获取value值
                console.log($("input").val());
            });
        });
</script>
```

# CSS操作

## jQuery操作CSS样式

- *css(name|pro|[,val|fn])方法*
  - 用于设置或获取元素CSS样式
  - 格式1:`DOM元素.css("样式名称", "值");` 
  - 格式2:`DOM元素.css({"样式名称1":"值1","样式名称2":"值2"});` 

```javascript
    <script>
        $(function () {
            $("button").click(function () {
                // 1.单个样式设置
//                $("div").css("width", "100px");
//                $("div").css("height", "100px");
//                $("div").css("background", "red");

                // 2.链式设置样式
//                $("div").css("width", "100px").css("height", "100px").css("background", "red");

                // 3.传入对象一次性设置样式
                $("div").css({
                   "width":"100px",
                    "height":"100px",
                    "background":"blue"
                });

                // 4.获取指定样式的值
                console.log($("div").css("width"));
            });
        });
    </script>
```

------

### jQuery操作元素尺寸

- *width([val|fn])方法*
  - 设置或获取元素宽度(相当于获取width属性值)

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11-jQuery操作位置和尺寸</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .father{
            width: 250px;
            height: 250px;
            background-color: red;
            margin-left: 50px;
            position: relative;
        }
        .son{
            width: 100px;
            height: 100px;
            background-color: blue;
            position: absolute;
            left: 50px;
            top: 50px;
        }
    </style>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            $("button").eq(0).click(function () {
                // 1.获取元素宽度(不包括padding和border)
//                alert($('.son').width());
            });
            $("button").eq(1).click(function () {
                // 2.设置元素宽度(不包括padding和border)
//                $(".son").width("50px");
            });
        });
    </script>
</head>
<body>
<div class="father">
    <div class="son"></div>
</div>
<button>获取</button>
<button>设置</button>
</body>
</html>
```

- **height([val|fn])方法**
  - 设置或获取元素宽度(相当于获取height属性值)
  - 用上面按钮代码自己写,工作后都得靠自己,多锻炼自学能力(如何查看文档,如何编写测试案例等)
- **innerHeight()/innerWidth()**
  - 用上面按钮代码自己写,工作后都得靠自己,多锻炼自学能力(如何查看文档,如何编写测试案例等)
- **outerHeight/outerWidth()**
  - 用上面按钮代码自己写,工作后都得靠自己,多锻炼自学能力(如何查看文档,如何编写测试案例等)

------

### jQuery操作元素位置

- *offset([coordinates])*
  - 获取或设置元素相对窗口的偏移位

```
    <script>
        $(function () {
            $("button").eq(0).click(function () {
                // 1.获取距离窗口的偏移位(从border开始)
                alert($('.son').offset().left); // 100
            });
            $("button").eq(1).click(function () {
                // 2.设置距离窗口的偏移位
                $('.son').offset({left:10, top:10});
            });
        });
    </script>
```

- *position()*
  - 获取相对于它最近的具有相对位置(position:relative或position:absolute)的父级元素的距离

```
    <script>
        $(function () {
            $("button").eq(0).click(function () {
                // 1.获取匹配元素相对父元素的偏移
                alert($('.son').position().left);// 50
            });
            $("button").eq(1).click(function () {
                // 2.无效,不能设置相对定位元素的偏移位
                $('.son').position({left:10, top:10})
            });
        });
    </script>
```

- *scrollTop([val])*
  - 设置或获取匹配元素相对滚动条顶部的偏移。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>12-jQuery操作位置</title>
    <style>
        *{
            margin: 0;
            padding: 0;
        }
        .scroll{
            margin-top: 100px;
            margin-left: 100px;
            width: 100px;
            height: 200px;
            border: 1px solid #000;
            overflow: auto;
        }
    </style>
    <script src="代码/js/jquery-1.12.4.js"></script>
    <script>
        $(function () {
            $("button").eq(0).click(function () {
                // 7.获取匹配元素相对滚动条顶部的偏移
//                alert($('.scroll').scrollTop());
//                alert($('html').scrollTop());
                // 兼容所有浏览器写法
                alert($('html').scrollTop()+$('body').scrollTop());
            });
            $("button").eq(1).click(function () {
                // 8.设置匹配元素相对滚动条顶部的偏移
//                $('.scroll').scrollTop(100);
//                $('html').scrollTop(100);
                // 兼容所有浏览器写法
                $('html,body').scrollTop(100);
            });
        });
    </script>
</head>
<body>
<div class="scroll">
    我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字
</div>
<button>获取</button>
<button>设置</button>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
</body>
</html>
```

- *scrollLeft([val])*
  - 用上面按钮代码自己写,工作后都得靠自己,多锻炼自学能力(如何查看文档,如何编写测试案例等)

# AJAX

```js
jQuery.ajax([settings])
```

| 参数       | 描述                                                         |
| :--------- | :----------------------------------------------------------- |
| *settings* | 可选。用于配置 Ajax 请求的键值对集合。可以通过 $.ajaxSetup() 设置任何选项的默认值。 |

### 参数

- options

  类型：Object可选。AJAX 请求设置。所有选项都是可选的。

- async

  类型：Boolean默认值: true。默认设置下，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为 false。注意，同步请求将锁住浏览器，用户其它操作必须等待请求完成才可以执行。

- beforeSend(XHR)

  类型：Function发送请求前可修改 XMLHttpRequest 对象的函数，如添加自定义 HTTP 头。XMLHttpRequest 对象是唯一的参数。这是一个 Ajax 事件。如果返回 false 可以取消本次 ajax 请求。

- cache

  类型：Boolean默认值: true，dataType 为 script 和 jsonp 时默认为 false。设置为 false 将不缓存此页面。jQuery 1.2 新功能。

- complete(XHR, TS)

  类型：Function请求完成后回调函数 (请求成功或失败之后均调用)。参数： XMLHttpRequest 对象和一个描述请求类型的字符串。这是一个 Ajax 事件。

- contentType

  类型：String默认值: "application/x-www-form-urlencoded"。发送信息至服务器时内容编码类型。默认值适合大多数情况。如果你明确地传递了一个 content-type 给 $.ajax() 那么它必定会发送给服务器（即使没有数据要发送）。

- context

  类型：Object这个对象用于设置 Ajax 相关回调函数的上下文。也就是说，让回调函数内 this 指向这个对象（如果不设定这个参数，那么 this 就指向调用本次 AJAX 请求时传递的 options 参数）。比如指定一个 DOM 元素作为 context 参数，这样就设置了 success 回调函数的上下文为这个 DOM 元素。就像这样：`$.ajax({ url: "test.html", context: document.body, success: function(){         $(this).addClass("done");       }}); `

- data

  类型：String发送到服务器的数据。将自动转换为请求字符串格式。GET 请求中将附加在 URL 后。查看 processData 选项说明以禁止此自动转换。必须为 Key/Value 格式。如果为数组，jQuery 将自动为不同值对应同一个名称。如 {foo:["bar1", "bar2"]} 转换为 '&foo=bar1&foo=bar2'。

- dataFilter

  类型：Function给 Ajax 返回的原始数据的进行预处理的函数。提供 data 和 type 两个参数：data 是 Ajax 返回的原始数据，type 是调用 jQuery.ajax 时提供的 dataType 参数。函数返回的值将由 jQuery 进一步处理。

- dataType

  类型：String预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断，比如 XML MIME 类型就被识别为 XML。在 1.4 中，JSON 就会生成一个 JavaScript 对象，而 script 则会执行这个脚本。随后服务器端返回的数据会根据这个值解析后，传递给回调函数。可用值:"xml": 返回 XML 文档，可用 jQuery 处理。"html": 返回纯文本 HTML 信息；包含的 script 标签会在插入 dom 时执行。"script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了 "cache" 参数。注意：在远程请求时(不在同一个域下)，所有 POST 请求都将转为 GET 请求。（因为将使用 DOM 的 script标签来加载）"json": 返回 JSON 数据 。"jsonp": JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。"text": 返回纯文本字符串

- error

  类型：Function默认值: 自动判断 (xml 或 html)。请求失败时调用此函数。有以下三个参数：XMLHttpRequest 对象、错误信息、（可选）捕获的异常对象。如果发生了错误，错误信息（第二个参数）除了得到 null 之外，还可能是 "timeout", "error", "notmodified" 和 "parsererror"。这是一个 Ajax 事件。

- global

  类型：Boolean是否触发全局 AJAX 事件。默认值: true。设置为 false 将不会触发全局 AJAX 事件，如 ajaxStart 或 ajaxStop 可用于控制不同的 Ajax 事件。

- ifModified

  类型：Boolean仅在服务器数据改变时获取新数据。默认值: false。使用 HTTP 包 Last-Modified 头信息判断。在 jQuery 1.4 中，它也会检查服务器指定的 'etag' 来确定数据没有被修改过。

- jsonp

  类型：String在一个 jsonp 请求中重写回调函数的名字。这个值用来替代在 "callback=?" 这种 GET 或 POST 请求中 URL 参数里的 "callback" 部分，比如 {jsonp:'onJsonPLoad'} 会导致将 "onJsonPLoad=?" 传给服务器。

- jsonpCallback

  类型：String为 jsonp 请求指定一个回调函数名。这个值将用来取代 jQuery 自动生成的随机函数名。这主要用来让 jQuery 生成度独特的函数名，这样管理请求更容易，也能方便地提供回调函数和错误处理。你也可以在想让浏览器缓存 GET 请求的时候，指定这个回调函数名。

- password

  类型：String用于响应 HTTP 访问认证请求的密码

- processData

  类型：Boolean默认值: true。默认情况下，通过data选项传递进来的数据，如果是一个对象(技术上讲只要不是字符串)，都会处理转化成一个查询字符串，以配合默认内容类型 "application/x-www-form-urlencoded"。如果要发送 DOM 树信息或其它不希望转换的信息，请设置为 false。

- scriptCharset

  类型：String只有当请求时 dataType 为 "jsonp" 或 "script"，并且 type 是 "GET" 才会用于强制修改 charset。通常只在本地和远程的内容编码不同时使用。

- success

  类型：Function请求成功后的回调函数。参数：由服务器返回，并根据 dataType 参数进行处理后的数据；描述状态的字符串。这是一个 Ajax 事件。

- traditional

  类型：Boolean如果你想要用传统的方式来序列化数据，那么就设置为 true。请参考工具分类下面的 jQuery.param 方法。

- timeout

  类型：Number设置请求超时时间（毫秒）。此设置将覆盖全局设置。

- type

  类型：String默认值: "GET")。请求方式 ("POST" 或 "GET")， 默认为 "GET"。注意：其它 HTTP 请求方法，如 PUT 和 DELETE 也可以使用，但仅部分浏览器支持。

- url

  类型：String默认值: 当前页地址。发送请求的地址。

- username

  类型：String用于响应 HTTP 访问认证请求的用户名。

- xhr

  类型：Function需要返回一个 XMLHttpRequest 对象。默认在 IE 下是 ActiveXObject 而其他情况下是 XMLHttpRequest 。用于重写或者提供一个增强的 XMLHttpRequest 对象。这个参数在 jQuery 1.3 以前不可用。

## 回调函数

如果要处理 $.ajax() 得到的数据，则需要使用回调函数：beforeSend、error、dataFilter、success、complete。

### beforeSend

在发送请求之前调用，并且传入一个 XMLHttpRequest 作为参数。

### error

在请求出错时调用。传入 XMLHttpRequest 对象，描述错误类型的字符串以及一个异常对象（如果有的话）

### dataFilter

在请求成功之后调用。传入返回的数据以及 "dataType" 参数的值。并且必须返回新的数据（可能是处理过的）传递给 success 回调函数。

### success

当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。

### complete

当请求完成之后调用这个函数，无论成功或失败。传入 XMLHttpRequest 对象，以及一个包含成功或错误代码的字符串。

# other

## jquery中val()和value区分

1.jQuery中的val()方法**用来获取或设定输入框或选择框的值。**

<input id="text" type="text"></input>

获取值：$("##text").val()

设置值：$("##text").val(“value”)

也可以来获取或设置select标签的值

<select id="select">
  <option>
select1
</option>
  <option>
select2
</option>
</select>

获取值：$("##select").val()

设置值：$("##select").val(“select1”)

2.jQuery中没有value方法，可以通过attr方法获取或设置标签中的value属性的值

<div id="value" value="div'>

获取值：$("##value").attr("value");

设定值：$("##value").attr("value","设定值");
3.在js中可以直接通过对象.属性获取属性值：

```html
<input id="text" type="text"></input>

<script>
var type = document.getElementById("text").type;
alert(type)；
</script>
```

## prop 和attr

 prop  对于HTML元素本身就带有的固有属性   attr  对于HTML元素我们自定义的加在标签中的属性

- 对于HTML元素本身就带有的固有属性，在处理时，使用prop方法。
- 对于HTML元素我们自己自定义的DOM属性，在处理时，使用attr方法。

## this和$(this)

this其实是一个Html 元素。
$this 只是个变量名，加$是为说明其是个jquery对象。
 而$(this)是个转换，将this表示的dom对象转为jquery对象，这样就可以使用jquery提供的方法操作。

## 获得内容 - text()、html() 以及 val()

三个简单实用的用于 DOM 操作的 jQuery 方法：

- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素的内容（包括 HTML 标记）
- val() - 设置或返回表单字段的值

## trigger() 方法触发被选元素的指定事件类型。

#### 触发事件

规定被选元素要触发的事件。

###### 语法

```javascript
$(selector).trigger(event,[param1,param2,...])
```

## jQuery :eq() 选择器

### 定义和用法

:eq() 选择器选取带有指定 index 值的元素。

index 值从 0 开始，所有第一个元素的 index 值是 0（不是 1）。

经常与其他元素/选择器一起使用，来选择指定的组中特定序号的元素（如上面的例子）。

## unbind() 方法移除被选元素的事件处理程序

该方法能够移除所有的或被选的事件处理程序，或者当事件发生时终止指定函数的运行。

ubind() 适用于任何通过 jQuery 附加的事件处理程序。

## js能对同一html的dom对象增加多个click、change事件，注意在不需要时解绑一些函数
