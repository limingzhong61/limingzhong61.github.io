---
title: vue
p: js-note/vue/vue
date: 2019-11-11 23:23:44
categories:
tags: js vue
---
# 第 1 章： Vue 核心
## 1.1. Vue 的基本认识
1.1.1. 官网
1) 英文官网: https://vuejs.org/
2) 中文官网: https://cn.vuejs.org/

### 1.1.2. 介绍描述
1) 渐进式 JavaScript 框架
2) 作者: 尤雨溪(一位华裔前 Google 工程师)
3) 作用: 动态构建用户界面

### 1.1.3. Vue 的特点
1) 遵循 MVVM 模式
2) 编码简洁, 体积小, 运行效率高, 适合移动/PC 端开发
3) 它本身只关注 UI, 可以轻松引入 vue 插件或其它第三库开发项目

### 1.1.4. 与其它前端 JS 框架的关联
1) 借鉴 angular 的**模板和数据绑定**技术
2) 借鉴 react 的**组件化和虚拟 DOM 技术** 

### 1.1.5. Vue 扩展插件
1) vue-cli: vue 脚手架
2) vue-resource(axios): ajax 请求
3) vue-router: 路由
4) vuex: 状态管理
5) vue-lazyload: 图片懒加载
6) vue-scroller: 页面滑动相关
7) mint-ui: 基于 vue 的 UI 组件库(移动端)
8) element-ui: 基于 vue 的 UI 组件库(PC 端) 

## 1.2. Vue 的基本使用 

```html
<div id="app">
    <input type="text" v-model="username">
    <p>Hello, {{username}}</p>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            username: 'atguigu'
        }
    })
</script>
```

### 1.2.3. 使用 vue 开发者工具调试

[Vue.jsdevtools-翻墙安装方式-推荐](https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=zh-CN)

### 1.2.4.理解Vue的MVVM

![1569983854283](/vue/1569983854283.png)

## 1.3. 模板语法

### 1.3.2.模板的理解

1) 动态的 html 页面
2) 包含了一些 JS 语法代码
a. 双大括号表达式
b. 指令(以 v-开头的自定义标签属性)
### 1.3.3. 双大括号表达式
1) 语法: {{exp}}
2) 功能: 向页面输出数据
3) 可以调用对象的方法

### 1.3.4. 指令一: 强制数据绑定
1) 功能: 指定变化的属性值
2) 完整写法: v-bind:xxx='yyy' //yyy 会作为表达式解析执行
3) 简洁写法: :xxx='yyy'

### 1.3.5. 指令二: 绑定事件监听
1) 功能: 绑定指定事件名的回调函数
2) 完整写法:
v-on:keyup='xxx'
v-on:keyup='xxx(参数)'
v-on:keyup.enter='xxx'
3) 简洁写法:
@keyup='xxx'
@keyup.enter='xx

### 1.3.6. 编码 

```html
<div id="app">
    <h2>1. 双大括号表达式</h2>
    <p>{{msg}}</p>
    <p>{{msg.toUpperCase()}}</p>
    <h2>2. 指令一: 强制数据绑定</h2>
    <a href="url">访问指定站点</a><br><!--不能使用-->
    <a v-bind:href="url">访问指定站点 2</a><br>
    <a :href="url">访问指定站点 3</a><br>
    <h2>3. 指令二: 绑定事件监听</h2>
    <button v-on:click="handleClick">点我</button>
    <button @click="handleClick">点我 2</button>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el: '#app',
        data: {// data 的所有属性都会成功 vm 对象的属性, 而模板页面中可以直接访问
            msg: 'NBA I Love This Game!',
            url: 'http://www.baidu.com'
        },
        methods: {
            handleClick () {
                alert('处理点击')
            }
        }
    })
</script>
```

### 1.3.8 v-model`和`双向数据绑定`

**v-model 只能运用在 表单元素和组件中**

```html
<h4>{{ msg }}</h4>

<!-- v-bind 只能实现数据的单向绑定，从 M 自动绑定到 V， 无法实现数据的双向绑定  -->
<input type="text" v-bind:value="msg" style="width:100%;">

<!-- 使用  v-model 指令，可以实现 表单元素和 Model 中数据的双向数据绑定 -->
<!-- input(radio, text, address, email....)   select    checkbox   textarea   -->
<input type="text" style="width:100%;" v-model="msg">
```

## [自定义组件的 `v-model`](https://cn.vuejs.org/v2/guide/components-custom-events.html#自定义组件的-v-model)

一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` 特性用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。

## 1.4. 计算属性和监视 

### 1.4.2. 计算属性

1) 在 computed 属性对象中定义计算属性的方法
2) 在页面中使用{{方法名}}来显示计算的结果
### 1.4.3. 监视属性
1) 通过通过 vm 对象的$watch()或 watch 配置来监视指定的属性
2) 当属性变化时, 回调函数自动调用, 在函数内部进行计算

### watch`属性的使用

考虑一个问题：想要实现 `名` 和 `姓` 两个文本框的内容改变，则全名的文本框中的值也跟着改变；（用以前的知识如何实现？？？）

1.监听`data`中属性的改变：

```js
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen',
        fullName: 'jack - chen'
      },
      methods: {},
      watch: {
        'firstName': function (newVal, oldVal) { // 第一个参数是新数据，第二个参数是旧数据
          this.fullName = newVal + ' - ' + this.lastName;
        },
        'lastName': function (newVal, oldVal) {
          this.fullName = this.firstName + ' - ' + newVal;
        }
      }
    });
  </script>
```

2.监听路由对象的改变：

```js
<div id="app">
    <router-link to="/login">登录</router-link>
    <router-link to="/register">注册</router-link>

    <router-view></router-view>
  </div>

  <script>
    var login = Vue.extend({
      template: '<h1>登录组件</h1>'
    });

    var register = Vue.extend({
      template: '<h1>注册组件</h1>'
    });

    var router = new VueRouter({
      routes: [
        { path: "/login", component: login },
        { path: "/register", component: register }
      ]
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {},
      router: router,
      watch: {
        '$route': function (newVal, oldVal) {
          if (newVal.path === '/login') {
            console.log('这是登录组件');
          }
        }
      }
    });
  </script>
```

### 1.4.4. 计算属性高级

1) 通过 getter/setter 实现对属性数据的显示和监视
2) **计算属性存在缓存, 多次读取只执行一次 getter 计算**

### `computed`计算属性的使用

// 计算属性； 特点：当计算属性中所以来的任何一个 data 属性改变之后，都会重新触发 本计算属性 的重新计算，从而更新 fullName 的值
          **注意1：** 计算属性，在引用的时候，一定不要加 () 去调用，直接把它 当作 普通 属性去使用就好了；
        **注意2：** 只要 计算属性，这个 f**unction 内部，所用到的 任何 data 中的数据发送了变化，就会 立即重新计算 这个 计算属性的值**
        **注意3：** 计算属性的求值结果，会被缓存起来，方便下次直接使用； 如果 计算属性方法中，所以来的任何数据，都没有发生过变化，则，不会重新对 计算属性求值；

1. 默认只有`getter`的计算属性：

```js
<div id="app">
    <input type="text" v-model="firstName"> +
    <input type="text" v-model="lastName"> =
    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen'
      },
      methods: {},
      computed: { 
        fullName() {
          return this.firstName + ' - ' + this.lastName;
        }
      }
    });
  </script>
```

2.定义有`getter`和`setter`的计算属性：

```js
<div id="app">
    <input type="text" v-model="firstName">
    <input type="text" v-model="lastName">
    <!-- 点击按钮重新为 计算属性 fullName 赋值 -->
    <input type="button" value="修改fullName" @click="changeName">

    <span>{{fullName}}</span>
  </div>

  <script>
    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {
        firstName: 'jack',
        lastName: 'chen'
      },
      methods: {
        changeName() {
          this.fullName = 'TOM - chen2';
        }
      },
      computed: {
        fullName: {
          get: function () {
            return this.firstName + ' - ' + this.lastName;
          },
          set: function (newVal) {
            var parts = newVal.split(' - ');
            this.firstName = parts[0];
            this.lastName = parts[1];
          }
        }
      }
    });
  </script>
```

