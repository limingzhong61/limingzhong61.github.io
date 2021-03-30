---
title: git
date: 2019-10-13 22:19:09
tags: 学习笔记
---
[TOC]

特点：

分布式版本控制系统
优势：
	SVN在开发过程中必须保证需要与SVN服务器连接才能使用，需要联网

[git官方文档](https://git-scm.com/docs/git-checkout)

# 基础操作

## 1、设置全局信息

```shell
设置开发者的用户名
	git config --global user.name nicolas
设置开发者邮箱
	git config --global user.email 1162314270@qq.com
取得全部的全局信息
	git config --list
	git config -l
```

## 2、创建仓库

1、创建目录当做仓库目录
2、进入目录中配置
将目录变为仓库目录，需要进入目录中配置
3、初始化仓库
方法一：创建一个包含有配置信息目录的仓库文件夹

```shell
git init
Initialized empty Git repository in E:/gitpro/.git/
```

## 文件区域

1. **工作区**
   用户编写代码的文件夹
   所有的文件操作以工作区为主
   GIT仓库

2.  **暂存库**
   只是将工作中的未保存文件保存到暂存区中，有GIT维护；
   保存的内容是暂时需要的

3. **版本库Master**

   真正发布的代码

## 3、仓库基本操作

1、观察仓库的状态

```shell
git status
```

2、将文件添加到git暂存区

```shell
git add 文件名称
```

4、批量添加新文件到暂存区

```shell
git add .
```

添加所有在仓库目录中创建的新文件到暂存区
3、将文件提交到版本库中

```shell
git commit -m "注释信息Comment"
```

现在才表示将新的文件提交到了GIT之中进行管理（这个代码才能被项目使用）
**自动增加并提交修改到版本库中**

```shell
git commit -a -m "注释Comment"
```

4、修改仓库文件
	



​	查看修改历史

```shell
git log 文件名
git log
```

​		每次提交时都会自动生成一个Commit ID（在日后版本恢复中使用）

### 比较同一文件

```shell
git diff
git diff 文件名
```

比对暂存区和工作区中readme.txt文件的差异

### 版本穿越

​	查看日志信息

```shell
git log
git log --pretty=oneline
```

简化查看
可以按q结束查看
整个日志记录是用栈来记录的
master库存在head指针
回退到上一个版本

```shell
git reset --hard head~1
```

查看所有删除的日志

```shell
git reflog
```

​	到指定版本

```shell
git reset --hard 1fc9d3f（Commit id）
```

### 撤销修改

#### 撤销工作区修改

显示那些文件发生了改变

```shell
git checkout
```

恢复单个文件

```shell
git checkout 文件名
```

恢复多个文件

```shell
git checkout .
```

#### 修改已在暂存区

将暂存区的文件撤回到工作区

```shell
git reset HEAD <file>...
```

将暂存区的文件撤回到工作区 :多个文件

```
git reset HEAD .
```

文件删除

```shell
del 文件名
```

删除工作区中的文件
恢复

```shell
git checkout -- 文件名
```

​			因为版本库中任存在，可以直接删除
​	git commit -a -m "Commet"
​		提交之后，库中相应的文件也没有了
​		只能通过版本穿越恢复



远程仓库：GITHUB ...

[git-book:github章节](https://git-scm.com/book/zh/v2/GitHub-账户的创建和配置)

# 远程仓库的使用

[git-book:Git-基础-远程仓库的使用](https://git-scm.com/book/zh/v2/Git-基础-远程仓库的使用)

### problem

**github每次push都要密码**

原因是使用了https方式 push 
这种方式产生的原因，一般是我们clone是一定是使用了http的方式 
例如我们在github上创建了一个项目，然后我们clone到本地时使用了http而非git (ssh)

```cmd
git remote rm origin
git remote add origin git@github.com:codeOflI/MyDouban.git
```



### 查看远程仓库

```shell
git remote [-v]
```

选项 `-v`，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。

### 添加远程仓库

`git remote add <shortname> <url>` 添加一个新的远程 Git 仓库

```shell
git remote add origin https://github.com/codeOflI/rep.git
```

### 从远程仓库中抓取与拉取

这个命令会访问远程仓库，从中拉取所有你还没有的数据。

```console
git fetch [remote-name]
```

如果你使用 `clone` 命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以 “origin” 为简写。 所以，`git fetch origin` 会抓取克隆（或上一次抓取）后新推送的所有工作。 必须注意 `git fetch` 命令会将数据拉取到你的本地仓库——它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

如果你有一个分支设置为跟踪一个远程分支（阅读下一节与 [Git 分支](https://git-scm.com/book/zh/v2/ch00/ch03-git-branching) 了解更多信息），可以使用 `git pull` 命令来自动的抓取然后合并远程分支到当前分支。 这对你来说可能是一个更简单或更舒服的工作流程；默认情况下，`git clone` 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支（或不管是什么名字的默认分支）。 运行 `git pull` 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

### 推送到远程仓库

`git push [remote-name] [branch-name]`。 当你想要将 master 分支推送到 `origin` 服务器时

```shell
git push origin master
```

### 查看某个远程仓库

使用 `git remote show [remote-name]` 命令

可以通过 `git remote show` 看到更多的信息。

### 远程仓库的移除与重命名

如果想要重命名引用的名字可以运行 `git remote rename` 去修改一个远程仓库的简写名。 例如，想要将 `pb` 重命名为 `paul`，可以用 `git remote rename` 这样做：

```console
$ git remote rename pb paul
$ git remote
origin
paul
```

 移除一个远程仓库——你已经从服务器上搬走了或不再想使用某一个特定的镜像了，又或者某一个贡献者不再贡献了——可以使用 `git remote rm` ：

```console
$ git remote rm paul
$ git remote
origin
```

## 打标签

Git 可以给历史中的某一个提交打上标签，以示重要。比较有代表性的是人们会使用这个功能来标记发布结点（v1.0 等等）。

### 列出标签

[https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE](https://git-scm.com/book/zh/v2/Git-基础-打标签)

## 配置远程仓库

ssh通信模式
1.启动git bash
  在此之中可以使用linux命令
2、生成ssh key密钥

```git
 ssh-keygen -t rsa -C "1162314270@qq.com"
```

```
命令输入后，首先询问用户密钥的保存位置
	/c/Users/11623/.ssh/id_rsa
	默认为公钥和私钥保存在：用户/.ssh/id_rsa
瑞后询问是否设置保护密码
```

3、将公钥内容保存到GITHUB
	

公钥文件，保存在外部使用**==(保存到github的ssh密匙中)==**
		id_rsa.pub
	

私钥文件，做本机标识
	id_rsa
	打开github的setting页面添加ssh key
		

将公钥内容复制到key内容中

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDDaUZOQterXD2TZWb9VMO/s4jaIBIp0GIlsBlMZ+fl50NWbua4R8EtudIPIiMDGxsE7UmE/HGBMedsCjo1nS6h5OLz9ZT0n5h4FYOqurQWsyfdm+tgZK5mK/t1e93WlF+cjja+KiCMbntMYq7o/FGPWnlFlfiz9KiVos+oRWgC3RUx4hDqZoKFmfTxAiXQMJy2/pVYmHqliZ9+6mOvRJoe2C1mmTClTU5y9KLw8AWF0jTykUAeAfZjX1lwub8xkJ34QSNZFaBiouWcA7sz/Qa8kDGdFbZwR41Ugc+P4KYli72pecsbj2eVTZoHuCkW5GxxupV9jSbj/MePZPHHhhdH lmz
```



```
C:\Users\11623>ssh-keygen -t rsa -C "lmz"
Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\11623/.ssh/id_rsa):
C:\Users\11623/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\11623/.ssh/id_rsa.
Your public key has been saved in C:\Users\11623/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:722mRw92N2R2fb71Yr3tBEgSQORYxzzzF6n4xRkh7Mk lmz
The key's randomart image is:
+---[RSA 2048]----+
|       o=+o.. .o |
|       + .=...+  |
|      . . .B.+ +.|
|          .oE.=++|
|        S  ..o=.o|
|         .  = .o+|
|          .o + o*|
|         . .+ +o+|
|          o=.. ++|
+----[SHA256]-----+

```

4、在github上建立远程仓库信息
	不选择：Initialize this repository with a README
	创建仓库后界面上就给出了仓库的访问地址
	SSH，有了SSH key直接连接
		https://github.com/NicolasCoder/rep.git
	HRRPS,所有人都可以连接
		https://github.com/NicolasCoder/rep.git
删除github仓库
	仓库的setting

## 客服端访问

本地代码同步到远程仓库中

```shell
Either specify the URL from the command-line or configure a remote repository using
    git remote add <name> <url>

and then push using the remote name
    git push <name>
```

2、连接成功后将本地仓库全部代码推送到服务器端

```shell
git push -u origin master
```

强推，即利用强覆盖方式用你本地的代码替代git仓库内的内容

```shell
git push -f
git push -u origin master -f
```

：**使用这个强制的方法**

```shell
git pull origin master --allow-unrelated-histories
```

后面加上 `--allow-unrelated-histories` ， 把两段不相干的 分支进行强行合并

提高本地修改
	首先将修改提高到master分支上，然后推送代码到服务器

**3、git pull**的作用是从一个仓库或者本地的分支拉取并且整合代码。

```shell
git pull [<options>] [<repository> [<refspec>…]]
```

**实例：**默认使用方式

```
git pull
```

按照`git branch` 设置的默认跟踪的服务器和分支来拉取。

**实例：** 拉取远程服务器`origin`的`master`分支

```shell
git pull origin master
```

**删除远程分支**

```shell
git remote rm origin
```

## 克隆远程仓库

已经初始化完成的仓库
		已执行
			Initialize this repository with a README
**克隆远程仓库：**（直接在当前目录新建远程仓库的项目文件）



```
git clone （HTTPS）https://github.com/codeOflI/ssm-crud
```



​		

# GIT分支

## 1).基础操作

1、查看当前仓库中可用的分支

```git
git branch
```

2、创建新的分支

```shell
git branch 分支名
```

3、切换分支

```shell
git checkout 分支名
```

要想进行开发，一定不能再master中开发，必须在子分支开发
回到master进行合并分支

```shell
git merge dev
```

​	合并后提示Fast-forward
​		快速合并方式
一般开发分支和master分支都在服务器上

```shell
git push -u origin 分支名
```

删除分支

```shell
git branch -d 分支名
```

删除远程分支

```shell
git push origin --delete 分支名
```

推送空的分支

```shell
git push origin:dev
```

## 2）合并操作

冲突解决
	创建并切换到dev分支

```
git checkout -b dev
```

两个分支修改了同一文件
		提交到master后会提示conflict和文件名
			打开文件会发现相应的冲突都写在了里面
			修改后再次提交即可
**分支合并模式**
	通过图形化的方式查看所有分支合并记录

```shell
git log --graph --pretty=oneline
```

```shell
Fast forwaed(默认)合并
```

​		不会产生新的提交点
添加参数 -no-ff(Not Fast Forward)
​		

```shell
git merge --no-ff -m "use no-ff merge" dev
```

**–no-ff的作用：**

git merge –no-ff 可以保存你之前的分支历史。能够更好的查看 merge历史，以及branch 状态。

git merge 则不会显示 feature，只保留单条分支记录。

​	master不予许修改，dev分支如果有多人共享，需要在创建自己的分支在进行开发，合并时使用no-ff方式
bug分支,暂挂工作区
​	有错误的代码不能保存在暂存区
​	保存当前工作区的状态
​		git stash
​	查看所有暂时挂起的工作区
​		git stash list
​	两步
​		恢复挂起的工作区
​			git stash apply
​		清除保存的工作区
​			git stash drop
​	一步
​		恢复并清除保存区
​			git stash pop
feature,扩展分支
​	已经开发，但不准备使用的功能等
​	强制删除有不同修改的分支
​		git branch -D feature
补丁
​	比较分支区别
​		git diff master > patch
​		通过命令行的管道流，将diff保存再来patch文件里
​	通过补丁合并分支
​		git apply patch
​		当项目很大的时候，效率更高
​		不便于开发者沟通
​	git format-patch -M master
​		通知master
​	应用分支补丁
​		git am （补丁路劲）"E:\gitpro\0001-git-apply-patch.patch"

## orphan分支

创建一个新的分支（空）而不是从原分支拷贝

```shell
git checkout --orphan branchName
```

 可以将A和B两个完全不同的工程，可以作为两个分支放到git中。
 这时候还是一个分支

```git
git branch
master
```

1.创建orphan分支，名为dev**（本地和远程都需要)**

```git
$ git checkout --orphan dev
```

**注：**
 如果不提交东西，这个分支实际上没有创建

2.修改一些东西，并提交

**注意：**

**远程分支和本地分支保持名称一致，可减少不必要麻烦**

```git
$ git add .
$ git commit -m "init"
$ git push origin dev
```

**注：**
 git push origin source表示把本地代码（origin）提交到source分支
 git push origin master表示把本地代码（origin）提交到master分支

3.现在就有两个分支了master和source

# .gitignore的操作

```
下面是一些.gitignore文件忽略的匹配规则：

*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```



目的是忽略指定类型的文件或者某个文件夹

1. a、新建.gitignore文件：
2. b、输入要忽略的文件（可用通配符）
3. 利用`git status` 查看，可以看出排除了写入的文件，避免了其提交

```
HELP.md
target/
!.mvn/wrapper/maven-wrapper.jar
!**/src/main/**
!**/src/test/**

### xxx ###
application.properties

### STS ###
.apt_generated
.classpath
.factorypath
.project
.settings
.springBeans
.sts4-cache

### IntelliJ IDEA ###
.idea
*.iws
*.iml
*.ipr
.mvn/
mvnw
mvnw.cmd

### NetBeans ###
/nbproject/private/
/nbbuild/
/dist/
/nbdist/
/.nb-gradle/
build/

### VS Code ###
.vscode/
```

# 删除git上已经提交的文件

1.先查看有哪些文件可以删除,但是不真执行删除

```
git rm -r -n job-executor-common/target/*
```

-r 递归移除目录

-n 加上这个参数，执行命令时，是不会删除任何文件，而是展示此命令要删除的文件列表预览，所以一般用这个参数先看看要删除哪些文件，防止误删，确认之后，就去掉此参数，真正的删除文件。

上面这个命令就是先查看 job-executor-common/target/* 下有哪些可以删除的内容

2.执行删除

```
git rm -r  job-executor-common/target/*
```

此时,就把指定目录下所有内容从本地版本库中删除了

如果只想从版本库中删除,但是本地仍旧保留的话,加上 --cached 参数

```
git rm -r --cached job-executor-common/target/*
```

3.**删除远程版本库中的文件**

再执行提交操作即可

```
git commit -m"移除target目录下所有文件"
git push origin dev其中origin dev为分支名称
```

# git-me



## 开发时不使用master(保存)分支

1. 开发时添加大型文件，如果在主分支添加，将很难删除其占用的空间，通过合并分支，或者删除分支，很容易办到。

## 添加大型文件请慎重

git commit之后git会保存此时所有文件，如果此时添加了不必要的大型文件，会不必要的增加文件大小。特别是master分支。

[https://git-scm.com/docs/git-checkout]: 

# github

[Github 网页上 更新 Fork别人的 Repository](https://blog.csdn.net/huutu/article/details/51018317)