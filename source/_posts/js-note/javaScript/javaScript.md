---
title: javaScript
p: js-note/javaScript/javaScript
date: 2019-11-12 10:18:21  
categories: js
tags: [js,前端]
---
**[github笔记下载地址](https://github.com/codeOflI/codeOflI.github.io/blob/dev/source/_posts/js-note/javaScript/javaScript.md)**
[TOC]

# JavaScript  

JavaScript负责页面中的的行为。  

它是一门运行在浏览器端的脚本语言。  

## JS的编写的位置  

1.可以编写到标签的指定属性中  
```html  
<button onclick="alert('hello');">我是按钮</button>  
<a href="javascript:alert('aaa');">超链接</a>  
```

2.可以编写到script标签中  

```html  
<script type="text/javascript">  
//编写js代码  
</script>  
```
3.可以将代码编写到外部的js文件中，然后通过标签将其引入  

script标签一旦用于引入外部文件了，就不能在编写代码了，即使编写了浏览器也会忽略  ,如果需要则可以在创建一个新的script标签用于编写内部代码  

```html  
<script type="text/javascript" src="文件路径"></script>  
```
## 输出语句  

```javascript  
alert("要输出的内容");  
```
该语句会在浏览器窗口中弹出一个警告框  
```javascript  
document.write("要输出的内容");  
```
该内容将会被写到body标签中，并在页面中显示  
```javascript  
console.log("要输出的内容");  
```
该内容会被写到开发者工具的控制台中  

## 基本的语法  

js函数声明不需要；分号，但是赋值语句要加；分号  

```javascript  
function functionName(arg0,arg1,arg2){  
//函数声明  
}  
var functionName=function(arg0,arg1,arg2){  
//函数表达式  
};(注意分号)  
```

注释  

单行注释  

```javascript  
//注释内容  
```
多行注释  
```javascript  
/*  
注释内容  
*/  
```

JS严格区分大小写	  

JS中每条语句以分号(;)结尾如果不写分号，浏览器会自动添加，但是会消耗一些系统资源，  而且有些时候，浏览器会加错分号，所以在开发中分号必须写  

JS中会自动忽略多个空格和换行，所以我们可以利用空格和换行对代码进行格式化。  

## 字面量和变量  

### 字面量  

字面量实际上就是一些固定的值，比如 1 2 3 4 true false null NaN "hello"  
**字面量都是不可以改变的。**  

由于字面量不是很方便使用，所以在JS中很少直接使用字面量  

### 变量  

变量可以用来保存字面量，并且可以保存任意的字面量  

一般都是通过变量来使用字面量，而不直接使用字面量，而且也可以通过变量来对字面量进行一个描述  

声明变量  

使用var关键字来声明一个变量  
```javascript
var a;  
```

为变量赋值 
```javascript
a = 1; 
```
声明和赋值同时进行 
```javascript
var a = 456;   
```
### 标识符  

在JS中所有的可以自主命名的内容，都可以认为是一个标识符，  
是标识符就应该遵守标识符的规范。  

比如：变量名、函数名、属性名  

规范：  
1.标识符中可以含有字母、数字、_、$  
2.标识符不能以数字开头  
3.标识符不能是JS中的关键字和保留字  
4.标识符一般采用驼峰命名法  
xxxYyyZzz  

# 数据类型  

## 六种数据类型  

 **JS中一共分成六种数据类型 5个基本数据类型+object**  
 String 字符串  
 Number 数值  
 Boolean 布尔值  
 Null 空值  
 Undefined 未定义  
 Object 对象  

 其中基本数据类型有	5个  

**typeof运算符检查数据类型**  

### 1.String 字符串  
JS中的字符串需要使用引号引起来双引号或单引号都行  
 在字符串中使用\作为转义字符  

```javascript  
\'  ==> '  
\"  ==> "  
\n  ==> 换行  
\t  ==> 制表符  
\\  ==> \	  
```

 使用typeof运算符检查字符串时，会返回"string"	  

### 2.Number 数值  
 **JS中所有的整数和浮点数都是Number类型**  

最大能表示的值：Number.MAX_VALUE=	1.7976931348623157e+308  

 特殊的数字：能赋值给变量  
	Infinity 正无穷 a = Infinity ,能赋值  
	-Infinity 负无穷  
	NaN 非法数字（Not A Number）  
 其他进制的数字的表示：  
0b 开头表示二进制，但是不是所有的浏览器都支持  
0 开头表示八进制  
0x 开头表示十六进制  

 使用typeof检查一个Number类型的数据时，会返回"number"  
（包括NaN 和 Infinity）  

### 3.Boolean 布尔值  
 布尔值主要用来进行逻辑判断，布尔值只有两个  
 true 逻辑的真  
 false 逻辑的假  
 使用typeof检查一个布尔值时，会返回"boolean"	  

### 4.Null 空值  
 空值专门用来表示为空的对象，Null类型的值只有一个  
 null  
 使用typeof检查一个Null类型的值时会返回"object"  

###  5.Undefined 未定义  
 **如果声明一个变量但是没有为变量赋值此时变量的值就是undefined**  
 该类型的值只有一个 undefined  
 使用typeof检查一个Undefined类型的值时，会返回"undefined"  

### 引用数据类型	  
 Object 对象  

## 类型转换  

 类型转换就是指将其他的数据类型，转换为String Number 或 Boolean  

### 转换为String  

 #### 方式一（强制类型转换）：  
 **调用被转换数据的toString()方法**  
 例子：  
var a = 123;  
a = a.toString();  
 注意：**这个方法不适用于null和undefined**  
由于这两个类型的数据中没有方法，所以调用toString()时会报错  

 #### 方式二（强制类型转换）：  
 **调用String()函数**  
 例子：  
```javascript
var a = 123;  
a = String(a);  
```
 原理：**对于Number Boolean String都会调用他们的toString()方法来将其转换为字符串，对于null值，直接转换为字符串"null"。对于undefined直接转换为字符串"undefined"**  

 #### 方式三（隐式的类型转换）:   
 **为任意的数据类型 +""**  
 例子：  
```javascript
var a = true;  
a = a + ""; 
```
 原理：和String()函数一样	  

### 转换为Number  

#### 方式一（强制类型转换）：  
 **调用Number()函数**  
 例子： 
```javascript
var s = "123";  
s = Number(s); 
```
 转换的情况：  
1. 字符串 > 数字    
	 如果字符串是一个合法的数字，则直接转换为对应的数字  
	 如果字符串是一个非法的数字，则转换为NaN  
	 如果是一个空串或纯空格的字符串，则转换为0  
2. 布尔值 > 数字     
	 true转换为1  
	 false转换为0  
3. 空值 > 数字     
	 null转换为0  
4. 未定义 > 数字    
	 undefined 转换为NaN  

#### 方式二（强制类型转换）：  
 调用parseInt()或parseFloat()  
 这两个函数专门用来将一个字符串转换为数字的  

如果对非String使用parseInt()或parseFloat()，它会**先将其转换为String**然后在操作 parseInt()  
 可以将**一个字符串中的有效的整数位**提取出来，并转换为Number    
 例子：  
 ```javascript  
var a = "123.456px";  
a = parseInt(a); //123  
 ```
 如果需要可以在parseInt()中指定一个第二个参数，来指定进制parseFloat()可以将一个**字符串中的有效的小数位**提取出来，并转换为Number    
 例子：  
```javascript  
var a = "123.456px";  
a = parseFloat(a); //123.456  
```
#### 方式三（隐式的类型转换）：  
 使用一元的+来进行隐式的类型转换    
 例子：  
```javascript  
var a = "123";  
a = +a;  
```

 **原理：和Number()函数一样**  
### 转换为布尔值  

#### 方式一（强制类型转换）：  
 使用Boolean()函数  
 例子：  
```javascript
var s = "false";  
s = Boolean(s); //true 
```

 转换的情况  
字符串 > 布尔  
	 除了空串其余全是true  
	  
数值 > 布尔  
	 除了0和NaN其余的全是true  
	  
null、undefined > 布尔  
	 都是false  
	  
对象 > 布尔  
	 都是true  

#### 方式二（隐式类型转换）：	  
 **为任意的数据类型做两次非运算，即可将其转换为布尔值**    
 例子：  
```javascript  
var a = "hello";  
a = !!a; //true  
```

# 基础语法  

## 运算符  
 运算符也称为操作符  
 通过运算符可以对一个或多个值进行运算或操作  
### typeof运算符  
 用来检查一个变量的数据类型  
 语法：typeof 变量  
 它会返回一个用于描述类型的字符串作为结果  
### 算数运算符  

+&ensp;对两个值进行加法运算并返回结果    
-&ensp;对两个值进行减法运算并返回结果    
*&ensp;对两个值进行乘法运算并返回结果    
/&ensp;对两个值进行除法运算并返回结果    
%&ensp;对两个值进行取余运算并返回结果  

 **除了加法以外，对非Number类型的值进行运算时，都会先转换为Number然后在做运算。**  
 而做加法运算时，如果是两个字符串进行相加，则会做拼串操作，将两个字符连接为一个字符串。  
 任何值和字符串做加法，都会先转换为字符串，然后再拼串  

### 一元运算符  

 一元运算符只需要一个操作数  
 #### 一元的+  
 就是正号，不会对值产生任何影响，但是可以将一个非数字转换为数字    
 例子：    
```javascript  
var a = true;  
a = +a;  
```
#### 一元的-  
 就是负号，可以对一个数字进行符号位取反  
 例子：  
```javascript  
var a = 10;  
a = a;  
```

#### 自增  
 自增可以使变量在原值的基础上自增1  
 自增使用 ++  
 自增可以使用 前++（++a）后++(a++)  
 无论是++a 还是 a++都会立即使原变量自增1  
不同的是++a和a++的值是不同的，  
	++a的值是变量的新值（自增后的值）  
	a++的值是变量的原值（自增前的值）  

#### 自减  
 自减可以使变量在原值的基础上自减1  
 自减使用   
 自减可以使用 前（a）后(a)  
 无论是a 还是 a都会立即使原变量自减1  
不同的是a和a的值是不同的，  
	a的值是变量的新值（自减后的值）  
	a的值是变量的原值（自减前的值）  

### 逻辑运算符  

!    
	 非运算可以对一个布尔值进行取反，true变false false边true  
	 当对非布尔值使用!时，会先将其转换为布尔值然后再取反  
	 我们可以利用!来将其他的数据类型转换为布尔值  

&&    
	 &&可以对符号两侧的值进行与运算  
	 只有两端的值都为true时，才会返回true。只要有一个false就会返回false。  
	 与是一个短路的与，如果第一个值是false，则不再检查第二个值  
	 对于非布尔值，它会将其转换为布尔值然后做运算，并返回原值  
	 规则：  
			1.如果第一个值为false，则返回第一个值  
			2.如果第一个值为true，则返回第二个值  

||    
	 ||可以对符号两侧的值进行或运算  
	 只有两端都是false时，才会返回false。只要有一个true，就会返回true。  
	 或是一个短路的或，如果第一个值是true，则不再检查第二个值  
	 对于非布尔值，它会将其转换为布尔值然后做运算，并返回原值  
	 规则：	  
			1.如果第一个值为true，则返回第一个值  
			2.如果第一个值为false，则返回第二个值  
### 赋值运算符  

=    
	 可以将符号右侧的值赋值给左侧变量    
+=      
```javascript
a += 5 相当于 a = a+5    
var str = "hello";  str += "world";  
```
-=    
```javascript
a -= 5  相当于 a = a-5  
```
*=    
```javascript  
a *= 5 相当于 a = a*5  
```
/=    
```javascript  
a /= 5 相当于 a = a/5	  
```


%=  
```javascript  
a %= 5 相当于 a = a%5 
```

### 关系运算符  

 关系运算符用来比较两个值之间的大小关系的  
	>  
	>=  
	<  
	<=  
 关系运算符的规则和数学中一致，用来比较两个值之间的关系，  
	如果关系成立则返回true，关系不成立则返回false。  
 如果比较的两个值是非数值，会将其转换为Number然后再比较。  
 如果比较的两个值都是字符串，此时会比较字符串的Unicode编码，而不会转换为Number。  

### 相等运算符  

  

 相等，判断左右两个值是否相等，如果相等返回true，如果不等返回false  
 相等会自动对两个值进行类型转换，如果**对不同的类型进行比较，会将其转换为相同的类型然后再比较**，转换后相等它也会返回true，null == undifined  

!=  
	 不等，判断左右两个值是否不等，如果不等则返回true，如果相等则返回false  
	 不等也会做自动的类型转换。  
	  
**===**  
	 **全等**，判断左右两个值是否全等，它和相等类似，只不过它不会进行自动的类型转换，  
		如果两个值的类型不同，则直接返回false  
		  
!==  
	 **不全等**，和不等类似，但是它不会进行自动的类型转换，如果两个值的类型不同，它会直接返回true  
	  
特殊的值：  
	 null和undefined  
		 由于undefined衍生自null，所以**null == undefined** 会返回true。  
			但是 null === undefined 会返回false。  
**NaN**  
	 NaN不与任何值相等，报告它自身 NaN == NaN //false  
	  
 判断一个值是否是NaN  
	 使用isNaN()函数  

### 三元运算符：  

?:  
	 语法：条件表达式?语句1:语句2;  
	 执行流程：  
		先对条件表达式求值判断，  
			如果判断结果为true，则执行语句1，并返回执行结果  
			如果判断结果为false，则执行语句2，并返回执行结果  
			  
优先级：  
 和数学中一样，JS中的运算符也是具有优先级的，  
	比如 先乘除 后加减 先与 后或  
 具体的优先级可以参考优先级的表格，在表格中越靠上的优先级越高，  
	优先级越高的越优先计算，优先级相同的，从左往右计算。  
 优先级不需要记忆，如果越到拿不准的，使用()来改变优先级。  

## 流程控制语句  

 程序都是自上向下的顺序执行的，  
通过流程控制语句可以改变程序执行的顺序，或者反复的执行某一段的程序。  

### 条件分支语句  
 条件判断语句也称为if语句  
 语法一：  
 ```javascript
 if(条件表达式){  
 	语句...  
 }  
 ```


```  
 执行流程：  
 if语句执行时，会先对条件表达式进行求值判断，  
 如果值为true，则执行if后的语句  
 如果值为false，则不执行  
```

 语法二：  
```javascript
if(条件表达式){  
	语句...  
}else{  
	语句...  
} 
```
```  
 执行流程：  
	if...else语句执行时，会对条件表达式进行求值判断，  
		如果值为true，则执行if后的语句  
		如果值为false，则执行else后的语句  
```

 语法三：  

```javascript  
if(条件表达式){  
	语句...  
}else if(条件表达式){  
	语句...  
}else if(条件表达式){  
	语句...  
}else if(条件表达式){  
	语句...  
}else{  
	语句...  
}	  
```
```  
 执行流程  
	 if...else if...else语句执行时，会自上至下依次对条件表达式进行求值判断，  
		如果判断结果为true，则执行当前if后的语句，执行完成后语句结束。  
		如果判断结果为false，则继续向下判断，直到找到为true的为止。  
		如果所有的条件表达式都是false，则执行else后的语句  
```

1.条件分支语句  
switch语句  
语法:  

```javascript  
switch(条件表达式){  
	case 表达式:  
		语句...  
		break;  
	case 表达式:  
		语句...  
		break;  
	case 表达式:  
		语句...  
		break;  
	default:  
		语句...  
		break;  
}  
```

执行流程：  
 switch...case...语句在执行时，会依次将case后的表达式的值和switch后的表达式的值进行全等比较，  
	如果比较结果为false，则继续向下比较。如果比较结果为true，则从当前case处开始向下执行代码。  
	如果所有的case判断结果都为false，则从default处开始执行代码。  
### 循环语句  
通过循环语句可以反复执行某些语句多次  
while循环  
 语法：  
```javascript  
while(条件表达式){  
    语句...  
}  
```

 执行流程：  
	while语句在执行时，会先对条件表达式进行求值判断，  
		如果判断结果为false，则终止循环  
		如果判断结果为true，则执行循环体  
		循环体执行完毕，继续对条件表达式进行求值判断，依此类推  
		  
do...while循环  
 语法:  


```javascript  
do{  
语句...  
}while(条件表达式)  
```


 执行流程  
	do...while在执行时，会先执行do后的循环体，然后在对条件表达式进行判断，  
		如果判断判断结果为false，则终止循环。  
		如果判断结果为true，则继续执行循环体，依此类推  
		  
 和while的区别：  
	while：先判断后执行  
	do...while: 先执行后判断  
	 do...while可以确保循环体至少执行一次。  
	  
for循环  
 语法：  
```javascript
for(①初始化表达式 ; ②条件表达式 ; ④更新表达式){  
    ③语句...  
}  
```
 执行流程：  
	首先执行①初始化表达式，初始化一个变量，  
	然后对②条件表达式进行求值判断，如果为false则终止循环  
	如果判断结果为true，则执行③循环体  
	循环体执行完毕，执行④更新表达式，对变量进行更新。  
	更新表达式执行完毕重复②  
	  
死循环  
```javascript
while(true){  

}  

for(;;){  

}
```
# 对象（Object）  

对象是JS中的引用数据类型  
**对象是一种复合数据类型，在对象中可以保存多个不同数据类型的属性**  
使用typeof检查一个对象时，会返回object  

## 对象的分类：  

1.内建对象  
	- 由ES标准中定义的对象，在任何的ES的实现中都可以使用  
	- 比如：Math String Number Boolean Function Object....  

2.宿主对象  
	- 由JS的运行环境提供的对象，目前来讲主要指由浏览器提供的对象  
	- 比如 BOM DOM  

3.自定义对象  

	- 由开发人员自己创建的对象  

创建对象  
 方式一：
```javascript
	 var obj = new Object();  
```
 方式二： 
 ```javascript
	 var obj = {}; 
 ```
**向对象中添加属性**  
 语法：  
	对象.属性名 = 属性值;  
	**对象["属性名"] = 属性值;**	//这种方式能够使用特殊的属性名  

 **对象的属性名没有任何要求，不需要遵守标识符的规范，但是在开发中，尽量按照标识符的要求去写。**  
属性值也可以任意的数据类型。  

读取对象中的属性  
 语法：  
	对象.属性名  
	对象["属性名"] //"属性名"可以使字符串常量，也可以是字符串变量  
 如果读取一个对象中没有的属性，它不会报错，而是返回一个undefined  

**删除对象中的属性**  
 语法：  
```javascript
delete 对象.属性名  
delete 对象["属性名"]  
```


## 遍历  

**使用in检查对象中是否含有指定属性**  
 语法："属性名" in 对象  
	 如果在对象中含有该属性，则返回true  
		如果没有则返回false  

      循环遍历对象自身的和继承的可枚举属性(不含Symbol属性).  

```javascript  
var obj = {'0':'a','1':'b','2':'c'};  
  
for(var i in obj) {  
     console.log(i,":",obj[i]);  
}  
```


​	  
**使用对象字面量，在创建对象时直接向对象中添加属性**  
语法： 
```javascript
var obj = {  
    属性名:属性值,  
    属性名:属性值,  
    属性名:属性值,  
    属性名:属性值  
} 
```
基本数据类型和引用数据类型  
 基本数据类型  
	String Number Boolean Null Undefined  
 引用数据类型  
	Object  
 **基本数据类型的数据，变量是直接保存的它的值。**  
	变量与变量之间是互相独立的，修改一个变量不会影响其他的变量。  
 **引用数据类型的数据，变量是保存的对象的引用（内存地址）。**  
	如果多个变量指向的是同一个对象，此时修改一个变量的属性，会影响其他的变量。  
 比较两个变量时，对于基本数据类型，比较的就是值，  
	对于引用数据类型比较的是地址，地址相同才相同  	  

## 函数（Function）	  

**函数也是一个对象，也具有普通对象的功能（能有属性）**  
函数中可以封装一些代码，在需要的时候可以去调用函数来执行这些代码  
使用typeof检查一个函数时会返回function  
创建函数  
 函数声明 
```javascript
function 函数名([形参1,形参2...形参N]){  
语句...  
}  
```
 函数表达式
```javascript
var 函数名 = function([形参1,形参2...形参N]){  
语句...  
};  
```
调用函数  
 语法：函数对象([实参1,实参2...实参N]);  
	fun() sum() alert() Number() parseInt()  
 当我们调用函数时，函数中封装的代码会按照编写的顺序执行  

**立即执行函数**  
函数定义完，立即被调用，这种函数叫做立即执行函数  
立即执行函数往往只会执行一次
```javascript
(function(a,b){  
    console.log("a = "+a);  
    console.log("b = "+b);  
})(123,456); 
```
遍历对象  

```javascript  
for(var v in obj){  
    document.write("property：name ="+v+"value="+obj[v]+"<br/>" );  
}  
```

形参和实参  
 形参：形式参数  
	 定义函数时，可以在()中定义一个或多个形参，形参之间使用,隔开  
		定义形参就相当于在函数内声明了对应的变量但是并不赋值，  
		形参会在调用时才赋值。  
		  
 实参：实际参数  
	 调用函数时，可以在()传递实参，传递的实参会赋值给对应的形参,  
		调用函数时JS解析器不会检查实参的类型和个数，可以传递任意数据类型的值。  
		**如果实参的数量大于形参，多余实参将不会赋值，**  
		**如果实参的数量小于形参，则没有对应实参的形参将会赋值undefined**  

**返回值，就是函数执行的结果。**  
 使用return 来设置函数的返回值。  
 语法：return 值;  
	 该值就会成为函数的返回值，可以通过一个变量来接收返回值  
 return后边的代码都不会执行，一旦执行到return语句时，函数将会立刻退出。  
 return后可以跟任意类型的值，可以是基本数据类型，也可以是一个对象。  
 **如果return后不跟值，或者是不写return则函数默认返回undefined。**  
 break、continue和return  
 break  
 	退出循环  
 continue  
 	跳过当次循环  
 return  
 	退出函数  
		  
**参数，函数的实参也可以是任意的数据类型。**  

**方法（method）**  
 可以将一个函数设置为一个对象的属性，  
	当一个对象的属性是一个函数时，  
		我们称这个函数是该对象的方法。  
 对象.方法名();  
 函数名()  

### 函数的属性和方法  

call()  
apply()  
 **这两个方法都是函数对象的方法需要通过函数对象来调用**  
 通过两个方法可以直接调用函数，并且**可以通过第一个实参来指定函数中this**  
 不同的是call是直接传递函数的实参而apply需要将实参封装到一个数组中传递  
**arguments**  
 arguments和this类似，都是函数中的隐含的参数  
 arguments是一个类数组元素，它用来封装函数执行过程中的实参  
	所以即使不定义形参，也可以通过arguments来使用实参  
 **arguments中有一个属性callee表示当前执行的函数对象**  

this（调用函数的那个对象）  
 this是函数的上下文对象，根据函数的调用方式不同会执向不同的对象  
	1.以函数的形式调用时，this是window  
	2.以方法的形式调用时，this是调用方法的对象  
	3.以构造函数的形式调用时，this是新建的那个对象  
	4.使用call和apply调用时，this是指定的那个对象  
	5.在全局作用域中this代表window  

## 作用域  

作用域简单来说就是一个变量的作用范围。  
在JS中作用域分成两种：  

1.全局作用域  

直接在script标签中编写的代码都运行在全局作用域中  
**全局作用域在打开页面时创建，在页面关闭时销毁。**  
全局作用域中有一个全局对象window，window对象由浏览器提供，  
可以在页面中直接使用，它代表的是整个的浏览器的窗口。  
**在全局作用域中创建的变量都会作为window对象的属性保存**  
在全局作用域中创建的函数都会作为window对象的方法保存  
在全局作用域中创建的变量和函数可以在页面的任意位置访问。  
在函数作用域中也可以访问到全局作用域的变量。  
尽量不要在全局中创建变量	  

2.函数作用域  

函数作用域是函数执行时创建的作用域，每次调用函数都会创建一个新的函数作用域。  
函数作用域在函数执行时创建，在函数执行结束时销毁。  
在函数作用域中创建的变量，不能在全局中访问。  
当在函数作用域中使用一个变量时，它会先在自身作用域中寻找，  
如果找到了则直接使用，如果没有找到则到上一级作用域中寻找，  
如果找到了则使用，找不到则继续向上找，一直会  

 **变量的声明提前**  
	 在全局作用域中，使用**var关键字声明的变量会在所有的代码执行之前被声明，但是不会赋值。**  
		所以我们可以在变量声明前使用变量。但是不使用var关键字声明的变量不会被声明提前。  
	 在函数作用域中，也具有该特性，使用var关键字声明的变量会在函数所有的代码执行前被声明，  
		如果没有使用var关键字声明变量，则变量会变成全局变量  
		  
 **函数的声明提前**  
	 在全局作用域中，使用**函数声明创建的函数（function fun(){}）,会在所有的代码执行之前被创建**，  
		也就是我们可以在函数声明前去调用函数，但是使用函数表达式(var fun = function(){})创建的函数没有该特性  
	 在函数作用域中，使用函数声明创建的函数，会在所有的函数中的代码执行之前就被创建好了。  

## this（上下文对象）  

我们每次调用函数时，解析器都会将一个上下文对象作为隐含的参数传递进函数。  
使用this来引用上下文对象，根据函数的调用形式不同，this的值也不同。  

指向当前对象  

this的不同的情况：  
1.以函数的形式调用时，this是window  
2.以方法的形式调用时，this就是调用方法的对象  
3.以构造函数的形式调用时，this就是新创建的对象  

## 4.构造函数  

构造函数是专门用来创建对象的函数  
**一个构造函数我们也可以称为一个类**  
通过一个构造函数创建的对象，我们称该对象时这个构造函数的实例  
通过同一个构造函数创建的对象，我们称为一类对象  
构造函数就是一个普通的函数，只是他的调用方式不同，  
如果直接调用，它就是一个普通函数  
如果使用new来调用，则它就是一个构造函数  

例子：  

```javascript  
function Person(name , age , gender){  
    this.name = name;  
    this.age = age;  
    this.gender = gender;  
    this.sayName = function(){  
        alert(this.name);  
    };  
}  
```

  

构造函数的执行流程：  
1.创建一个新的对象  
2.将新的对象作为函数的上下文对象（this）  
3.执行函数中的代码  
4.将新建的对象返回  

**instanceof 用来检查一个对象是否是一个类的实例**  
 语法：对象 instanceof 构造函数  
	 如果该对象时构造函数的实例，则返回true，否则返回false  
	 **Object是所有对象的祖先，所以任何对象和Object做instanceof都会返回true**  
	  
枚举对象中的属性  
for...in  
语法：  	  

```javascript  
for(var 属性名 in 对象){  
  
}  
```

for...in语句的循环体会执行多次，对象中有几个属性就会执行几次，  
	每次讲一个属性名赋值给我们定义的变量，我们可以通过它来获取对象中的属性  	  

## 原型（prototype）  

 创建一个函数以后，**解析器都会默认在函数中添加一个数prototype**  
	prototype属性指向的是一个对象，这个对象我们称为原型对象。  
 当函数作为构造函数使用，**它所创建的对象中都会有一个隐含的属性执行该原型对象。**  

```javascript  
这个隐含的属性可以通过对象.__proto__来访问。  
```

**原型对象就相当于一个公共的区域，凡是通过同一个构造函数创建的对象他们通常都可以访问到相同的原型对象。**  
	我们可以将对象中共有的属性和方法统一添加到原型对象中，  
		这样我们只需要添加一次，就可以使所有的对象都可以使用。  
 当我们去访问对象的一个属性或调用对象的一个方法时，它会先自身中寻找，  
	如果在自身中找到了，则直接使用。  
	如果没有找到，则去原型对象中寻找，如果找到了则使用，  
	**如果没有找到，则去原型的原型中寻找，**依此类推。直到找到Object的原型为止，Object的原型的原型为null，  
	如果依然没有找到则返回undefined  
 **hasOwnProperty()**  
	 这个方法可以用来检查**对象自身中**是否含有某个属性  
	 语法：对象.hasOwnProperty("属性名")  

## toString方法  

当我们直接在页面中打印一个对象时，事件上是输出的对象的toString()方法的返回值  

如果我们希望在输出对象时不输出[object Object]，可以为对象添加一个toString()方法	  

```javascript  
//修改Person原型的toString  
Person.prototype.toString = function(){  
	return "Person[name="+this.name+",age="+this.age+",gender="+this.gender+"]";  
};  
```

  

## 垃圾回收（GC）  

就像人生活的时间长了会产生垃圾一样，程序运行过程中也会产生垃圾  
这些垃圾积攒过多以后，会导致程序运行的速度过慢，  
所以我们需要一个垃圾回收的机制，来处理程序运行过程中产生垃圾  
当一个对象没有任何的变量或属性对它进行引用，此时我们将永远无法操作该对象，  
此时这种对象就是一个垃圾，这种对象过多会占用大量的内存空间，导致程序运行变慢，  
所以这种垃圾必须进行清理。  
在JS中拥有自动的垃圾回收机制，会自动将这些垃圾对象从内存中销毁，  
我们不需要也不能进行垃圾回收的操作  
我们需要做的只是要将不再使用的对象设置null即可  

# 数组（Array）  

 数组也是一个对象，是一个用来存储数据的对象和Object类似，但是它的存储效率比普通对象要高  
 数组中保存的内容我们称为元素	  
 数组使用索引（index）来操作元素  
 索引指由0开始的整数  

## 数组的操作：  

 创建数组  

```javascript  
var arr = new Array();  
var arr = [];  
```

向数组中添加元素  
语法；  
数组对象[索引] = 值;  

```javascript  
arr[0] = 123;  
arr[1] = "hello";  
```

 创建数组时直接添加元素  
 语法：  
```javascript
 var arr = [元素1,元素2....元素N]; 
```
 例子：
```javascript
 var arr = [123,"hello",true,null];  
```

 获取和修改数组的长度  
	 使用length属性来操作数组的长度  
	 获取长度：  
		数组.length  
		 length获取到的是数组的最大索引+1  
		 对于连续的数组，length获取到的就是数组中元素的个数  
	 修改数组的长度  
		数组.length = 新长度  
			 如果修改后的length大于原长度，则多出的部分会空出来  
			 如果修改后的length小于原长度，则原数组中多出的元素会被删除  
	 向数组的最后添加元素  
		数组[数组.length] = 值;  
		  

## 数组的方法  

| functionName | function                                                     | usage                                   |
| ------------ | ------------------------------------------------------------ | --------------------------------------- |
| push()       | 用来向数组的末尾添加一个或多个元素，并返回数组新的长度       | 语法：数组.push(元素1,元素2,元素N)pop() |
| pop()        | 用来删除数组的最后一个元素，并返回被删除的元素               |                                         |
| unshift()    | 向数组的开头添加一个或多个元素，并返回数组的新的长度         |                                         |
| shift()      | 删除数组的开头的一个元素，并返回被删除的元素                 |                                         |
| reverse()    | 可以用来反转一个数组，它会对原数组产生影响                   |                                         |
| concat()     | 可以连接两个或多个数组，它不会影响原数组，而是新数组作为返回值返回 |                                         |
|              |                                                              |                                         |
|              |                                                              |                                         |
|              |                                                              |                                         |

### slice(sart,[end])  
	 可以从一个数组中截取指定的元素  
	 该方法不会影响原数组，而是将截取到的内容封装为一个新的数组并返回  
	 参数：  
		1.截取开始位置的索引（包括开始位置）  
		2.截取结束位置的索引（不包括结束位置）  
			 第二个参数可以省略不写，如果不写则一直截取到最后  
		 参数可以传递一个负值，如果是负值，则从后往前数  
###  splice()  
	 可以用来删除数组中指定元素，并使用新的元素替换  
		该方法会将删除的元素封装到新数组中返回  
	 参数：  
		1.删除开始位置的索引  
		2.删除的个数  
		3.三个以后，都是替换的元素，这些元素将会插入到开始位置索引的前边  
### join([splitor])  
 可以将一个数组转换为一个字符串  
 参数：  
	需要一个字符串作为参数，这个字符串将会作为连接符来连接数组中的元素  
	如果不指定连接符则默认使用,  
### sort()  
 可以对一个数组中的内容进行排序，默认是按照Unicode编码进行排序  
	调用以后，会直接修改原数组。  
 可以自己指定排序的规则，需要一个回调函数作为参数：  

我们可以自己来指定排序的规则  
我们可以在sort()添加一个回调函数，来指定排序规则，  
	回调函数中需要定义两个形参,  
	浏览器将会分别使用数组中的元素作为实参去调用回调函数  
	使用哪个元素调用不确定，但是肯定的是在数组中a一定在b前边  

- 浏览器会根据回调函数的返回值来决定元素的顺序，  
	如果返回一个大于0的值，则元素会交换位置  
	如果返回一个小于0的值，则元素位置不变  
	如果返回一个0，则认为两个元素相等，也不交换位置  
  
- 如果需要升序排列，则返回 a-b  
	如果需要降序排列，则返回b-a  
  
```  javascript
function(a,b){  
	//升序排列  
	//return a-b;  
	  
	//降序排列  
	return b-a;  
}  
```

## 遍历数组  

 遍历数组就是将数组中元素都获取到  
 一般情况我们都是使用for循环来遍历数组
```javascript
for(var i=0 ; i<数组.length ; i++){  
    //数组[i]  
}  
```


 使用forEach()方法来遍历数组（不兼容IE8）  

```javascript
数组.forEach(function(value , index , obj){  
  
});  
```

forEach()方法需要一个回调函数作为参数，  
数组中有几个元素，回调函数就会被调用几次，  
每次调用时，都会将遍历到的信息以实参的形式传递进来，  
我们可以定义形参来获取这些信息。  
value:正在遍历的元素  
index:正在遍历元素的索引  
obj:被遍历对象    

# 常用类和方法  

## 包装类  

在JS中为我们提供了**三个包装类：**  
String() Boolean() Number()  
 通过这三个包装类可以创建基本数据类型的对象  
例子：
```javascript
var num = new Number(2);  
var str = new String("hello");  
var bool = new Boolean(true); 
```
但是在实际应用中千万不要这么干。  

当我们去操作一个基本数据类型的属性和方法时，  
**解析器会临时将其转换为对应的包装类，然后再去操作属性和方法，**  
操作完成以后再将这个临时对象进行销毁。  

## Date  

日期的对象，在JS中通过Date对象来表示一个时间  
创建对象  
 创建一个当前的时间对象  
```javascript
 var d = new Date();  
```

 创建一个指定的时间对象  
```javascript
	var d = new Date("月/日/年 时:分:秒");  
```

### 方法：  

| name              |                                                              |
| ----------------- | ------------------------------------------------------------ |
| getDate()         | 当前日期对象是几日（1-31）                                   |
| getDay()          | 返回当前日期对象时周几（0-6）<br/>		 0 周日<br/>		 1 周一 。。。 |
| getMonth()        | 返回当前日期对象的月份（0-11）<br/>	 0 一月 1 二月 。。。 |
| getFullYear()     | 从 Date 对象以四位数字返回年份。                             |
| getHours()        | 返回 Date 对象的小时 (0 ~ 23)。                              |
| getMinutes()      | 返回 Date 对象的分钟 (0 ~ 59)。                              |
| getSeconds()      | 返回 Date 对象的秒数 (0 ~ 59)。                              |
| getMilliseconds() | 返回 Date 对象的毫秒(0 ~ 999)。                              |
| getTime()         | 返回当前日期对象的时间戳<br/>	 时间戳，指的是从1970年月1日 0时0分0秒，**到现在时间的毫秒数**<br/>		计算机底层保存时间都是以时间戳的形式保存的。 |
| Date.now()        | 可以获取当前代码执行时的时间戳                               |
| setHours()        | 设置 Date 对象中的小时 (0 ~ 23)                              |

## Math			  

Math属于一个工具类，它不需要我们创建对象，它里边封装了属性运算相关的常量和方法  
我们可以直接使用它来进行数学运算相关的操作  
方法：  
Math.PI  
	 常量，圆周率  
Math.abs()  
	 绝对值运算  
Math.ceil()  
	 向上取整  
Math.floor()  
	 向下取整  
Math.round()  
	 四舍五入取整  
Math.random()	  
	 生成一个01之间的随机数  
	 生成一个xy之间的随机数  
		Math.round(Math.random()*(y-x)+x);  
Math.pow(x,y)  
	 求x的y次幂  
Math.sqrt()  
	 对一个数进行开方  
Math.max()  
	 求多个数中最大值  
Math.min()  
	 求多个数中的最小值  

## 字符串的相关的方法  

使用ES6中的字符串新方法  

 **String.prototype.padStart(maxLength, fillString='')** 或 **String.prototype.padEnd(maxLength, fillString='')**来填充字符串；  

length  
 获取字符串的长度  
charAt()  
 根据索引获取指定的字符  
charCodeAt()  
 根据索引获取指定的字符编码  
**String.fromCharCode()**  
 **根据字符编码获取字符**  
indexOf()  
lastIndexOf()  
 从一个字符串中检索指定内容  
 需要一个字符串作为参数，这个字符串就是要检索的内容，  
	如果找到该内容，则会返回其第一次出现的索引，如果没有找到则返回-1。  
 可以指定一个第二个参数，来表示开始查找的位置  
 indexOf()是从前向后找  
 lastIndexOf()是从后向前找  
slice(start,[end])  
 可以从一个字符串中截取指定的内容，并将截取到内容返回，不会影响原变量  
 参数：  
	第一个：截取开始的位置（包括开始）  
	第二个：截取结束的位置**（不包括结束）**  
		 可以省略第二个参数，如果省略则一直截取到最后  
	 可以传负数，如果是负数则从后往前数  
substr()	  
 和slice()基本一致，不同的是它第二个参数不是索引，而是截取的数量  

substring()  
 和slice()基本一致，不同的是它不能接受负值作为参数，如果设置一个负值，则会自动修正为0，  
	**substring()中如果第二个参数小于第一个，自动调整位置**  
toLowerCase()   
 将字符串转换为小写并返回  
toUpperCase()   
 将字符串转换为大写并返回  

### 正则表达相关方法  

**split()**  
 可以根据指定内容将一个字符串拆分为一个数组  
 参数：  
	 需要一个字符串作为参数，将会根据字符串去拆分数组  
		可以接收一个正则表达式，此时会根据正则表达式去拆分数组  
		  
**match()**  
	 可以根据正则表达式，从一个字符串中将符合条件的内容提取出来  
	 默认情况下我们的match只会找到第一个符合要求的内容，找到以后就停止检索  
		我们可以设置正则表达式为全局匹配模式，这样就会匹配到所有的内容  
		可以为一个正则表达式设置多个匹配模式，且顺序无所谓  
	 match()会将匹配到的内容封装到一个数组中返回，即使只查询到一个结果  

**replace()**  
	 可以将字符串中指定内容替换为新的内容  
	参数：  
		1.被替换的内容，可以接受一个正则表达式作为参数  
		**2.新的内容**  空串则为删除""  
	默认只会替换第一个  

search()  
	 可以搜索字符串中是否含有指定内容  
	 如果搜索到指定内容，则会返回第一次出现的索引，如果没有搜索到返回1  
	 它可以接受一个正则表达式作为参数，然后会根据正则表达式去检索字符串  
	 serach()只会查找第一个，即使设置全局匹配也没用			  

# 正则表达式  

正则用来定义一些字符串的规则，程序可以根据这些规则来判断一个字符串是否符合规则，  
也可以将一个字符串中符合规则的内容提取出来。  
**创建正则表达式**  
 var reg = new RegExp("正则","匹配模式"); 注意：使用构造函数时，由于它的参数是一个字符串，而\是字符串中转义字符，如果要使用\则需要使用\\来代替  

 var reg = /正则表达式/匹配模式 （匹配模式可以多个一起写：/gi）  

## 语法：  

匹配模式：  
	i:忽略大小写（ignore）  
	g:全局匹配模式（默认为1次）  
	 设置匹配模式时，可以都不设置，也可以设置1个，也可以全设置，设置时没有顺序要求  
	  
正则语法		  
	| 或  
	[] 或  
	\[^ ] 除了  

	**[x-y] x的ascii到y的ascii码之间的值**  

[a-z] 小写字母 **（也可以[e-i])**  
[A-Z] 大写字母   
[A-z] 任意字母,**但是还包括了其他ASCII在此之中的**  
[0-9] 任意数字  

