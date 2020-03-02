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

