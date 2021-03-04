---
title: hexo
date: 2019-9-27 22:19:09
tags: 学习笔记
---
[TOC]

[hexo官方文档](https://hexo.io/zh-cn/docs/setup)

# 安装hexo

npm安装hexo

```undefined
npm install -g hexo-cli
```

注意找到hexo.cmd的目录，构建环境变量

如：E:\Program Files\nodejs\my_node\node_global\hexo.cmd

## 开始构建

条件 npm+git

配置Deployment，在其文件夹中，找到_config.yml文件，修改repo值（在末尾）

```yaml
deploy:
  type: git
  repository: git@github.com:Li-MingZhong/Li-MingZhong.github.io.git(自己仓库的ssh链接)
  branch: master
```

在生成以及部署文章之前，需要安装一个扩展：

```shell
npm install hexo-deployer-git --save
```

新建一篇博客，在cmd执行命令：hexo new post “博客名”

使用编辑器编好文章，那么就可以使用命令**：hexo d -g，生成以及部署(即自动提交）了**

```shell
hexo d -g
```

然后提交

# 使用

启动

```shell
hexo s
hexo serve
```



**在 source 文件夹下的所有 md 文件或者 html 文件都会被渲染**

在sourece/_post目录下相应的md添加相应的头信息由hexo解析。

```markdown
---

title: mybatis(网页显示和文章标题)

p: java-note/mybatis/mybatis
（java-note/mybatis/mybatis.md(省略了.md)在_post路径下的位置，需要创建对于文件夹）,不需要p参数也能被解析

date: 2019-11-12 10:40:06 (创建时间)

categories: java 

tags: [java,mybatis]

---
```

```markdown
---

title: mybatis

date: 2019-11-12 10:40:06

categories: java

tags: [java,mybatis]

---
```



# [命令][https://hexo.io/zh-cn/docs/commands]

## new

```shell
hexo n -p js-note/vue/vue 'vue'
```

能挂载图片和创建路径

```
$ hexo new [layout] <title>
```

新建一篇文章。如果没有设置 `layout` 的话，默认使用 [_config.yml](https://hexo.io/zh-cn/docs/configuration) 中的 `default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。

```
$ hexo new "post title with whitespace"
```

| 参数              | 描述                                          |
| :---------------- | :-------------------------------------------- |
| `-p`, `--path`    | 自定义新文章的路径                            |
| `-r`, `--replace` | 如果存在同名文章，将其替换                    |
| `-s`, `--slug`    | 文章的 Slug，作为新文章的文件名和发布后的 URL |

默认情况下，Hexo 会使用文章的标题来决定文章文件的路径。对于独立页面来说，Hexo 会创建一个以标题为名字的目录，并在目录中放置一个 `index.md` 文件。**你可以使用 `--path` 参数来覆盖上述行为、自行决定文件的目录：**

```
hexo new page --path about/me "About me"
```

**以上命令会创建一个 `source/about/me.md` 文件，同时 Front Matter 中的 title 为 `"About me"`**

注意！title 是必须指定的！如果你这么做并不能达到你的目的：

```
hexo new page --path about/me
```

此时 Hexo 会创建 `source/_posts/about/me.md`，同时 `me.md` 的 Front Matter 中的 title 为 `"page"`。这是因为在上述命令中，hexo-cli 将 `page` 视为指定文章的标题、并采用默认的 `layout`。

# 解决markdown插入图片问题

`hexo`默认无法自动处理文章插入本地图片，需要通过扩展插件支持。

## 图片路径问题

1.配置`_config.yml`里面的`post_asset_folder:false`这个选项设置为`true`。

2.安装[hexo-asset-image](https://github.com/7ym0n/hexo-asset-image.git)

```shell
npm install hexo-asset-image --save
```

3.运行`hexo n "xxxx"`来生成md博文，`/source/_posts`文件夹内除了xxxx.md文件还有一个同名的文件夹，把图片放入该文件夹。

```shell
hexo n "xxxx"
```

4.使用`![xxx](xxx/xxx.png)`直接插入图片即可。

### 注意

1.**图片名中间不能有空格**

# hexo设置多标签和多分类

分类↓↓↓↓
categories: 一级分类-二级分类 

例如 categories: java-SpringMVC
标签↓↓↓↓
tags: [标签1,标签2，...] 

例如 tags: [java,SpringMVC]

# hexo clean

```shell
PS E:\Codes\other\hexo\my-blog> hexo clean
INFO  Deleted database.
```

hexo会记录以前的配置信息和tags之类的，所以，清楚标签之后，hexo clear一下

## 设置跳过的渲染文件

skip_render:

```shell
skip_render: [README.md, "**/*.js","**/source/**","**/other/**","**/src/**"]
```



## markdwon解析错误

1.一个`# `后面没有内容，解析失败



## next主题

[]: 

[next主题优化 ](https://blog.csdn.net/nightmare_dimple/article/details/86661502) 

## 4.添加动态背景

## 5. 修改标签样式

## 9.文章阴影设置

## 10. 添加文章版权信息

## 12. 添加打赏

## 13. 添加页面宠物

## 18.增加阅读次数/时长和访客数

## 19.加入网易云音乐播放器