元符号  

检查一个字符串中是否含有 .  
  . 表示任意字符  
  在正则表达式中使用\作为转义字符  
  \. 来表示.  
  \\  表示\  

\w  
	任意字母、数字、_  [A-z0-9_]  
  \W  
	除了字母、数字、_  [ ^A-z0-9_]  
  \d  
	任意的数字 [0-9]  
  \D  
	除了数字 [ ^0-9]  
  \s  
	空格  
  \S  
	除了空格  
  \b  
	单词边界  
  \B  
	除了单词边界  

  

**量词**  
	 通过量词可以设置一个内容出现的次数  
	 量词只对它前边的一个内容起作用  
	 {n} 正好出现n次  
	 {m,n} 出现mn次  
	 {m,} m次以上  

+至少一个，相当于{1,}  
*个或多个，相当于{0,}  
 ? 0个或1个，相当于{0,1}	  

**边界表达式**（不要在java中用，javaScript中用）  
	^:正则开始  
	$:正则结束 ：**注意结束前一个才是结束匹配**  

```javascript  
reg = /^a/;  
reg = /b$/;  
```

## 方法：  

test()  
	 可以用来检查一个字符串是否符合正则表达式  
	 如果符合返回true，否则返回false  
例子  

去掉两端的空格:  

```javascript  
var s = "        	f    afa    ";   
s = s.replace(/^\s*|\s*$/g,"");  
```