### 1.4.5. 编码

```html
<div id="demo">
    姓: <input type="text" placeholder="First Name" v-model="firstName"><br>
    名: <input type="text" placeholder="Last Name" v-model="lastName"><br>
    姓名 1(单向): <input type="text" placeholder="Full Name" v-model="fullName1"><br>
    姓名 2(单向): <input type="text" placeholder="Full Name" v-model="fullName2"><br>
    姓名 3(双向): <input type="text" placeholder="Full Name2" v-model="fullName3"><br>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#demo',
        data: {
            firstName: 'Kobe',
            lastName: 'bryant',
            fullName2: 'Kobe bryant'
        },
        computed: {
            fullName: function () {
                return this.firstName + " " + this.lastName
            },
            fullName3: {
                get: function () {
                    return this.firstName + " " + this.lastName
                },
                set: function (value) {
                    var names = value.split(' ')
                    this.firstName = names[0]
                    this.lastName = names[1]
                }
            }
        },
        watch: {
            lastName: function (newVal, oldVal) {
                this.fullName2 = this.firstName + ' ' + newVal
            }
        }
    })
    vm.$watch('firstName', function (val) {
        this.fullName2 = val + ' ' + this.lastName
    })
```

### ``watch`、`computed`和`methods`之间的对比

1. `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
2. `methods`方法表示一个具体的操作，主要书写业务逻辑；
3. `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；

## 1.5. class 与 style 绑定

### 1.5.2. 理解

1) 在应用界面中, 某个(些)元素的样式是变化的
2) class/style 绑定就是专门用来实现动态样式效果的技术

### 1.5.3. class 绑定（使用class样式）

1) :class='xxx'
2) 表达式是字符串: 'classA'
3) 表达式是对象: {classA:isA, classB: isB}
4) 表达式是数组: ['classA', 'classB']

1.数组

```html
<h1 :class="['red', 'thin']">这是一个邪恶的H1</h1>
```

2.数组中使用三元表达式

```html
<h1 :class="['red', 'thin', isactive?'active':'']">这是一个邪恶的H1</h1>
```

3.数组中嵌套对象

```html
<h1 :class="['red', 'thin', {'active': isactive}]">这是一个邪恶的H1</h1>
```

4.直接使用对象

```html
<h1 :class="{red:true, italic:true, active:true, thin:true}">这是一个邪恶的H1</h1>
```

**注意**
**如果类名中有-，必须用`''`括起来,不然没有用**

```html
<li :class="{'page-item': true,active: true}"></li>
```

### 1.5.4. style 绑定

1) :style="{ color: activeColor, fontSize: fontSize + 'px' }"
2) 其中 activeColor/fontSize 是 data 属性

### 1.5.5. 编吗

```html
<style>
    .classA {
        color: red;
    } .
    classB {
        background: blue;
    } .
    classC {
        font-size: 20px;
    }
</style>
<div id="demo">
    <h2>1. class 绑定: :class='xxx'</h2>
    <p class="classB" :class="a">表达式是字符串: 'classA'</p>
    <p :class="{classA: isA, classB: isB}">表达式是对象: {classA:isA, classB: isB}</p>
    <p :class="['classA', 'classC']"> 表达式是数组: ['classA', 'classB']</p>
    <h2>2. style 绑定</h2>
    <p :style="{color, fontSize}">style="{ color: activeColor, fontSize: fontSize +
        'px' }"</p>
    <button @click="update">更新</button>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el : '#demo',
        data : {
            a: 'classA',
            isA: true,
            isB: false,
            color: 'red',
            fontSize: '20px'
        },
        methods : {
            update () {
                this.a = 'classC'
                this.isA = false
                this.isB = true
                this.color = 'blue'
                this.fontSize = '30px'
            }
        }
    })
</script>
```

## 1.6. 条件渲染 

### 1.6.2. 条件渲染指令

1) v-if 与 v-else
2) v-show

### 1.6.3. 比较 v-if 与 v-show  

3)**如果需要频繁切换 v-show 较好**
4)**当条件不成立时, v-if 的所有子节点不会解析(项目中使用)**

### 1.6.4. 编码  

```html
<div id="demo">
    <h2 v-if="ok">表白成功</h2>
    <h2 v-else>表白失败</h2>
    <h2 v-show="ok">求婚成功</h2>
    <h2 v-show="!ok">求婚失败</h2>
    <br>
    <button @click="ok=!ok">切换</button>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#demo',
        data: {
            ok: false
        }
    })
</script>
```

## 1.7. 列表渲染  

1) 列表显示指令
数组: v-for / index
对象: v-for / key
2) 列表的更新显示
删除 item
替换 item
3) 列表的高级处理
列表过滤
列表排序

[**注意事项**](https://cn.vuejs.org/v2/guide/list.html#注意事项)

由于 JavaScript 的限制，Vue **不能**检测以下数组的变动：

1. 当你**利用索引直接设置一个数组项时**，例如：`vm.items[indexOfItem] = newValue`
2. 当你**修改数组的长度时**，例如：`vm.items.length = newLength`

### 1.7.2. 编码  

```html
<div id="demo">
    <h2>测试: v-for 遍历数组</h2>
    <ul>
        <li v-for="(p, index) in persons" :key="index">
            {{index}}--{{p.name}}--{{p.age}}
            <button @click="deleteItem(index)">删除</button>
            <button @click="updateItem(index, {name:'Jok',age:15})">更新</button>
        </li>
    </ul>
    <h2>测试: v-for 遍历对象</h2>
    <ul>
        <li v-for="(value, key) in persons[0]">
            {{ key }} : {{ value }}
        </li>
    </ul>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el: '#demo',
        data: {
            persons: [
                {id: 1, name: 'Tom', age: 13},
                {id: 2, name: 'Jack', age: 12},
                {id: 3, name: 'Bob', age: 14}
            ]
        },
        methods: {
            deleteItem(index) {
                this.persons.splice(index, 1)
            },
            updateItem(index, p) {
                // this.persons[index] = p // 页面不会更新
                this.persons.splice(index, 1, p)
            }
        }
    })
</script>
```

```html
<div id="demo">
    <input type="text" name="searchName" placeholder="搜索指定用户名"
           v-model="searchName">
    <ul>
        <li v-for="(p, index) in filterPerson" :key="index">
            {{index}}--{{p.name}}--{{p.age}}
        </li>
    </ul>
    <button @click="setOrderType(1)">年龄升序</button>
    <button @click="setOrderType(2)">年龄降序</button>
    <button @click="setOrderType(0)">原本顺序</button>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el: '#demo',
        data: {
            orderType: 0, //0 代表不排序, 1 为升序, 2 为降序
            searchName: '',
            persons: [
                {id: 1, name: 'Tom', age: 13},
                {id: 2, name: 'Jack', age: 12},
                {id: 3, name: 'Bob', age: 17},
                {id: 4, name: 'Cat', age: 14},
                {id: 4, name: 'Mike', age: 14},
                {id: 4, name: 'Monica', age: 16}
            ]
        },
        methods: {
            setOrderType (orderType) {
                this.orderType = orderType
            }
        },
        computed: {
            filterPerson() {
                let {orderType, searchName, persons} = this
                // 过滤
                persons = persons.filter(p => p.name.indexOf(searchName)!=-1)
                // 排序
                if(orderType!==0) {
                    persons = persons.sort(function (p1, p2) {
                        if(orderType===1) {
                            return p1.age-p2.age
                        } else {
                            return p2.age-p1.age
                        }
                    })
                } r
                eturn persons
            }
        }
    })
</script>
```

## 1.8. 事件处理

### 1.8.2. 绑定监听

1) v-on:xxx="fun"
2) @xxx="fun"
3) @xxx="fun(参数)"
4) 默认事件形参: event 

