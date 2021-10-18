[TOC]

# 常用语法

**展示目录(table of contents)结构**

```mrakdown
 [toc]
```

链接 和html一样能使用相对路径

```markdown
[对象传递和返回](./Appendix-Passing-and-Returning-Objects.md)
[百度](www.baidu.com)
```

 **多级标题**

```markdown
一级标题 
# + space
二级 
## +space 
三级
### + ...
```

## 列表

有序列表

1. 你好 1.space
2. 



- 你好 -+space
- 得到

## insert code ```+type

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("hello ");
    }
}
```

# other

## 使用notepad++增加markdown换行

typora产生的markdown文件通常没有换行`  `,及两个空格，

可以使用notepad++或者word、进行替换，将`\r\n`替换为`  两个空格+\r\n`

## markdown如何跳转到文本内的指定位置

知道用的是`typora`就好办了，有官方的说明：

[https://support.typora.io/Lin...](https://support.typora.io/Links/#faq)

```
# title

...
...
...


[link](#title) jump to title
```

注：要按住`ctrl`点击那个`link`

也可参考
[Typora中实现内部跳转](https://www.jianshu.com/p/c4d28c3f69ac)

# pdf转markdown

**pdf -> word -> typora导入word文件**，图片自己解决

# 三. 页面内跳转

MarkDown中是通过定义链接的方式来定义跳转的，在这里，跳转也称为`锚点`，跳转的目标称为`锚点目标`；

所以，在 MarkDown 中实现页面内跳转的方法就是：定义一个 `锚点目标` 和 对应的 `锚点` ，用户点击 `锚点` 便可跳转到对应的 `锚点目标` 位置处；

`锚点` 和 `锚点目标` 的定义格式如下：

## 1. 锚点的定义

锚点就是一个链接，另外，由于在MarkDown中可以直接写HTML，所以在MarkDown中实现锚点有两种方式：MarkDown方式 和 HTML方式；

### 1.1 MarkDown锚点

MarkDown锚点本质上就是一个[MarkDown链接](#链接的定义)，只是链接地址的格式为：



```bash
链接地址 = #目标内容
```

所以 MarkDown锚点 的定义也有两种方式

#### 行内式

**语法：**



```objectivec
锚点 = [内容](#目标内容 "标题")
```

**说明：**

- `标题` 是可选的，可以用单引号 或 双引号；

**转换成HTML后，会生成如下标签：**



```xml
<a href="#目标内容" title="标题">内容</a>
```

**示例：**



```csharp
[MarkDown方式的锚点](#MarkDown锚点)
```

**渲染成HTML后，会生成如下标签：**



```xml
<a href="#MarkDown锚点" >MarkDown方式的锚点</a>
```

**效果如下：点击下面的锚点**

[MarkDown方式的锚点](###1.1 MarkDown锚点)

作者：科研者
链接：https://www.jianshu.com/p/4898c2e9a36d