# DOM  
Document Object Model  
文档对象模型，通过DOM可以来任意来修改网页中各个内容  
文档  
 文档指的是网页，一个网页就是一个文档  
对象  
 对象指将网页中的每一个节点都转换为对象  
	转换完对象以后，就可以以一种纯面向对象的形式来操作网页了  
模型  
 模型用来表示节点和节点之间的关系，方便操作页面  
节点（Node）  
 节点是构成网页的最基本的单元，网页中的每一个部分都可以称为是一个节点  
 虽然都是节点，但是节点的类型却是不同的  
 常用的节点  
	 文档节点 （Document），代表整个网页  
	 元素节点（Element），代表网页中的标签  
	 属性节点（Attribute），代表标签中的属性  
	 文本节点（Text），代表网页中的文本内容  
	  

## DOM操作  

 DOM查询  
 在网页中浏览器已经为我们提供了**document对象**，  
	**它代表的是整个网页，它是window对象的属性，可以在页面中直接使用。**  
 document查询方法：  
	 根据元素的id属性查询一个元素节点对象：  
		 document.getElementById("id属性值");  
	 根据元素的name属性值查询一组元素节点对象:  
		 document.getElementsByName("name属性值");  
	 根据标签名来查询一组元素节点对象：  
		 document.getElementsByTagName("标签名");  
		  
 元素的属性：  
	 **读取元素的属性：**  
		语法：元素.属性名  
		例子：ele.name    
			  ele.id    
			  ele.value   
			  ele.className  
			注意：class属性不能采用这种方式，  
			**读取class属性时需要使用 元素.classNam**e	   