**`event` 是原生 DOM 事件**
5) 隐含属性对象: $event

### 1.8.3. 事件修饰符

1) .prevent : 阻止事件的默认行为 event.preventDefault()
2) .stop : 停止事件冒泡 event.stopPropagation()

### 事件修饰符

- .stop       阻止冒泡

  ```html
  <!-- 使用  .stop  阻止冒泡 -->
  <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click.stop="btnHandler">
  </div>
  ```

- .prevent    阻止默认事件

  ```html
  <!-- 使用 .prevent 阻止默认行为 -->
      <a href="http://www.baidu.com" @click.prevent="linkClick">有问题，先去百度</a>
  ```

- .capture    添加事件侦听器时使用事件捕获模式

  ```html
  <!-- 使用  .capture 实现捕获触发事件的机制 -->
  <div class="inner" @click.capture="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
  </div>
  ```

- .self       只当事件在该元素本身（比如不是子元素）触发时触发回调

  ```html
  <!-- 使用 .self 实现只有点击当前元素时候，才会触发事件处理函数 -->
  <div class="inner" @click="div1Handler">
      <input type="button" value="戳他" @click="btnHandler">
  </div>
  ```

- .once       事件只触发一次

```html
<!-- 使用 .once 只触发一次事件处理函数 -->
<a href="http://www.baidu.com" @click.prevent.once="linkClick">有问题，先去百度</a>
```

### 1.8.4. 按键修饰符

1) .keycode : 操作的是某个 keycode 值的键
2) .keyName : 操作的某个按键名的键(少部分)

### 1.8.5. 编码

```html
<div id="example">
    <h2>1. 绑定监听</h2>
    <button v-on:click="test1">Greet</button>
    <button @click="test1">Greet2</button>
    <button @click="test2($event, 'hello')">Greet3</button>
    <h2>2. 事件修饰符</h2>
    <!-- 阻止事件默认行为 -->
    <a href="http://www.baidu.com" @click.prevent="test3">百度一下</a>
    <br/>
    <br/>
    <!-- 停止事件冒泡 -->
    <div style="width: 200px;height: 200px;background: red" @click="test4">
        <div style="width: 100px;height: 100px;background: green"
             @click.stop="test5"></div>
    </div>
    <h2>3. 按键修饰符</h2>
    <input @keyup.8="test6">
    <input @keyup.enter="test6">
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue(el: '#example',
            data: {
            name: 'Vue.js'
            },
        methods: {
            test1 (event) {
                // 方法内 `this` 指向 vm
                // alert('Hello ' + this.name + '!')
                // `event` 是原生 DOM 事件
                alert(event.target.innerHTML)
            },
                test2 (event, msg) {
                    alert(event.target.innerHTML + '---' + msg)
                },
                    test3() {
                        alert('阻止事件的默认行为')
                    },
                        test4() {
                            alert('out')
                        },
                            test5() {
                                alert('inner')
                            },
                                test6(event) {
                                    alert(event.keyCode + '---' + event.target.value)
                                }
        }
    })
</script>
```

## 1.9. 表单输入绑定

### 1.9.2. 使用 v-model 对表单数据自动收集

1) text/textarea
2) checkbox
3) radio
4) select

### 1.9.3. 编码

```html
<div id="demo">
    <form @submit.prevent="handleSubmit">
        <span>用户名: </span>
        <input type="text" v-model="user.username"><br>
        <span>密码: </span>
        <input type="password" v-model="user.pwd"><br>
        <span>性别: </span>
        <input type="radio" id="female" value="female" v-model="user.sex">
        <label for="female">女</label>
        <input type="radio" id="male" value="male" v-model="user.sex">
        <label for="male">男</label><br>
        <span>爱好: </span>
        <input type="checkbox" id="basket" value="basketball"
               v-model="user.likes">
        <label for="basket">篮球</label>
        <input type="checkbox" id="foot" value="football"
               v-model="user.likes">
        <label for="foot">足球</label>
        <input type="checkbox" id="pingpang" value="pingpang"
               v-model="user.likes">
        <label for="pingpang">乒乓</label><br>
        <span>城市: </span>
        <select v-model="user.cityId">
            <option value="">未选择</option>
            <option v-for="city in allCitys" :value="city.id">
                {{ city.name }}
            </option>
        </select><br>
        <span>介绍: </span>
        <textarea v-model="user.desc" rows="10"></textarea><br><br>
        <input type="submit" value="注册">
    </form>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#demo',
        data: {
            user: {
                username: '',
                pwd: '',
                sex: 'female',
                likes: [],
                cityId: '',
                desc: '',
            },
            allCitys: [{id: 1, name: 'BJ'}, {id: 2, name: 'SZ'},{id: 4, name:
                                                                 'SH'}],
        },
        methods: {
            handleSubmit (event) {
                alert(JSON.stringify(this.user))
            }
        }
    })
</script>
```

## 1.10. Vue 实例生命周期

### 1.10.2. 生命周期流程图

![img](vue/lifecycle.png)

### [vue实例的生命周期](https://cn.vuejs.org/v2/guide/instance.html#实例生命周期)

- 什么是生命周期：从Vue实例创建、运行、到销毁期间，总是伴随着各种各样的事件，这些事件，统称为生命周期！
- [生命周期钩子](https://cn.vuejs.org/v2/api/#选项-生命周期钩子)：就是生命周期事件的别名而已；
- 生命周期钩子 = 生命周期函数 = 生命周期事件
- 主要的生命周期函数分类：
- 创建期间的生命周期函数：
- **beforeCreate**：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性
- **created**：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
- **beforeMount**：此时已经完成了模板的编译，但是还没有挂载到页面中
- **mounted**：此时，已经将编译好的模板，挂载到了页面指定的容器中显示
- 运行期间的生命周期函数：
- beforeUpdate：状态更新之前执行此函数， 此时 data 中的状态值是最新的，但是界面上显示的 数据还是旧的，因为此时还没有开始重新渲染DOM节点
- **updated：**实例更新完毕之后调用此函数，此时 data 中的状态值 和 界面上显示的数据，都已经完成了更新，界面已经被重新渲染好了！
- 销毁期间的生命周期函数：
- beforeDestroy：实例销毁之前调用。在这一步，实例仍然完全可用。
- destroyed：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。

### 1.10.3. vue 生命周期分析

1)初始化显示

beforeCreate()

created()

beforeMount()

mounted()

2)更新状态: this.xxx = value

beforeUpdate()
updated()

3)销毁 vue 实例: vm.$destory()

beforeDestory()
estoryed()

### 1.10.4. 常用的生命周期方法

1) created()/mounted(): 发送 ajax 请求, 启动定时器等异步任务
2) beforeDestory(): 做收尾工作, 如: 清除定时器

### 1.10.5. 编码

```html
<div>
    <button @click="destoryVue">destory vue</button>
    <p v-show="isShowing">{{msg}}</p>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    var vue = new Vue({
        el: 'div',
        data: {
            msg: '尚硅谷 IT 教育',
            isShowing: true,
            persons: []
        },
        beforeCreate () {
            console.log('beforeCreate() msg=' + this.msg)
            e.log('created() msg='+this.msg)
            this.intervalId = setInterval(() => {
                console.log('-----')
                this.isShowing = !this.isShowing
            }, 1000)
        },
        beforeMount () {
            console.log('beforeMount() msg='+this.msg)
        },
        mounted () {
            console.log('mounted() msg='+this.msg)
        },
        beforeUpdate() {
            console.log('beforeUpdate isShowing='+this.isShowing)
        },
        updated () {
            console.log('updated isShowing='+this.isShowing)
        },
        beforeDestroy () {
            console.log('beforeDestroy() msg='+this.msg)
            clearInterval(this.intervalId)
        },
        destroyed () {
            console.log('destroyed() msg='+this.msg)
        },
        methods: {
            destoryVue () {
                vue.$destroy()
            }
        }
    })
</script>
```

## 1.11. 过渡&动画  

### 1.11.2. vue 动画的理解 

1) 操作 css 的 trasition 或 animation
2) vue 会给目标元素添加/移除特定的 class
3) 过渡的相关类名
xxx-enter-active: 指定显示的 transition
xxx-leave-active: 指定隐藏的 transition
xxx-enter/xxx-leave-to: 指定隐藏时的样式 

![1569986009888](/vue/1569986009888.png)

### 1.11.3. 基本过渡动画的编码

1) 在目标元素外包裹`<transition name="xxx">`
2) 定义 class 样式
指定过渡样式: transition
指定隐藏时的样式: opacity/其它

### 1.11.4. 编码 1

```html
<style>
    .fade-enter-active, .fade-leave-active {
        transition: opacity .5s
    } .
    fade-enter, .fade-leave-to {
        opacity: 0
    } /
    * 可以设置不同的进入和离开动画 */
    .slide-fade-enter-active {
        transition: all .3s ease;
    } .
    slide-fade-leave-active {
        transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    } .
    slide-fade-enter, .slide-fade-leave-to {
        transform: translateX(10px);
        opacity: 0;
    }
</style>
<div id="demo1">
    <button @click="show = !show">
        Toggle1
    </button>
    <transition name="fade">
        <p v-if="show">hello</p>
    </transition>
</div>
<div id="demo2">
    <button @click="show = !show">
        Toggle2
    </button>
    <transition name="slide-fade">
        <p v-if="show">hello</p>
    </transition>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    new Vue({
        el: '#demo1',
        data: {
            show: true
        }
    })
    new Vue({
        el: '#demo2',
        data: {
            show: true
        }
    })
</script>
```

```html
1.11.5. 编码 2
<style>
    .bounce-enter-active {
        animation: bounce-in .5s;
    } .
    bounce-leave-active {
        animation: bounce-in .5s reverse;
    } @
        keyframes bounce-in {
            0% {
                transform: scale(0);
            } 
            50% {
                transform: scale(1.5);
            } 
            100% {
                transform: scale(1);
            }
    }
</style>
<div id="test2">
    <button @click="show = !show">Toggle show</button>
    <br>
    <transition name="bounce">
        <p v-if="show" style="display: inline-block">Look at me!</p>
    </transition>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script>
    new Vue({
        el: '#test2',
        data: {
            show: true
        }
    })
</script>
```

## 1.12. 过滤器

### 1.12.2. 理解过滤器

1) 功能: 对要显示的数据进行特定格式化后再显示
2) 注意: 并没有改变原本的数据, 可是产生新的对应的数据

### 1.12.3. 定义和使用过滤器

1) 定义过滤器

```js
Vue.filter(filterName, function(value[,arg1,arg2,...]){
    // 进行一定的数据处理
    return newValue
})
```

2)使用过滤器

```html
<div>{{myData | filterName}}</div>
<div>{{myData | filterName(arg)}}</div>
```

### 1.12.4. 编码 

```html
<div id="test">
    <p>当前时间为: {{currentTime}}</p>
    <p>当前时间 1 为: {{currentTime | dateStr}}</p>
    <p>当前时间 2 为: {{currentTime | dateStr('YYYY-MM-DD')}}</p>
    <p>当前时间 3 为: {{currentTime | dateStr('HH:mm:ss')}}</p>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript"
        src="https://cdn.bootcss.com/moment.js/2.19.0/moment.js"></script>
<script>
    // 注册过滤器
    Vue.filter('dateStr', function (value, format) {
        return moment(value).format(format || 'YYYY-MM-DD HH:mm:ss')
    })
    new Vue({
        el: '#test',
        data: {
            currentTime: new Date()
        }
    })
</script>
```

## 1.13. 内置指令与自定义指令
### 1.13.2. 常用内置指令
1) v:text : 更新元素的 textContent
2) v-html : 更新元素的 innerHTML
3) v-if : 如果为 true, 当前标签才会输出到页 面

4) v-else: 如果为 false, 当前标签才会输出到页面
5) v-show : 通过控制 display 样式来控制显示/隐藏
6) v-for : 遍历数组/对象
7) v-on : 绑定事件监听, 一般简写为@
8) v-bind : 强制绑定解析表达式, 可以省略 v-bind
9) v-model : 双向数据绑定
10) ref : 指定唯一标识, vue 对象通过$els 属性访问这个元素对象
11) v-cloak : 防止闪现, 与 css 配合: [v-cloak] { display: none } 

### 1.13.3. 自定义指令
1) 注册全局指令

```js
Vue.directive('my-directive', function(el, binding){
    el.innerHTML = binding.value.toupperCase()
})
```

2) 注册局部指令

```js
directives : {
    'my-directive' : {
        bind (el, binding) {
            el.innerHTML = binding.value.toupperCase()
        }
    }
}
```



3) 使用指令

```js
v-my-directive='xxx'
```



### 1.13.4. 编码 1(内置指令 ）

```html
<style>
    [v-cloak] {
        display: none
    }
</style>
<div id="example">
    <p v-text="url"></p>
    <p v-html="url"></p>
    <img :id="myid" :src="imageSrc">
    <p>
        <span ref="message">atguigu.com</span>
        <button @click="showMsg">显示左侧文本</button>
    </p>
    <p v-cloak>{{url}}</p>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    alert('模拟加载慢')
    new Vue({
        el: '#example',
        data: {
            url: '<a href="http://www.atguigu.com">尚硅谷</a>',
            myid: 'abc123',
            imageSrc: 'http://cn.vuejs.org/images/logo.png'
        },
        methods: {
            showMsg: function () {
                alert(this.$refs.message.textContent)
            }
        }
    })
</script>
```

### 1.13.5. 编码 2(自定义指令)

需求: 自定义 2 个指令

1. 功能类型于 v-text, 但转换为全大写
2. 功能类型于 v-text, 但转换为全小写
```html
<div id="demo1">
    <p v-upper-text="msg"></p>
    <p v-lower-text="msg"></p>
</div>
<div id="demo2"> p v-upper-text="msg2"></p>
<p v-lower-text="msg2"></p> <!--局部指令, 此处不能使用-->
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript">
    //注册全局指令
    Vue.directive('upper-text', function (el, binding) {
        el.innerHTML = binding.value.toUpperCase()
    })
    new Vue({
        el: '#demo1',
        data: {
            msg: 'NBA love this game!'
        },
        directives: { // 注册局部指令
            'lower-text': {
                bind (el, binding) {
                    el.innerHTML = binding.value.toLowerCase()
                }
            }
        }
    })
    new Vue({
        el: '#demo2',
        data: {
            msg2: 'I Like You'
        }
    })
</script
```

## 1.14. 自定义插件
### 1.14.2. 说明
1) Vue 插件是一个包含 install 方法的对象
2) 通过 install 方法给 Vue 或 Vue 实例添加方法, 定义全局指令等
### 1.14.3. 编码
1) 插件 JS

```js
/**
- 自定义 Vue 插件
  */
(function () {
    const MyPlugin = {}
    MyPlugin.install = function (Vue, options) {
        // 1. 添加全局方法或属性
        Vue.myGlobalMethod = function () {
            alert('Vue 函数对象方法执行')
        } //

        2. 添加全局资源
        Vue.directive('my-directive', function (el, binding) {
            el.innerHTML = "MyPlugin my-directive " + binding.value
        })
        // 3. 添加实例方法
        Vue.prototype.$myMethod = function () {
            alert('vue 实例对象方法执行')
        }
    }
    window.MyPlugin = MyPlugin
})()
```