修改元素的属性：  
	语法：元素.属性名 = 属性值  
	  
 innerHTML  
	 使用该属性可以获取或设置元素内部的HTML代码  

## 事件（Event）  

 事件指的是用户和浏览器之间的交互行为。比如：点击按钮、关闭窗口、鼠标移动。。。  
 我们可以为事件来绑定回调函数来响应事件。  
 绑定事件的方式：  
	1.可以在标签的事件属性中设置相应的JS代码  
		例子：  

```javascript  
<button onclick="js代码。。。">按钮</button>  
```

2.可以通过为对象的指定事件属性设置回调函数的形式来处理事件  
	例子：  

```javascript  
<button id="btn">按钮</button>  
<script>  
    var btn = document.getElementById("btn");  
    btn.onclick = function(){  
  
    };  
</script>  
```

文档的加载  
 浏览器在加载一个页面时，是按照自上向下的顺序加载的，加载一行执行一行。  
 如果将js代码编写到页面的上边，当代码执行时，页面中的DOM对象还没有加载，  
	此时将会无法正常获取到DOM对象，导致DOM操作失败。  
 解决方式一：  
	 可以将js代码编写到body的下边  
	  

```javascript  
<body>  
		<button id="btn">按钮</button>  
  
		<script>  
			var btn = document.getElementById("btn");  
			btn.onclick = function(){  
		  
			};  
	</script>  
</body>  
```

 解决方式二：  
	 将js代码编写到window.onload = function(){}中  
	 window.onload 对应的回调函数会在整个页面加载完毕以后才执行，  
		所以可以确保代码执行时，DOM对象已经加载完毕了		  