2) 页面使用插件
```html
<div id="demo">
    <!--使用自定义指令-->
    <p v-my-directive="ms》g"></p>
</div>
<script type="text/javascript" src="../js/vue.js"></script>
<script type="text/javascript" src="vue-myPlugin.js"></script>
<script type="text/javascript">
    //使用自定义插件
    Vue.use(MyPlugin)
    var vm = new Vue({
        el: '#demo',
        data: {
            msg: 'atguigu'
        }
    })
    //调用自定义的静态方法
    Vue.myGlobalMethod()
    //调用自定义的对象方法
    vm.$myMethod()
</script> 
```

# 第 2 章： vue 组件化编码
## 2.1. 使用 vue-cli 创建模板项目
### 2.1.1. 说明
1) vue-cli 是 vue 官方提供的脚手架工具
2) github: https://github.com/vuejs/vue-cli
3) 作用: 从 https://github.com/vuejs-templates 下载模板项目 

### 2.1.2. 创建 vue 项目
```shell
npm install -g @vue/cli
```

## cli 2.x安装

**拉取 2.x 模板 (旧版本)**

Vue CLI >= 3 和旧版使用了相同的 `vue` 命令，所以 Vue CLI 2 (`vue-cli`) 被覆盖了。如果你仍然需要使用旧版本的 `vue init` 功能，你可以全局安装一个桥接工具：

```bash
npm install -g @vue/cli-init
```



```bash
vue init webpack vue_demo(project_name)
cd vue_demo
npm install
npm run dev
```

![1570005978113](vue/1570005978113.png)

访问: http://localhost:8080/ 

![1570456404361](vue/1570456404361.png)

### 2.1.3. 模板项目的结构

|-- build : webpack 相关的配置文件夹(基本不需要修改)
|-- dev-server.js : 通过 express 启动后台服务器
|-- config: webpack 相关的配置文件夹(基本不需要修改)
|-- index.js: 指定的后台服务的端口号和静态资源文件夹
|-- node_modules
|-- src : 源码文件夹
|-- components: vue 组件及其相关资源文件夹
|-- App.vue: 应用根主组件
|-- main.js: 应用入口 js
|-- static: 静态资源文件夹（**build打包原封不动的放到dist文件夹中**）
|-- .babelrc: babel 的配置文件
|-- .eslintignore: eslint 检查忽略的配置
|-- .eslintrc.js: eslint 检查的配置
|-- .gitignore: git 版本管制忽略的配置
|-- index.html: 主页面文件
|-- package.json: 应用包配置文件
|-- README.md: 应用描述说明的 readme 文件

**.editorconfig配置格式**

![1570455128364](vue/1570455128364.png)

## cli 3.x安装

![1570456721188](vue/1570456721188.png)

创建项目

```shell
vue create hello-world
```

![1570457528400](vue/1570457528400.png)

**在c:user/xxx/.vuerc里可以删除预设的vuecli配置**

rc-> run command

### vuecli3配置文件的查看和修改

```shell
vue ui
```

自定义配置文件vue.config.js

```js
// vue.config.js
module.exports = {
  // 选项...
}
```



## 2.2. 项目的打包与发布

### 2.2.1. 打包:
```shell
npm run build
```

![1570456377343](vue/1570456377343.png)

### 2.2.2. 发布 1: 使用静态服务器工具包

npm install -g serve
serve dist
访问: http://localhost:5000

### 2.2.3. 发布 2: 使用动态 web 服务器(tomcat)
修改配置: .config/index.js

```js
  build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: '/vue_demo',
  }
```

重新打包:
npm run build
修改 dist 文件夹为项目名称: xxx
将 xxx 拷贝到运行的 tomcat 的 webapps 目录下
访问: http://localhost:8080/xx 

## 2.3. eslint
### 2.3.1. 说明
1) ESLint 是一个代码规范检查工具
2) 它定义了很多特定的规则, 一旦你的代码违背了某一规则, eslint会作出非常有用的提示
3) 官网: http://eslint.org/
4) 基本已替代以前的 JSLint
### 2.3.2. ESLint 提供以下支持
1) ES
2) JSX
3) style 检查
4) 自定义错误和提示
### 2.3.3. ESLint 提供以下几种校验
1) 语法错误校验
2) 不重要或丢失的标点符号， 如分号
3) 没法运行到的代码块（使用过 WebStorm 的童鞋应该了解）
4) 未被使用的参数提醒
5) 确保样式的统一规则， 如 sass 或者 less
6) 检查变量的命名
### 2.3.4. 规则的错误等级有三种
1) 0： 关闭规则。
2) 1： 打开规则， 并且作为一个警告（信息打印黄色字体）
3) 2： 打开规则， 并且作为一个错误（信息打印红色字体 )

### 2.3.5. 相关配置文件
1) .eslintrc.js : 全局规则配置文件
'rules': {
'no-new': 1
}
2) 在 js/vue 文件中修改局部规则
/* eslint-disable no-new */
new Vue({
el: 'body',
components: { App }
})
3) .eslintignore: 指令检查忽略的文件
*.js
*.vu 

## 2.4. 组件定义与使用
### 2.4.1. vue 文件的组成(3 个部分)
1) 模板页面

```html
<template>
页面模板
</template>
```
2) JS 模块对象
```js
<script>
    export default {
	data() {return {}},
    	methods: {},
        computed: {},
        components: {}
	}
</script>
```
3) 样式

```css
<style> 

样式定义
</style>
```

### 2.4.2. 基本使用
1) 引入组件
2) 映射成标签
3) 使用组件标签

```html
<template>
    <HelloWorld></HelloWorld>
    <hello-world></hello-world>
</template>
<script>
    import HelloWorld from './components/HelloWorld'
    export default {
        components: {
            HelloWorld
        }
    }
</script>
```

2.4.3. 关于标签名与标签属性名书写问题
1) 写法一: 一模一样
2) 写法二: 大写变小写, 并用-连接 

## 2.5. 组件间通信
### 2.5.1. 组件间通信基本原则
1) 不要在子组件中直接修改父组件的状态数据
2) 数据在哪, 更新数据的行为(函数)就应该定义在哪
### 2.5.2. vue 组件间通信方式
1) props
2) vue 的自定义事件
3) 消息订阅与发布(如: pubsub 库)
4) slot
5) vuex(后面单独讲)

## 2.6. 组件间通信 1: props
2.6.1. 使用组件标签时

```html
<my-component name='tom' :age='3' :set-name='setName'></my-component>
```

### 2.6.2. 定义 MyComponent 时
1) 在组件内声明所有的 props
2) 方式一: **只指定名称(类型要为字符串)**

```js
props: ['name', 'age', 'setName']
```

3) 方式二: 指定名称和类型

```json
props: {
    name: String,
    age: Number,
    setNmae: Function
}
```

4) 方式三: 指定名称/类型/必要性/默认值

```js
props: {
name: {type: String, required: true, default:xxx},
} 
```

### 2.6.3. 注意
1) 此方式用于父组件向子组件传递数据
2) 所有标签属性都会成为组件对象的属性, 模板页面可以直接引用
3) 问题:
a. 如果需要向非子后代传递数据必须多层逐层传递
b. 兄弟组件间也不能直接 props 通信, 必须借助父组件才可以

## 2.7. 组件间通信 2: vue 自定义事件 

## 子组件向父组件传值

1. 原理：父组件将方法的引用，传递到子组件内部，子组件在内部调用父组件传递过来的方法，同时把要发送给父组件的数据，在调用方法的时候当作参数传递进去；
2. **父组件将方法的引用传递给子组件，其中，`getMsg`是父组件中`methods`中定义的方法名称，`func`是子组件调用传递过来方法时候的方法名称**

```html
<son @func="getMsg"></son>
```

3. 子组件内部通过`this.$emit('方法名', 要传递的数据)`方式，来调用父组件中的方法，同时把数据传递给父组件使用

```js
<div id="app">
    <!-- 引用父组件 -->
    <son @func="getMsg"></son>

    <!-- 组件模板定义 -->
    <script type="x-template" id="son">
      <div>
        <input type="button" value="向父组件传值" @click="sendMsg" />
      </div>
    </script>
  </div>

  <script>
    // 子组件的定义方式
    Vue.component('son', {
      template: '#son', // 组件模板Id
      methods: {
        sendMsg() { // 按钮的点击事件
          this.$emit('func', 'OK'); // 调用父组件传递过来的方法，同时把数据传递出去
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getMsg(val){ // 子组件中，通过 this.$emit() 实际调用的方法，在此进行定义
          alert(val);
        }
      }
    });
  </script>
```

## 使用 `this.$refs` 来获取元素和组件

```js
  <div id="app">
    <div>
      <input type="button" value="获取元素内容" @click="getElement" />
      <!-- 使用 ref 获取元素 -->
      <h1 ref="myh1">这是一个大大的H1</h1>

      <hr>
      <!-- 使用 ref 获取子组件 -->
      <my-com ref="mycom"></my-com>
    </div>
  </div>

  <script>
    Vue.component('my-com', {
      template: '<h5>这是一个子组件</h5>',
      data() {
        return {
          name: '子组件'
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      data: {},
      methods: {
        getElement() {
          // 通过 this.$refs 来获取元素
          console.log(this.$refs.myh1.innerText);
          // 通过 this.$refs 来获取组件
          console.log(this.$refs.mycom.name);
        }
      }
    });
  </script>
```



### 2.7.1. 绑定事件监听

使用 `this.$refs` 来获取元素和组件

```html
  <div id="app">
    <div>
      <input type="button" value="获取元素内容" @click="getElement" />
      <!-- 使用 ref 获取元素 -->
      <h1 ref="myh1">这是一个大大的H1</h1>
      <hr>
      <!-- 使用 ref 获取子组件 -->
      <my-com ref="mycom"></my-com>
    </div>
  </div>

  <script>
    Vue.component('my-com', {
      template: '<h5>这是一个子组件</h5>',
      data() {
        return {
          name: '子组件'
        }
      }
    });

    // 创建 Vue 实例，得到 ViewModel
    var vm = new Vue({
      el: '#app',
      methods: {
        getElement() {
          // 通过 this.$refs 来获取元素
          console.log(this.$refs.myh1.innerText);
          // 通过 this.$refs 来获取组件
          console.log(this.$refs.mycom.name);
        }
      }
    });
  </script>
```



```js
// 方式一: 通过 v-on 绑定
@delete_todo="deleteTodo"
// 方式二: 通过$on()
this.$refs.xxx.$on('delete_todo', function (todo) {
    this.deleteTodo(todo)
})
```

### 2.7.2. 触发事件
```js
// 触发事件(只能在父组件中接收)
this.$emit(eventName, data)
```



### 2.7.3. 注意:
1) 此方式只用于子组件向父组件发送消息(数据)
2) 问题: **隔代组件或兄弟组件间通信此种方式不合适** 

## 2.8. 组件间通信 3: 消息订阅与发布(PubSubJS 库)
```shell
npm install pubsub-js
```



```js
import PubSub from 'pubsub-js'
```

2.8.1. 订阅消息

```js
PubSub.subscribe('msg', function(msg, data){})
```

2.8.2. 发布消息

```js
PubSub.publish('msg', data)
```

2.8.3. 注意
1) 优点: **此方式可实现任意关系组件间通信(数据)**
2.8.4. 事件的 2 个重要操作(总结)
1) 绑定事件监听 (订阅消息)
目标: 标签元素 `<button>`
事件名(类型): click/focus
回调函数: function(event){}
2) 触发事件 (发布消息)
DOM 事件: 用户在浏览器上对应的界面上做对应的操作
自定义: 编码手动触发

## 组件间通信 插槽4: slot
2.9.1. 理解
此方式用于父组件向子组件传递`标签数据`

### [编译作用域](https://cn.vuejs.org/v2/guide/components-slots.html#编译作用域)

父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

2.9.2. 子组件: 

**Child.vue**

```html
<template>
    <div>
        <slot name="xxx">不确定的标签结构 1</slot>
		<div>组件确定的标签结构</div>
		<slot name="yyy">不确定的标签结构 2</slot>
	</div>
</template>
```



2.9.3. 父组件: Parent.vue

```html
<child>
    <div slot="xxx">xxx 对应的标签结构</div>
    <div slot="yyy">yyy 对应的标签结构</div>
</child> 
```

## [自定义组件的 `v-model`](https://cn.vuejs.org/v2/guide/components-custom-events.html#自定义组件的-v-model)

一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` 特性用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突：

```js
Vue.component('base-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean
  },
  template: `
    <input
      type="checkbox"
      v-bind:checked="checked"
      v-on:change="$emit('change', $event.target.checked)"
    >
  `
})
```

现在在这个组件上使用 `v-model` 的时候：

```html
<base-checkbox v-model="lovingVue"></base-checkbox>
```

这里的 `lovingVue` 的值将会传入这个名为 `checked` 的 prop。同时当 `<base-checkbox>` 触发一个 `change` 事件并附带一个新的值的时候，这个 `lovingVue` 的属性将会被更新。

注意你仍然需要在组件的 `props` 选项里声明 `checked` 这个 prop。

# 第 3 章： vue-ajax 

## 3.1. vue 项目中常用的 2 个 ajax 库
### 3.1.1. vue-resource
vue 插件, 非官方库, vue1.x 使用广泛

### 3.1.2. axios
通用的 ajax 请求库, 官方推荐, vue2.x 使用广泛
## 3.2. vue-resource 的使用
3.2.1. 在线文档
https://github.com/pagekit/vue-resource/blob/develop/docs/http.md
3.2.2. 下载

```shell
npm install vue-resource --save
```

3.2.3. 编码

```js
// 引入模块
import VueResource from 'vue-resource'
// 使用插件
Vue.use(VueResource)
// 通过 vue/组件对象发送 ajax 请求
this.$http.get('/someUrl').then((response) => {
// success callback
console.log(response.data) //返回结果数据
}, (response) => {
// error callbacb

console.log(response.statusText) //错误信息
}) 
```

## 3.3. axios 的使用
3.3.1. 效果
ajax_test.gif
3.2. 在线文档
https://github.com/pagekit/vue-resource/blob/develop/docs/http.md
3.3. 下载:

```shell
npm install axios --save
```

3.4. 编码

```js
// 引入模块
import axios from 'axios'
// 发送 ajax 请求
axios.get(url)
.then(response => {
console.log(response.data) // 得到返回结果数据
})
.catch(error => {
console.log(error.message)
} )
```

# 第 4 章： vue UI 组件库

## 4.1. 常用
1) Mint UI:
a. 主页: http://mint-ui.github.io/#!/zh-cn
b. 说明: 饿了么开源的基于 vue 的移动端 UI 组件库
2) Elment
a. 主页: http://element-cn.eleme.io/#/zh-CN
b. 说明: 饿了么开源的基于 vue 的 PC 端 UI 组件库
4.2. 使用 Mint UI
4.2.1. 下载: 

```shell
npm install --save mint-ui 
```



4.2.2. 实现按需打包

1. 下载

  ```js
  npm install --save-dev babel-plugin-component
  ```

2.修改 babel 配置

  ```js
  "plugins": ["transform-runtime",["component", [
      {
          "libraryName": "mint-ui",
          "style": true
      }
  ]]]
  ```

## 4.2.3. mint-ui 组件分类

  1) 标签组件
  2) 非标签组件
  4.2.4. 使用 mint-ui 的组件

  1)index.html

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1,
                               minimum-scale=1, user-scalable=no" />
<script
        src="https://as.alipayobjects.com/g/component/fastclick/1.0.6/fastclick.js"></scrip
    t>
        <script>
        if ('addEventListener' in document) {
            document.addEventListener('DOMContentLoaded', function() {
                FastClick.attach(document.body);
            }, false);
        } i
        f(!window.Promise) {
            document.writeln('<script
                             src="https://as.alipayobjects.com/g/component/es6-promise/3.2.2/es6-promise.min.js"
                             '+'>'+'<'+'/'+'script>');
                             }
</script >
```