```javascript  
<script>  
    window.onload = function(){  
        var btn = document.getElementById("btn");  
        btn.onclick = function(){  
        };  
    };  
</script>	    
```

## DOM查询  

通过具体的元素节点来查询  
元素.getElementsByTagName()  
通过标签名查询当前元素的指定后代元素  

**子节点包括便签元素中的文本，子元素自包含标签元素**  

元素.childNodes  
 获取当前元素的**所有子节点**  
 **会获取到空白的文本子节点**  

childNodes属性会获取包括文本节点在呢的所有节点  
  根据DOM标签标签间空白也会当成文本节点  
  注意：在IE8及以下的浏览器中，不会将空白文本当成子节点，  
	所以该属性在IE8中会返回4个子元素而其他浏览器是9个  

元素.children  
 获取当前元素的**所有子元素**  

元素.firstChild  
 获取当前元素的**第一个子节点**，会获取到空白的文本子节点  

元素.lastChild  
 获取当前元素的**最后一个子节点**  

元素.parentNode  
 获取当前元素的父元素  

元素.previousSibling  
 获取当前元素的前一个兄弟节点  

previousElementSibling获取前一个兄弟元素，IE8及以下不支持  

元素.nextSibling  
 获取当前元素的后一个兄弟节点  

firstElementChild获取当前元素的第一个子元素  
 firstElementChild不支持IE8及以下的浏览器，  
	如果需要兼容他们尽量不要使用  

innerHTML和innerText  
这两个属性并没有在DOM标准定义，但是大部分浏览器都支持这两个属性  
两个属性作用类似，都可以获取到标签内部的内容，  
**不同是innerHTML会获取到html标签，而innerText会自动去除标签**  
如果使用这两个属性来设置标签内部的内容时，没有任何区别的	  

**读取标签内部的文本内容**  

</h1>h1中的文本内容</h1>  

元素.firstChild.nodeValue  

## document对象的其他的属性和方法  

document.all  
 **获取页面中的所有元素**，相当于document.getElementsByTagName("*");  

document.documentElement  
 **获取页面中html根元素**  

document.body  
 获取页面中的body元素  

document.getElementsByClassName()  
 **根据元素的class属性值查询一组元素节点对象**  
 这个方法不支持IE8及以下的浏览器  

document.querySelector()  
 **根据CSS选择器去页面中查询一个元素**  
 如果匹配到的元素有多个，则它会返回查询到的第一个元素	  

document.querySelectorAll()	  
 根据CSS选择器去页面中查询一组元素  
 会将匹配到所有元素封装到一个数组中返回，即使只匹配到一个  

##  DOM修改  

document.createElement("TagName")  
	可以用于创建一个元素节点对象，  
	它需要一个标签名作为参数，将会根据该标签名创建元素节点对象，  
	并将创建好的对象作为返回值返回  
document.createTextNode("textContent")  
可以根据文本内容创建一个文本节点对象  

**父节点.appendChild(子节点)**  
  向父节点中添加指定的子节点  
**父节点.insertBefore(新节点,旧节点)**  
 将一个新的节点插入到旧节点的前边  
父节点.replaceChild(新节点,旧节点)  
 使用一个新的节点去替换旧节点  

**父节点.removeChild(子节点)**  
 删除指定的子节点  
  推荐方式：**子节点.parentNode.removeChild(子节点)**  

**以上方法，实际就是改变了相应元素（标签）的innerHTML的值。**  

```javascript  
myClick("btn07",function(){  
    //向city中添加广州  
    var city = document.getElementById("city");  
  
    /*  
	* 使用innerHTML也可以完成DOM的增删改的相关操作  
	* 一般我们会两种方式结合使用  
	*/  
    //city.innerHTML += "<li>广州</li>";  
  
    //创建一个li  
    var li = document.createElement("li");  
    //向li中设置文本  
    li.innerHTML = "广州";  
    //将li添加到city中  
    city.appendChild(li);  
  
});  
```

## DOM对CSS的操作  

### 读取和修改内联样式  

使用style属性来操作元素的内联样式  
	读取内联样式：  
	语法：元素.style.样式名  
例子：  
	元素.style.width  
	元素.style.height  
	注意：**如果样式名中带有-，则需要将样式名修改为驼峰命名法将-去掉，然后后的字母改大写**  
	比如：backgroundcolor > backgroundColor  
	borderwidth > borderWidth  
修改内联样式：  
语法：元素.style.样式名 = 样式值  
 **通过style修改和读取的样式都是内联样式**，由于内联样式的优先级比较高，  
	所以我们通过JS来修改的样式，往往会立即生效，  
	**但是如果样式中设置了!important，则内联样式将不会生效。**  
	  

### 读取元素的当前样式  

正常浏览器  
 **使用getComputedStyle()**  
 这个方法是window对象的方法，可以返回一个对象，这个对象中保存着当前元素生效样式  
 参数：  
	1.要获取样式的元素  
	2.可以传递一个伪元素，一般传null  
 例子：  
	获取元素的宽度  
		getComputedStyle(box , null)["width"];  
 通过该方法读取到样式都是只读的不能修改  

IE8  
 **使用currentStyle**  
 语法：  
	元素.currentStyle.样式名  
 例子：  
	box.currentStyle["width"]  
 通过这个属性读取到的样式是只读的不能修改  

**实现兼容性**  

//对象.属性不存在，不会报错，如果直接寻找对象，（当前作用域到全局作用域）找不到会报错  

```javascript  
/*  
* 定义一个函数，用来获取指定元素的当前的样式  
* 参数：  
* 		obj 要获取样式的元素  
* 		name 要获取的样式名  
*/  
function getStyle(obj , name){  
//对象.属性不存在，不会报错，如果直接寻找对象，（当前作用域到全局作用域）找不到会报错  
    if(window.getComputedStyle){  
        //正常浏览器的方式，具有getComputedStyle()方法  
        return getComputedStyle(obj , null)[name];  
    }else{  
        //IE8的方式，没有getComputedStyle()方法  
        return obj.currentStyle[name];  
    }  
    //return window.getComputedStyle?getComputedStyle(obj , null)[name]:obj.currentStyle[name];			  
}  
```

### 其他的样式相关的属性  

注意：以下样式都是只读的,未指明偏移量都是相对于当前窗口左上角  

clientHeight  
 元素的可见高度，包括元素的内容区和内边距的高度  
clientWidth  
 元素的可见宽度，包括元素的内容区和内边距的宽度  
offsetHeight  
 整个元素的高度，包括内容区、内边距、边框  
offfsetWidth  
 整个元素的宽度，包括内容区、内边距、边框  
offsetParent  
 当前元素的定位父元素  
 离他最近的开启了定位的祖先元素，如果所有的元素都没有开启定位，则返回body  
offsetLeft  
offsetTop  
 当前元素和定位父元素之间的偏移量  
 offsetLeft水平偏移量  offsetTop垂直偏移量  

scrollHeight  
scrollWidth  
 获取元素滚动区域的高度和宽度  

scrollTop  
scrollLeft  
 获取元素垂直和水平滚动条滚动的距离  

判断滚动条是否滚动到底  
 垂直滚动条  
	scrollHeight -scrollTop = clientHeight  
	  
 水平滚动	  
	scrollWidth -scrollLeft = clientWidth	  

  

# 事件（Event）  

## 事件对象  

当响应函数被调用时，浏览器每次都会将一个事件对象作为实参传递进响应函数中，这个事件对象中封装了当前事件的相关信息，比如：鼠标的坐标，键盘的按键，鼠标的按键，滚轮的方向。。  

可以在响应函数中定义一个形参，来使用事件对象，但是在IE8以下浏览器中事件对象没有做完实参传递，而是作为window对象的属性保存  

例子：  
```javascript  
元素.事件 = function(event){  
    event = event || window.event;  
};  
  
元素.事件 = function(e){  
	e = e || event;  
	  
};  
```

**获取到鼠标的坐标**  
  clientX和clientY  
	用于获取鼠标在当前的可见窗口的坐标  
  div的偏移量，是相对于整个页面的  

  pageX和pageY 可以获取鼠标相对于当前页面的坐标  
	但是这个两个属性在IE8中不支持，所以如果需要兼容IE8，则不要使用  
var left = event.clientX;  
var　top = event.clientY;  

  

 ## 事件的冒泡（Bubble）  

 事件的冒泡指的是事件向上传导，当后代元素上的事件被触发时，将会导致其祖先元素上的同类事件也会触发。  
 事件的冒泡大部分情况下都是有益的，如果需要取消冒泡，则需要使用事件对象来取消  
 **可以将事件对象的cancelBubble设置为true，即可取消冒泡**  
   例子：  

```javascript  
元素.事件 = function(event){  
    event = event || window.event;  
    event.cancelBubble = true;  
};  
```

  

## 事件的委派  

 指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应函数来处理事件。  

事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能		  

我们希望，只绑定一次事件，即可应用到多个的元素上，即使元素是后添加的  
我们可以尝试将其绑定给元素的共同的祖先元素  

 **target** : event中的target表示的触发事件的对象  

## 事件的绑定  

addEventListener()  
 通过这个方法也可以为元素绑定响应函数  
参数：  
	1.事件的字符串，不要on  
	2.回调函数，当事件触发时该函数会被调用  
	3.是否在捕获阶段触发事件，需要一个布尔值，一般都传false  

使用addEventListener()可以同时为一个元素的相同事件同时绑定多个响应函数，  
这样当事件被触发时，响应函数将会按照函数的绑定顺序执行  

这个方法不支持IE8及以下的浏览器  

```javascript  
btn01.addEventListener("click",function(){  
	alert(1);  
},false);  
  
btn01.addEventListener("click",function(){  
	alert(2);  
},false);					  
```

attachEvent()  

 在IE8中可以使用attachEvent()来绑定事件  
参数：  
	1.事件的字符串，要on  
	2.回调函数  

这个方法也可以同时为一个事件绑定多个处理函数，  
	不同的是它是后绑定先执行，执行顺序和addEventListener()相反  

```javascript  
btn01.attachEvent("onclick",function(){  
alert(1);  
});  
  
btn01.attachEvent("onclick",function(){  
alert(2);  
});	  
```

```javascript  
//定义一个函数，用来为指定元素绑定响应函数  
/*  
			 * addEventListener()中的this，是绑定事件的对象  
			 * attachEvent()中的this，是window  
			 *  需要统一两个方法this  
			 */  
/*  
			 * 参数：  
			 * 	obj 要绑定事件的对象  
			 * 	eventStr 事件的字符串(不要on)  
			 *  callback 回调函数  
			 */  
function bind(obj , eventStr , callback){  
    if(obj.addEventListener){  
        //大部分浏览器兼容的方式  
        obj.addEventListener(eventStr , callback , false);  
    }else{  
        /*  
					 * this是谁由调用方式决定  
					 * callback.call(obj)  
					 */  
        //IE8及以下  
        obj.attachEvent("on"+eventStr , function(){  
            //在匿名函数中调用回调函数  
            callback.call(obj);  
        });  
    }  
}  
```

## 事件的传播  

 关于事件的传播网景公司和微软公司有不同的理解  
 微软公司认为事件应该是由内向外传播，也就是当事件触发时，应该先触发当前元素上的事件，  
	然后再向当前元素的祖先元素上传播，也就说事件应该在冒泡阶段执行。  
 网景公司认为事件应该是由外向内传播的，也就是当前事件触发时，应该先触发当前元素的最外层的祖先元素的事件，  
	然后在向内传播给后代元素  
 W3C综合了两个公司的方案，将事件传播分成了三个阶段  
	1.捕获阶段  
		 在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件  
	2.目标阶段  
		 事件捕获到目标元素，捕获结束开始在目标元素上触发事件  
	3.冒泡阶段  
		 事件从目标元素向他的祖先元素传递，依次触发祖先元素上的事件  

 如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true  
	一般情况下我们不会希望在捕获阶段触发事件，所以这个参数一般都是false  

 IE8及以下的浏览器中没有捕获阶段  

## 常用事件  

### 鼠标事件  

拖拽事件  