2) main.js
import {Button} from 'mint-ui'
Vue.component(Button.name, Button)
3) App.vue

```html
<template>
    <mt-button @click="handleClick" type="primary" style="width: 100%">Test</mt-button>
</template>
<script>
    import {Toast} from 'mint-ui'
    export default {
        methods: {
            handleClick () {
                Toast('点击了测试');
            }
        }
    }
</script> 
```

# 第 5 章： vue-router
## 5.1. 理解
5.1.1. 说明
1) 官方提供的用来实现 SPA 的 vue 插件
2) github: https://github.com/vuejs/vue-router
3) 中文文档: http://router.vuejs.org/zh-cn/
4) 下载: 

```shell
npm install vue-router --save 
```

5.1.2. 相关 API 说明
1)

```js
VueRouter(): 用于创建路由器的构建函数
new VueRouter({
    // 多个配置项
})
```

2) 路由配置

```js
routes: [
    { // 一般路由
        path: '/about',
        component: About
    },
    { // 自动跳转路由
        path: '/',
        redirect: '/about'
    }
]
```

3) 注册路由器

```js
import router from './router'
new Vue({
    router
})
```

4) 使用路由组件标签

```js
1. <router-link>: 用来生成路由链接
   <router-link to="/xxx">Go to XXX</router-link>
2. <router-view>: 用来显示当前路由组件界面
   <router-view></router-view>
```

### 编程式路由导航

1) this.$router.push(path): 相当于点击路由链接(可以返回到当前路由界面)

```js
router.push({ name: 'user', params: { userId: 123 }})
```

2) this.$router.replace(path): 用新路由替换当前路由(不可以返回到当前路由界面)
3) this.$router.back(): 请求(返回)上一个记录路由
4) this.$router.go(-1): 请求(返回)上一个记录路由
5) this.$router.go(1): 请求下一个记录路由 

## 5.2. 基本路由 

5.2.2. 路由组件
Home.vue
About.vue
5.2.3. 应用组件: App.vue

```html
<div>
    <!--路由链接-->
    <router-link to="/about">About</router-link>
    <router-link to="/home">Home</router-link>
    <!--用于渲染当前路由组件-->
    <router-view></router-view>
</div>
```

5.2.4. 路由器模块: src/router/index.js

```js
// 声明使用vue-router插件
/*
内部定义并注册了2个组件标签(router-link/router-view),
给组件对象添加了2个属性:
  1. $router: 路由器
  2. $route: 当前路由
 */
Vue.use(VueRouter)

export default new VueRouter({
    routes: [
        {
            path: '/',
            redirect: '/about'
        },
        {
            path: '/about',
            component: About
        },
        {
            path: '/home',
            component: Home
        }
    ]
}）
```

### 5.2.5. 注册路由器: main.js
```js
import Vue from 'vue'
import router from './router'
// 创建 vue 配置路由器
new Vue({
el: '#app',
router,
render: h => h(app)
})
```

5.2.6. 优化路由器配置
linkActiveClass: 'active', // 指定选中的路由链接的 class
5.2.7. 总结: 编写使用路由的 3 步
1) 定义路由组件
2) 注册路由
3) 使用路由

```js
<router-link>
<router-view> 
```

### 注意

注册VueRouter路由是的**属性不是routers是==routes==**

```js
Vue.use(VueRouter)

export default new VueRouter({
    routes: [
        {}
    ]
}）
```

## 5.3. 嵌套路由

### 5.3.2. 子路由组件
News.vue
Message.vue
### 5.3.3. 配置嵌套路由: router.js
```js
path: '/home',
    component: home,
        children: [
            {
                path: 'news',
                component: News
            },
            {
                path: 'message',
                component: Message
            }
        ]
```

### 5.3.4. 路由链接: Home.vue
```html
<router-link to="/home/news">News</router-link>
<router-link to="/home/message">Message</router-link>
<router-view></route-view 
```

## 5.4. 向路由组件传递数据 

### 5.4.2. 方式 1: 路由路径携带参数(param/query)
1) 配置路由

```js
children: [
    {
        path: 'mdetail/:id',
        component: MessageDetail
    }
]
```

2) 路由路径

```html
<router-link :to="'/home/message/mdetail/'+m.id">{{m.title}}</router-link>
```

3) 路由组件中读取请求参数

```js
this.$route.params.id

watch: {
    $route: function () { // 改变当前路由组件参数数据时自动调用
        console.log('$route()')
        const id = this.$route.params.id
        this.detail = messageDetails.find(detail => detail.id===id*1)
    }
}
```

### $route.query

- 类型: `Object`

  一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象。

### 5.4.3. 方式 2: \<router-view>属性携带数据

```html
<router-view :msg="msg"></router-view>
```

## 5.5. 缓存路由组件对象
5.5.1. 理解
1) 默认情况下, 被切换的路由组件对象会死亡释放, 再次回来时是重新创建的
2) 如果可以缓存路由组件对象, 可以提高用户体验
5.5.2. 编码实现

```html
<keep-alive>
<router-view></router-view>
</keep-alive>
```

## 实现跳转时判断是否登录

main.js

```js
router.beforeEach((to, from, next) => {
  if (to.meta.title) {
    document.title = to.meta.title
  }
  const flag = to.meta.withOutLogin
  console.log(flag)
  console.log(store.state.user)
  // 判断该路由是否需要登录权限
  if (!flag && !store.state.user) {
    request({
      url: "/user/info",
      method: "GET"
    })
    .then(res => {
      console.log(res);
      if(res.data.success){
        next()
        const user = res.data.extend.user;
        store.commit(RECEIVE_USER, {
          user
        })
      }else{
        next('/login')
      }
    })
    .catch(err => {
      console.log(err);
    });
  } else {
    next()  // 确保一定要有next()被调用
  }
})
```



## 5.7 reoute.routes里的meta属性

**meta能给每个route指定附加值.**

可以实现通过路径判断是否显示组件

```js
{
    path: '/msite',
    component: Msite,
        meta: {
            showFooter: true
        }
}
```

```
<FooterGuide v-show="$route.meta.showFooter"></FooterGuide>
```

# 第 6 章： vuex

## 6.1. vuex 理解
6.1.1. vuex 是什么
1) github 站点: https://github.com/vuejs/vuex
2) 在线文档: https://vuex.vuejs.org/zh-cn/
3) 简单来说: 对 vue 应用中多个组件的共享状态进行集中式的管理(读/写)
6.1.2. 状态自管理应用
1) state: 驱动应用的数据源

2) view: 以声明方式将 state 映射到视图
3) actions: 响应在 view 上的用户输入导致的状态变化(包含 n 个更新状态的方法) 

![1570107505633](vue/1570107505633.png)

6.1.3. 多组件共享状态的问题
1) 多个视图依赖于同一状态
2) 来自不同视图的行为需要变更同一状态
3) 以前的解决办法
a. 将数据以及操作数据的行为都定义在父组件
b. 将数据以及操作数据的行为传递给需要的各个子组件(有可能需要多级传递)
4) vuex 就是用来解决这个问题的
## 6.2. vuex 核心概念和 API
### 6.2.1. state
1) vuex 管理的状态对像

2) 它应该是唯一的

```js
const state = {
    xxx: initValue
}
```



```js
import { mapState, mapActions } from "vuex";
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```



### 6.2.2. mutations

1) 包含多个直接更新 state 的方法(回调函数)的对象
2) 谁来触发: action 中的 commit('mutation 名称')
3) 只能包含同步的代码, 不能写异步代码

```js
const mutations = {
    yyy (state, {data1}) {
        // 更新 state 的某个属性
    }
}
```