```javascript  
<!DOCTYPE html>  
    <html>  
    <head>  
    <meta charset="UTF-8">  
        <title></title>  
<style type="text/css">  
  
    #box1{  
width: 100px;  
height: 100px;  
background-color: red;  
position: absolute;  
}  
  
#box2{  
width: 100px;  
height: 100px;  
background-color: yellow;  
position: absolute;  
  
left: 200px;  
top: 200px;  
}  
  
    </style>  
  
<script type="text/javascript">  
  
    window.onload = function(){  
    /*  
				 * 拖拽box1元素  
				 *  - 拖拽的流程  
				 * 		1.当鼠标在被拖拽元素上按下时，开始拖拽  onmousedown  
				 * 		2.当鼠标移动时被拖拽元素跟随鼠标移动 onmousemove  
				 * 		3.当鼠标松开时，被拖拽元素固定在当前位置	onmouseup  
				 */  
  
    //获取box1  
    var box1 = document.getElementById("box1");  
    var box2 = document.getElementById("box2");  
    var img1 = document.getElementById("img1");  
  
    //开启box1的拖拽  
    drag(box1);  
    //开启box2的  
    drag(box2);  
  
    drag(img1);  
  
};  
  
/*  
			 * 提取一个专门用来设置拖拽的函数  
			 * 参数：开启拖拽的元素  
			 */  
function drag(obj){  
    //当鼠标在被拖拽元素上按下时，开始拖拽  onmousedown  
    obj.onmousedown = function(event){  
  
        //设置box1捕获所有鼠标按下的事件  
        /*  
					 * setCapture()  
					 * 	- 只有IE支持，但是在火狐中调用时不会报错，  
					 * 		而如果使用chrome调用，会报错  
					 */  
        /*if(box1.setCapture){  
						box1.setCapture();  
					}*/  
        obj.setCapture && obj.setCapture();  
  
  
        event = event || window.event;  
        //div的偏移量 鼠标.clentX - 元素.offsetLeft  
        //div的偏移量 鼠标.clentY - 元素.offsetTop  
        var ol = event.clientX - obj.offsetLeft;  
        var ot = event.clientY - obj.offsetTop;  
  
  
        //为document绑定一个onmousemove事件  
        document.onmousemove = function(event){  
            event = event || window.event;  
            //当鼠标移动时被拖拽元素跟随鼠标移动 onmousemove  
            //获取鼠标的坐标  
            var left = event.clientX - ol;  
            var top = event.clientY - ot;  
  
            //修改box1的位置  
            obj.style.left = left+"px";  
            obj.style.top = top+"px";  
  
        };  
  
        //为document绑定一个鼠标松开事件  
        document.onmouseup = function(){  
            //当鼠标松开时，被拖拽元素固定在当前位置	onmouseup  
            //取消document的onmousemove事件  
            document.onmousemove = null;  
            //取消document的onmouseup事件  
            document.onmouseup = null;  
            //当鼠标松开时，取消对事件的捕获  
            obj.releaseCapture && obj.releaseCapture();  
        };  
  
 /*  
* 当我们拖拽一个网页中的内容时，浏览器会默认去搜索引擎中搜索内容，  
* 	此时会导致拖拽功能的异常，这个是浏览器提供的默认行为，  
* 	如果不希望发生这个行为，则可以通过return false来取消默认行为  
*   
* 但是这招对IE8不起作用  
*/  
        return false;  
    };  
}  
  
  
</script>  
</head>  
<body>  
  
    我是一段文字  
  
<div id="box1"></div>  
  
<div id="box2"></div>  
  
<img src="img/an.jpg" id="img1" style="position: absolute;"/>  
    </body>  
</html>  
```

  

滚轮事件：  

onwheel都支持  

```javascript		  
<!DOCTYPE html>  
    <html>  
    <head>  
    <meta charset="UTF-8">  
        <title></title>  
<style type="text/css">  
  
    #box1{  
width: 100px;  
height: 100px;  
background-color: red;  
}  
  
    </style>  
<script type="text/javascript">  
  
    window.onload = function(){  
  
  
    //获取id为box1的div  
    var box1 = document.getElementById("box1");  
  
    //为box1绑定一个鼠标滚轮滚动的事件  
    /*  
				 * onmousewheel鼠标滚轮滚动的事件，会在滚轮滚动时触发，  
				 * 	但是火狐不支持该属性  
				 *   
				 * 在火狐中需要使用 DOMMouseScroll 来绑定滚动事件  
				 * 	注意该事件需要通过addEventListener()函数来绑定  
				 */  
  
  
    box1.onmousewheel = function(event){  
  
        event = event || window.event;  
  
  
        //event.wheelDelta 可以获取鼠标滚轮滚动的方向  
        //向上滚 120   向下滚 -120  
        //wheelDelta这个值我们不看大小，只看正负  
  
        //alert(event.wheelDelta);  
  
        //wheelDelta这个属性火狐中不支持  
        //在火狐中使用event.detail来获取滚动的方向  
        //向上滚 -3  向下滚 3  
        //alert(event.detail);  
  
  
        /*  
					 * 当鼠标滚轮向下滚动时，box1变长  
					 * 	当滚轮向上滚动时，box1变短  
					 */  
        //判断鼠标滚轮滚动的方向  
        if(event.wheelDelta > 0 || event.detail < 0){  
            //向上滚，box1变短  
            box1.style.height = box1.clientHeight - 10 + "px";  
  
        }else{  
            //向下滚，box1变长  
            box1.style.height = box1.clientHeight + 10 + "px";  
        }  
  
        /*  
					 * 使用addEventListener()方法绑定响应函数，取消默认行为时不能使用return false  
					 * 需要使用event来取消默认行为event.preventDefault();  
					 * 但是IE8不支持event.preventDefault();这个玩意，如果直接调用会报错  
					 */  
        event.preventDefault && event.preventDefault();  
  
  
        /*  
					 * 当滚轮滚动时，如果浏览器有滚动条，滚动条会随之滚动，  
					 * 这是浏览器的默认行为，如果不希望发生，则可以取消默认行为  
					 */  
        return false;  
  
  
  
  
    };  
  
    //为火狐绑定滚轮事件  
    bind(box1,"DOMMouseScroll",box1.onmousewheel);  
  
  
};  
  
  
function bind(obj , eventStr , callback){  
    if(obj.addEventListener){  
        //大部分浏览器兼容的方式  
        obj.addEventListener(eventStr , callback , false);  
    }else{  
        /*  
					 * this是谁由调用方式决定  
					 * callback.call(obj)  
					 */  
        //IE8及以下  
        obj.attachEvent("on"+eventStr , function(){  
            //在匿名函数中调用回调函数  
            callback.call(obj);  
        });  
    }  
}  
  
</script>  
</head>  
<body style="height: 2000px;">  
  
    <div id="box1"></div>  
  
</body>  
</html>  
  
```

### 键盘事件  

键盘事件：  
onkeydown  
 按键被按下  
 对于onkeydown来说如果一直按着某个按键不松手，则事件会一直触发  
 当onkeydown连续触发时，第一次和第二次之间会间隔稍微长一点，其他的会非常的快，这种设计是为了防止误操作的发生。  
onkeyup  
 按键被松开  

键盘事件一般都会绑定给一些可以获取到焦点的对象或者是document  

keyCode  

可以通过keyCode来获取按键的编码  
通过它可以判断哪个按键被按下  
除了keyCode，事件对象中还提供了几个属性  
altKey  
ctrlKey  
shiftKey  
这个三个用来判断alt ctrl 和 shift是否被按下  
如果按下则返回true，否则返回false  

```javascript  
//console.log(event.keyCode);  
  
//判断一个y是否被按下  
//判断y和ctrl是否同时被按下  
if(event.keyCode === 89 && event.ctrlKey){  
	console.log("ctrl和y都被按下了");  
}  
```

```javascript  
input.onkeydown = function(event) {  
    event = event || window.event;  
    //数字 48 - 57  
    //使文本框中不能输入数字  
    if(event.keyCode >= 48 && event.keyCode <= 57) {  
        //在文本框中输入内容，属于onkeydown的默认行为  
        //如果在onkeydown中取消了默认行为，则输入的内容，不会出现在文本框中  
        return false;  
    }  
};  
  
```

# BOM  

浏览器对象模型(browser object model)  
BOM可以使我们通过JS来操作浏览器  
在BOM中为我们提供了一组对象，用来完成对浏览器的操作  
BOM对象  
Window  
 代表的是整个浏览器的窗口，同时window也是网页中的全局对象  
Navigator  
 代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器  
Location  
 代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息，或者操作浏览器跳转页面  
History  
 代表浏览器的历史记录，可以通过该对象来操作浏览器的历史记录  
	由于隐私原因，该对象不能获取到具体的历史记录，只能操作浏览器向前或向后翻页  
	而且该操作只在当次访问时有效  
Screen  
 代表用户的屏幕的信息，通过该对象可以获取到用户的显示器的相关的信息  

这些BOM对象在浏览器中都是作为window对象的属性保存的，  
可以通过window对象来使用，也可以直接使用  

## Navigator  

 代表的当前浏览器的信息，通过该对象可以来识别不同的浏览器  
 由于历史原因，Navigator对象中的大部分属性都已经不能帮助我们识别浏览器了  
 一般我们只会使用userAgent来判断浏览器的信息，  
	userAgent是一个字符串，这个字符串中包含有用来描述浏览器信息的内容，  
	不同的浏览器会有不同的userAgent  

火狐的userAgent  
Mozilla5.0 (Windows NT 6.1; WOW64; rv:50.0) Gecko20100101 Firefox50.0  

Chrome的userAgent  
Mozilla5.0 (Windows NT 6.1; Win64; x64) AppleWebKit537.36 (KHTML, like Gecko) Chrome52.0.2743.82 Safari537.36  

IE8  
Mozilla4.0 (compatible; MSIE 8.0; Windows NT 6.1; WOW64; Trident7.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E)  

IE9  
Mozilla5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident7.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E)  

IE10  
Mozilla5.0 (compatible; MSIE 10.0; Windows NT 6.1; WOW64; Trident7.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E)  

IE11  
Mozilla5.0 (Windows NT 6.1; WOW64; Trident7.0; SLCC2; .NET CLR 2.0.50727; .NET CLR 3.5.30729; .NET CLR 3.0.30729; Media Center PC 6.0; .NET4.0C; .NET4.0E; rv:11.0) like Gecko  
 在IE11中已经将微软和IE相关的标识都已经去除了，所以我们基本已经不能通过UserAgent来识别一个浏览器是否是IE了  