### 6.2.3. actions
1) 包含多个事件回调函数的对象
2) 通过执行: commit()来触发 mutation 的调用, 间接更新 state
3) 谁来触发: 组件中: $store.dispatch('action 名称', data1) // 'zzz'
4) 可以包含异步代码(定时器, ajax)

```js
const actions = {
    zzz ({commit, state}, data1) {
        commit('yyy', {data1})
    }
}
```

### 6.2.4. getters
1) 包含**多个计算属性(get)的对象**
2) 谁来读取: 组件中: $store.getters.xxx

```js
const getters = {
    mmm (state) {
        return .. 

    }
}
```

### 6.2.5. modules
1) 包含多个 module
2) 一个 module 是一个 store 的配置对象
3) 与一个组件(包含有共享数据)对应

### 6.2.6. 向外暴露 store 对象

```js
export default new Vuex.Store({
    state,
    mutations,
    actions,
    getters
})
```

### 6.2.7. 组件中

```js
import {mapState, mapGetters, mapActions} from 'vuex'
export default {
    computed: {
        ...mapState(['xxx']),
        ...mapGetters(['mmm']),
    } m
    ethods: mapActions(['zzz'])
} {
    {xxx}} {{mmm}} @click="zzz(data)"
```



### 6.2.8. 映射 store
```js
import store from './store'
new Vue({
    store
} 
```

### 6.2.9. store 对象
1) 所有用 vuex 管理的组件中都**多了一个属性$store, 它就是一个 store 对象**
2) 属性:

```js
state: 注册的 state 对象
getters: 注册的 getters 对象
```

3) 方法:
dispatch(actionName, data): 分发调用 action 





## 6.3使用案例

**使用时安装**

```shell
npm install --save vuex
```

## 6.3. demo1: 计数器 

### 5.3.1 定义store模块

store.js

```js
/**
* vuex 的 store 对象模块
*/
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
/*
state 对象
类似于 data
*/
const state = {
    count: 0 // 初始化状态数据
} 
/* mutations 对象
包含个方法: 能直接更新 state
一个方法就是一个 mutation
mutation 只能包含更新 state 的同步代码, 也不会有逻辑
mutation 由 action 触发调用: commit('mutationName')
    */
const mutations = {
    INCREMENT(state) {
        state.count++
    },
    DECREMENT (state) { // ctrl + shift + x
        state.count--
    }
}
/*
      actions 对象
      包含个方法: 触发 mutation 调用, 间接更新 state
      一个方法就是一个 action
      action 中可以有逻辑代码和异步代码
      action 由组件来触发调用: this.$store.dispatch('actionName')
          */
const actions = {
    increment ({commit}) {
        commit('INCREMENT')
    },
    decrement ({commit}) {
        commit('DECREMENT')
    },
    incrementIfOdd ({commit, state}) {
        if(state.count%2===1) {
            commit('INCREMENT')
        }
    },
    incrementAsync ({commit}) {
        setTimeout(() => {
            commit('INCREMENT')
        }, 1000)
    }
}
/*
getters 对象包含多个 get 计算计算属性方法
*/
const getters = {
    oddOrEven (state) {
        return state.count%2===0 ? '偶数' : '奇数'
    },
    count (state) {
        return state.count
    }
} //
向外暴露 store 实例对象
export default new Vuex.Store({
    state,
    mutations,
    actions,
    getters
}）
```

### 6.3.2 在vue中注册

 main.js  

```js
import Vue from 'vue'
import app from './app1.vue'
// import app from './app.vue'
import store from './store'
new Vue({
    el: '#app',
    render: h => h(app),
    store // 所有组件都多个一个属性: $store
})
```

### 6.3.3. app.vue(未优化前) 

```js
<template>
    <div>
    <p>clicked: {{$store.state.count}} times, count is {{oddOrEven}}</p>
<button @click="increment">+</button>
<button @click="decrement">-</button>
<button @click="incrementIfOdd">increment if odd</button>
<button @click="incrementAsync">increment async</button>
</div>
</template>
<script>
    export default {
computed: {
    oddOrEven () {
        return this.$store.getters.oddOrEven
    }
},
    methods: {
        increment () {
            this.$store.dispatch('increment')
        },
            decrement () {
                this.$store.dispatch('decrement')
            },
                incrementIfOdd () {
                    this.$store.dispatch('incrementIfOdd')
                },
                    incrementAsync () {
                        this.$store.dispatch('incrementAsync')
                    }
    }
}
    </script>
<style>
        </style>
```

### 6.3.4. app2.vue(优化后) 

 ```html
<template>
    <div>
        <p>clicked: {{count}} times, count is {{oddOrEven2}}</p>
        <button @click="increment">+</button>
        <button @click="decrement">-</button>
        <button @click="incrementIfOdd">increment if odd</button>
        <button @click="incrementAsync">increment async</button>
    </div>
</template>
<script>
    import {mapGetters, mapActions} from 'vuex'
    export default {
        computed: mapGetters({ // 名称不一样
            oddOrEven2: 'oddOrEven',
            count: 'count'
        }),
        methods: mapActions(['increment', 'decrement', 'incrementIfOdd',
                             'incrementAsync']) // 名称一样
    }
</script>
<style>
</style>
 ```

## 6.5. vuex 结构分析 

![vuex结构图](vue/vuex结构图.png)

## 使用注意

**只有当前组件使用的数据不需要使用vuex管理**

# vue使用-me

## 使用bootstarp

1安装

需要安装`jquery`,因为`bootstrap`是依赖于`jquery`的。我们同样使用npm安装`jquery`。bootstrap也需要安装`popper.js`

```shell
npm install --save jquery
npm install --save popper.js
npm install --save bootstrap@4
```

2.在 **vue.config.js** 文件写入以下内容：

```js
/*
*Vue-CLI项目的核心配置文件
*/
const webpack = require("webpack");

module.exports = {
 configureWebpack: {
   plugins: [
     new webpack.ProvidePlugin({
       $: "jquery",
       jQuery: "jquery",
       "window.jQuery": "jquery",
       Popper: ["popper.js", "default"]
     })
   ]
 }
};
```



2引入

```js
//使用bootstarp
import $ from 'jquery'
import "bootstrap/dist/css/bootstrap.css";
import "bootstrap/dist/js/bootstrap.min.js";
new Vue({
  $,
  render: h => h(App),
}).$mount('#app')
```

## 使用 Font Awesome

Font Awesome
：http://fontawesome.dashgame.com/

npm 安装 font-awesome

```shell
npm install font-awesome --save-dev
```



1
font-awesome引入
在 mani.js 文件中引入 font-awesome

```js
import 'font-awesome/css/font-awesome.css' 
```

or

```js
import 'font-awesome/scss/font-awesome.scss'
```



1
然后在组件中使用即可实现：

```html
 <span class="fa fa-camera-retro fa-lg"></span>
```









## [vue-cli 3.x配置跨域代理](https://segmentfault.com/a/1190000014474361)

```js
// vue.config.js
module.exports = {
    // 修改的配置
    // 将baseUrl: '/api',改为baseUrl: '/',
    baseUrl: '/',
    devServer: {
        proxy: {
            '/api': {
                target: 'http://www.example.org',
                changeOrigin: true,
                ws: true,
                pathRewrite: {
                  '^/api': ''
                }
            }
        }
    }
}
// .env.development
VUE_APP_BASE_API=/api
```



# vue项目使用.env文件配置全局环境变量

[官方文档](https://cli.vuejs.org/zh/guide/mode-and-env.html#模式)

关于文件名：必须以如下方式命名，不要乱起名，也无需专门手动控制加载哪个文件

.env 全局默认配置文件，不论什么环境都会加载合并

.env.development 开发环境下的配置文件

.env.production 生产环境下的配置文件

关于文件名：必须以如下方式命名，不要乱起名，也无需专门手动控制加载哪个文件

.env 全局默认配置文件，不论什么环境都会加载合并

.env.development 开发环境下的配置文件

.env.production 生产环境下的配置文件



# 其他