```javascript  
alert(navigator.appName);  
  
var ua = navigator.userAgent;  
  
console.log(ua);  
  
if(firefoxi.test(ua)){  
alert("你是火狐！！！");  
}else if(chromei.test(ua)){  
alert("你是Chrome");  
}else if(msiei.test(ua)){  
alert("你是IE浏览器~~~");  
}else if("ActiveXObject" in window){  
alert("你是IE11，枪毙了你~~~");  
}  
```

## History  

 对象可以用来操作浏览器向前或向后翻页	  
length  
 属性，可以获取到当成访问的链接数量  
back()  
 可以用来回退到上一个页面，作用和浏览器的回退按钮一样	  
forward()  
 可以跳转下一个页面，作用和浏览器的前进按钮一样	  
go()  
 可以用来跳转到指定的页面  
 它需要一个整数作为参数  
	1:表示向前跳转一个页面 相当于forward()  
	2:表示向前跳转两个页面  
	-1:表示向后跳转一个页面  
	-2:表示向后跳转两个页面  

## Location  

 该对象中封装了浏览器的地址栏的信息	  
如果直接打印location，则可以获取到地址栏的信息（当前页面的完整路径）  
alert(location);  
如果直接将location属性修改为一个完整的路径，或相对路径  
则我们页面会自动跳转到该路径，并且会生成相应的历史记录  
location = "http:www.baidu.com";  
location = "01.BOM.html";  
assign()  
 用来跳转到其他的页面，作用和直接修改location一样	  
reload()  
 用于重新加载当前页面，作用和刷新按钮一样  
 如果在方法中传递一个true，作为参数，则会强制清空缓存刷新页面  
location.reload(true);	  
replace()  
 可以使用一个新的页面替换当前页面，调用完毕也会跳转页面  
	不会生成历史记录，不能使用回退按钮回退  

## window  

###  定时器  

**setInterval()**  
 定时调用  
 可以将一个函数，每隔一段时间执行一次  
 参数：  
	1.回调函数，该函数会每隔一段时间被调用一次  
	2.每次调用间隔的时间，单位是毫秒  

 返回值：  
	返回一个Number类型的数据  
	这个数字用来作为定时器的唯一标识  
**clearInterval()可以用来关闭一个定时器**  
方法中需要一个定时器的标识作为参数，这样将关闭标识对应的定时器   

clearInterval()可以接收任意参数，  
	如果参数是一个有效的定时器的标识，则停止对应的定时器  
	如果参数不是一个有效的标识，则什么也不做  

```javascript  
var num = 1;  
var timer = setInterval(function() {  
	count.innerHTML = num++;  
	if(num == 11) {  
		//关闭定时器  
		clearInterval(timer);  
	}  
}, 1000);  
```

### 延时调用  

**setTimeout**  

延时调用一个函数不马上执行，而是隔一段时间以后在执行，而且只会执行一次  
延时调用和定时调用的区别，定时调用会执行多次，而延时调用只会执行一次  
延时调用和定时调用实际上是可以互相代替的，在开发中可以根据自己需要去选择  

var timer = setTimeout(function(){  
console.log(num++);  
},3000);  

使用clearTimeout()来关闭一个延时调用  
clearTimeout(timer);  

#类的操作  

**直接修改元素的类css：**  

通过style属性来修改元素的样式，每修改一个样式，浏览器就需要重新渲染一次页面。 这样的执行的性能是比较差的，而且这种形式当我们要修改多个样式时，也不太方便 我希望一行代码，可以同时修改多个样式  

我们可以通过修改元素的class属性来间接的修改样式.这样一来，我们只需要修改一次，即可同时修改多个样式，浏览器只需要重新渲染页面一次，性能比较好，  
并且这种方式，可以使表现和行为进一步的分离  

```javascript  
box.className += " b2";	//注意有空格，添加class属性  
```

```javascript  
//定义一个函数，用来向一个元素中添加指定的class属性值  
/*  
 * 参数:  
 * 	obj 要添加class属性的元素  
 *  cn 要添加的class值  
 * 	  
 */  
function addClass(obj, cn) {  
	if (!hasClass(obj, cn)) {  
		obj.className += " " + cn;  
	}  
}  
/*  
 * 判断一个元素中是否含有指定的class属性值  
 * 	如果有该class，则返回true，没有则返回false  
 * 	  
 */  
function hasClass(obj, cn) {  
	var reg = new RegExp("\\b" + cn + "\\b");  
	return reg.test(obj.className);  
}  
/*  
 * 删除一个元素中的指定的class属性  
 */  
function removeClass(obj, cn) {  
	//创建一个正则表达式  
	var reg = new RegExp("\\b" + cn + "\\b");  
	//删除class  
	obj.className = obj.className.replace(reg, "");  
}  
/*  
 * toggleClass可以用来切换一个类  
 * 	如果元素中具有该类，则删除  
 * 	如果元素中没有该类，则添加  
 */  
function toggleClass(obj , cn){	  
	//判断obj中是否含有cn  
	if(hasClass(obj , cn)){  
		//有，则删除  
		removeClass(obj , cn);  
	}else{  
		//没有，则添加  
		addClass(obj , cn);  
	}  
}  
```

# JSON  

 **JavaScript Object Notation** JS对象表示法

## JSON 格式

1. 复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。
2. 原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和`null`（不能使用`NaN`, `Infinity`, `-Infinity`和`undefined`）。
3. 字符串**必须使用双引号表示**，不能使用单引号。
4. 对象的键名必须放在双引号里面。
5. 数组或对象最后一个成员的后面，不能加逗号。

  

JS中的对象只有JS自己认识，其他的语言都不认识  
**JSON就是一个特殊格式的字符串**，这个字符串可以被任意的语言所识别，  
并且可以转换为任意语言中的对象，JSON在开发中主要用来数据的交互  
 JSON和JS对象的格式一样，只不过**JSON字符串中的属性名必须加双引号**  
	其他的和JS语法一致  
JSON分类：  
	1.对象 {}  
	2.数组 []  

JSON中允许的值：  
	1.字符串  
	2.数值  
	3.布尔值  
	4.null  
	5.对象  
	6.数组  

举例：  

```javascript  
var arr = '[1,2,3,"hello",true]';  
			  
var obj2 = '{"arr":[1,2,3]}';  
  
var arr2 ='[{"name":"孙悟空","age":18,"gender":"男"},{"name":"孙悟空","age":18,"gender":"男"}]';  
```

JSON工具类  

json > js对象  
JSON.parse()  
 可以将以JSON字符串转换为js对象  
 它需要一个JSON字符串作为参数，会将该字符串转换为JS对象并返回  

var o = JSON.parse(json);  
var o2 = JSON.parse(arr);  

var obj3 = {name:"猪八戒" , age:28 , gender:"男"};  

JS对象 > JSON  
JSON.stringify()                -ify/fy，表示"使……化。  
 可以将一个JS对象转换为JSON字符串  
 需要一个js对象作为参数，会返回一个JSON字符串  

var str = JSON.stringify(obj3);  
console.log(str);  

JSON这个对象在IE7及以下的浏览器中不支持，所以在这些浏览器中调用时会报错  



  

  

  

  

  

  


​		  

# other  

## localStorage  

只读的`localStorage` 属性允许你访问一个[`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document) 源（origin）的对象 [`Storage`](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage)；其存储的数据能在跨浏览器会话保留。`localStorage` 类似 [`sessionStorage`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage)，但其区别在于：存储在 `localStorage` 的数据可以长期保留；而当页面会话结束——也就是说，当页面被关闭时，存储在 `sessionStorage` 的数据会被清除 。  

## eval()  

eval()  
 这个函数可以用来执行一段字符串形式的JS代码，并将执行结果返回  
 如果使用eval()执行的字符串中含有{},它会将{}当成是代码块  
	如果不希望将其当成代码块解析，则需要在字符串前后各加一个()  

 eval()这个函数的功能很强大，可以直接执行一个字符串中的js代码，  
	但是在开发中尽量不要使用，首先它的执行性能比较差，然后它还具有安全隐患  
		  

	var str = '{"name":"孙悟空","age":18,"gender":"男"}';  
	var obj = eval("("+str+")");  
编码  

```html  
<!DOCTYPE html>  
<html>  
	<head>  
		<meta charset="UTF-8">  
		<title></title>  
		<script type="text/javascript">  
			  
			/*  
			 * 在字符串中使用转义字符输入Unicode编码  
			 * 	\u四位编码  
			 */  
			console.log("\u2620");	  
		</script>  
	</head>  
	<body>		  
		<!--在网页中使用Unicode编码  
			&#编码; 这里的编码需要的是10进制  
		-->  
		<h1 style="font-size: 200px;">&#9760;</h1>  
		<h1 style="font-size: 200px;">&#9856;</h1>		  
	</body>  
</html>  
  
```

confirm()用于弹出一个带有确认和取消按钮的提示框  
需要一个字符串作为参数，该字符串将会作为提示文字显示出来  
如果用户点击确认则会返回true，如果点击取消则返回false  
var flag = confirm("确认删除"+name+"吗?");  

## # 原生js  

## 原生js实现复制内容到剪切板  

```js  
copy() {  
    const input = document.createElement("input");  
    document.body.appendChild(input);  
    input.setAttribute("value",this.solution.code);  
    input.select();  
    if (document.execCommand("copy")) {  
        document.execCommand("copy");  
        // console.log("复制成功");  
    }  
    document.body.removeChild(input);  
}  
```

  **[github笔记下载地址](https://github.com/codeOflI/codeOflI.github.io/blob/dev/source/_posts/js-note/javaScript/javaScript.md)**

