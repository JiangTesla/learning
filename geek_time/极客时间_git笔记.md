# 极客时间_git 笔记

Git，一个版本控制工具，虽然我的工作中使用不到，用到了也就是个git clon，（没错我工作中主要用的是svn。。。。）但一个优秀的版本控制工具怎么能不学习一下如何使用呢。这篇博客是作为一个极客时间git课程的笔记的，实际git还有很多有用的资料和用法，详见官网和官方指导手册（手册上面写的太多了，看不过来-_-）。

git官网：<https://git-scm.com/>
**手册**：<https://git-scm.com/book/en/v2>
强烈建议去读一下这个手册，极客时间的那个课程很多截图都是取自这里。现在来看，极客时间的课程更偏向于针对工作环境下如何使用Git这个工具，如果真的要深入了解Git最好还是去读这个文档，考录到这个文档大概500+，所以还是倾向于按照课程安排，在需要理解的地方参照文档来补充下。

极客时间课程《玩转Git三剑客》的笔记，《玩转Git三剑客》课程地址：<https://time.geekbang.org/course/intro/100021601?tab=catalog>

## 第一章 Git基础

所谓基础知识，实际常用的就那么几个命令，想要深入了解个人感觉去看官方手册比较好。这里列一下视频中提到的几个Git命令：

### 01 课程综述

基本没什么需要记得，嗯，Git很好，下一个

### 02 安装Git

新电脑需要了解下，主要是win和linux下安装，详情见 [Git官网](<https://git-scm.com/>)

虽然我的电脑已经安装过来，但是这里还是记录下具体的地址吧，免得重装系统之后一时找不到（怎么可能这都不会，直接去官网啊，这都要啰嗦一下ε=(´ο｀*)))唉）。（或者后期可能会补上截图说明？也或者是补充下在无网络条件下本地源码编译安装？）
[下载地址](https://git-scm.com/downloads)
[linux下安装](<https://git-scm.com/download/linux>)
[windows下安装](https://git-scm.com/download/win)
[macOS](https://git-scm.com/download/mac)(本穷b没试过)

### 03 使用Git之前需要做的最小配置

如何进行配置，详情可以查阅progit（中文版）的P19以及P340 自定义GIT，建议在linux环境下进行操作，很多配置文件生效的逻辑是```/etc/gitconfig<~/.config/git/config<.git/config``` 这种配置风格类似vim,当你在windows下工作的时候就不容易找到这些文件（鬼知道这些配置文件放在了哪里，在Windows下的git Bash工具里你可以去配置/etc/gitconfig，但我暂时没找到它在我电脑的哪个位置）

#### 配置工作人员的user.name和user.name

```git config --global user.name 'you_name'```
```git config --global user.email 'you_email@demo.com'```

#### config工具的三个作用域

缺省等同于local

```bash
git config --local  #local 只对某一个仓库有效
git config --global #global 对当前用户所有仓库有效
git config --system #system 对系统所有登录的用户有效
```

显示config的配置，加--list

```bash
git config --list --local  #local 只对某一个仓库有效
git config --list --global #global 对当前用户所有仓库有效
git config --list --system #system 对系统所有登录的用户有效
```

### 04 创建第一个仓库并配置local用户信息

#### 两种场景

1.把已有的项目代码纳入Git管理

```bash
cd xxx
git init #xxx目录下会生成一个.git/目录
```

2.新建的项目直接用Git管理

```bash
cd xxx
git init your_project   #会在当前路径下创建名为your_project的文件夹，这个your_project里面会有一个.git/目录
cd your_project
```

### 05通过几次commit来认识工作区和暂存区

```bash
git status
git add -u  #对于已经被git管理的文件可以使用git add -u将修改加入暂存区
git commit -M"xxx"
```

### 00 额外加一个分支的简介

详情参考progit的Git分支章节P70

**分支创建**:

```bash
git branch testing  #在当前所在的提交对象上创建一个指针
```

它只是为你创建了一个可以移动的新的指针.

**分支切换**:

```bash
git checkout testing  #切换到新创建的 testing 分支
```

这样 HEAD 就指向 testing 分支了

#### 总结下Git管理下的文件的状态

详见progit的P35
你工作目录下的每一个文件都不外乎这两种状态：**已跟踪** 或 **未跟踪**。
>**已跟踪文件**：**已跟踪**的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后， 它们的状态可能是**未修改**，**已修改**或**已放入暂存区**。简而言之，已跟踪的文件就是 Git 已经知道的文件。
>**未跟踪文件**：工作目录中除已跟踪文件外的其它所有文件都属于**未跟踪文件**，它们既不存在于上次快照的记录中，也没有被放 入暂存区。
>>初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态，因为 Git刚刚检出了它们， 而你尚未编辑过它们。编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 在工作时，你可以选择性地将这些修改过的文件放入暂存区，然后提交所有已暂存的修改，如此反复。

检查当前文件状态

![文件变化周期](./img/%E6%96%87%E4%BB%B6%E7%9A%84%E7%8A%B6%E6%80%81%E5%8F%98%E5%8C%96%E5%91%A8%E6%9C%9F.png)

### 06给文件重命名的简便方法

```bash
git reset --hard    #这个命令会清理掉工作区和暂存区的内容。相当危险。小心使用。
```

```bash
git mv a b    #git mv 原文件名 目标文件名,将a重命名为b，并且这个操作并不需要add到暂存区就可以直接commit。
```

### 07 通过git log查看版本演变历史

直接使用```git log```就可以查看版本信息，可以通过几个参数调整```git log```的输出结果。当然详情还是需要看下progit的P43，这里有比较详细说明。

下面列出常用的几种

```bash
git log --oneline   #比较简洁的看log
git log -n4         #只看最近4次提交
git log --oneline -n4   #还可以组合
git log --graph     #图形化显示
git log --all       #显示所有（主要在有分支的情况下使用）
git log --help      #帮助文档
```

### 08 通过图形工具查看版本演变历史

`gitk`图形化界面，不太好用（而且中文乱码了估计是要在哪改编码格式，没兴趣搞这种东西，VSCode插件可能有更好用的吧），没什么好记的。

中文乱码可以将全局配置为utf-8编码

```bash
git config --global gui.encoding utf-8
```

### 09 探秘.git目录

***这节后面要在回看一遍，讲的挺好的***

```bash
git cat-file -t #查看git文件类型用t
git cat-file -p #查看git文件内容用p
```

git的主要对象有三种，commit、tree、blob。

```bash
$ cd .git/
$ ls -al
total 18
drwxr-xr-x 1 JXP 197121   0 Oct 10 02:24 ./
drwxr-xr-x 1 JXP 197121   0 Oct  8 21:36 ../
-rw-r--r-- 1 JXP 197121 598 Oct 10 02:07 COMMIT_EDITMSG
-rw-r--r-- 1 JXP 197121  23 Oct  8 21:10 HEAD   #指向是个引用，直接编辑这个文件可以做到切换分支 git checkout 分支。
-rw-r--r-- 1 JXP 197121 190 Oct 10 02:24 config #--local的config配置信息
-rw-r--r-- 1 JXP 197121  73 Oct  8 21:10 description
-rw-r--r-- 1 JXP 197121 175 Oct 10 02:21 gitk.cache
drwxr-xr-x 1 JXP 197121   0 Oct  8 21:10 hooks/
-rw-r--r-- 1 JXP 197121 489 Oct 10 02:07 index
drwxr-xr-x 1 JXP 197121   0 Oct  8 21:10 info/
drwxr-xr-x 1 JXP 197121   0 Oct  8 22:16 logs/
drwxr-xr-x 1 JXP 197121   0 Oct 10 02:08 objects/   #git文件系统核心内容,每一个都是一个对象
drwxr-xr-x 1 JXP 197121   0 Oct  8 21:10 refs/  #引用，里面存放了各个分支和tag的信息

```

### 10 commit、tree和blob三个对象之间的关系

Git对象彼此之间的关系 **这个讲的也比较深入，建议多看几遍**
![文件变化周期](./img/git%E7%9A%84commit_tree_blob%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB.png)
1.一个commit对应一个tree，这个tree代表了当时的这个commit的所有文件夹以及文件的快照
2.tree下有tree和blob
2.blob：一个文件对应一个blob，blob跟文件名无关，只要内容相同就是一个blob

### 11 | 小练习：数一数tree的个数

### 12 | 分离头指针情况下的注意事项

（没太搞懂）分离头指针是指目前的分支没有基于某一个branch，当进行分支切换时，基于分离头指针产生的commit不会记录在分支树上。如果这些变更是重要的，切记要和某一个分支绑在一起。

### 13 | 进一步理解HEAD和branch

HEAD一般是指向某一个分支（其实是指向了分支的一次commit），也可以指向某一个commit（分离头指针状态，此时没有分支）

做分支切换时，HEAD就会进行切换。

```bash
git checkout #
```

**比较差异**：

```bash
git diff A B #A,B是某两次commit的哈希值
git diff HEAD^1 HEAD #HEAD^1是上一级，也就是HEAD的父亲（也可以用HEAD~1，HEAD^1^1等同于HEAD~2）
```

## 第二章 独自使用Git时的常见场景

### 14 | 怎么删除不需要的分支？

删除分支：

```bash
git branch -d 分支名    #
git branch -D 分支名    #

```

### 15 | 怎么修改最新commit的message？

```bash
git commit --amend  #对最近一次提交的message做变更
```

### 16 | 怎么修改老旧commit的message？

```bash
git rebase -i 哈希  #对最近一次提交的message做变更
```

### 17 | 怎样把连续的多个commit整理成1个？

```bash
git rebase -i 哈希  #对最近一次提交的message做变更
```

### 18 | 怎样把间隔的几个commit整理成1个？

```bash
git rebase -i 哈希  #对最近一次提交的message做变更
```

### 19 | 怎么比较暂存区和HEAD所含文件的差异？

```bash
git diff --cached   #比较暂存区和HEAD所含文件的差异
```

### 20 | 怎么比较工作区和暂存区所含文件的差异？

```bash
git diff    #比较工作区和暂存区所含文件的差异
git diff -- xxx #xxx是指文件名
```

### 21 | 如何让暂存区恢复成和HEAD的一样？

意思就是不保留暂存区的内容
使用```reset```命令

```bash
git reset   #清掉暂存区内容
git diff --cached   #比较暂存区和HEAD
```

### 22 | 如何让工作区的文件恢复为和暂存区一样？

```bash
git checkout    #对工作区进行修改使用checkout
```

### 23 | 怎样取消暂存区部分文件的更改？

如果暂存区修改了很多文件，只有部分文件撤销，其他的保留

变化暂存区需要想到```reset```命令

```bash
git reset HEAD -- xxx   #xxx是你要恢复的文件
```

### 24 | 消除最近的几次提交

彻底的将commit从git仓库消失，小心使用

```bash
git reset --hard 哈希值   #哈希值是一个commit的哈希值，HEAD会变，工作区和暂存区都会变成你指定的那个commit的内容了，慎用
```

### 25 | 看看不同提交的指定文件的差异

```bash
git diff temp master -- xxx #temp和master是两个分支
#也可以用
git diff 2259b4ef70221bd 1e1badde7d1c   #对比两个commit之间的差异
```

### 26 | 正确删除文件的方法

```bash
git rm filename #
```

### 27 | 开发中临时加塞了紧急任务怎么处理？

思路：将手头的修改放到一个位置存起来，等处理了紧急的任务（将紧急任务commit之后）之后在（将工作区）恢复回来

```bash
git stash   #将工作区的内容放到一个区域（类似堆栈）
git stash list  #列出
#将修复的紧急的事情完成之后
git add -u
git commit
#再将之前放到临时区域的内容还原回来
git stssh 
```

### 28 | 如何指定不需要Git管理的文件？

写一个```.gitignoree```文件
详细写法参考《progit.pdf》P35

在最简单的情况下，一个仓库可能只根目录下有一个 .gitignore 文件，它递归地应用到整个仓库中。 然而，子目录下也可以有额外的 .gitignore 文件。子目录中的 .gitignore文件中的规则只作用于它所在的目录中。（Linux 内核的源码库拥有 206 个 .gitignore 文件。）

GitHub 有一个十分详细的针对数十种项目及语言的 .gitignore 文件列表， 你可以在
<https://github.com/github/gitignore> 找到它。

```bash
# 忽略所有的 .a 文件
*.a
# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a
# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO
# 忽略任何目录下名为 build 的文件夹
build/
# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

### 29 | 如何将Git仓库备份到本地？

常用传输协议：
本地就自己用，团队经常在远端有个公共的git托管平台

|   常用协议   |语法格式  |   说明    |
|    :----:  | :----:   |    :----:|
|   本地协议  | path/to/repo.git |哑协议|
|   本地协议(2) | file:///path/to/repo.git |智能协议|
| http/https协议 | file:///path/to/repo.git |(需要用户名密码)平时接触到的都是智能协议|
|    ssh协议 | user@git-server.com:path/to/repo.git |(需要公私秘钥对)工作中最常用的智能协议|

**直观区别**：哑协议传输进度不可见；智能协议可见
**传输速度**：智能协议比哑协议速度快。

```bash
#做个演示
#在开始架设 Git 服务器前，需要把现有仓库导出为裸仓库——即一个不包含当前工作目录的仓库。（progit.pdf P133）
git clone --bare /Users/path/to/.git ya.git #bare不带工作区的裸仓库
git clone --bare file:///Users/path/to/.git zhi_neng.git    #file协议

#在一个新的git目录操作（进行过git init）
# git remote add <shortname> <url> 添加一个新的远程 Git 仓库，同时指定一个方便使用的简写
git remote add jxp file:///e/tm/my_project.git  
#显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。
git remote -v

#现在你可以在命令行中使用字符串 pb 来代替整个 URL。 
#例如，如果你想拉取 Paul 的仓库中有但你没有的信息，
#可以运行  git fetch <remote> 这个命令会访问远程仓库，从中拉取所有你还没有的数据。 
#执行完成后，你将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。
git fetch jxp   #只有信息，没有被管理的文件



git remote add test file:///e/江小飘的笔记/git笔记_极客时间/    #与远端关联，注意是关联，不是clone
```

未完待续。。。没理解

目前来看，是需要先做一个远端的git仓库(没有工作区的那种)，然后其他人向这个远端仓库提交修改（但目前对git分支的理解不够，有点搞不明白）

## 第三章 Git于GitHub的简单同步

### 30 | 注册一个GitHub账号

### 31 | 配置公私钥

### 32 | 在GitHub上创建个人仓库

### 33 | 把本地仓库同步到GitHub

## 第四章 Git多人单分支协作时的常见场景 (暂时需要不了解)

主要记录和学习下这个章节，多人协作目前还不涉及，工具么，**知道基础概念之后**，剩下的具体操作用到的时候再看即可，用不上的“高端”操作看了也白看。

### 34 | 不同人修改了不同文件如何处理？

### 35 | 不同人修改了同文件的不同区域如何处理？

### 36 | 不同人修改了同文件的同一区域如何处理？

### 37 | 同时变更了文件名和文件内容如何处理？

### 38 | 把同一文件改成了不同的文件名如何处理？

## 第五章 Git集成使用禁忌

### 39 | 禁止向集成分支执行push -f操作

### 40 | 禁止向集成分支执行变更历史的操作

## 第六章 初识GitHub

### 41 | GitHub为什么会火？

### 42 | GitHub都有哪些核心功能？

### 43 | 怎么快速淘到感兴趣的开源项目?

### 44 | 怎样在GitHub上搭建个人博客

### 45 | 开源项目怎么保证代码质量？

### 46 | 为何需要组织类型的仓库？

## 第七章 使用GitHub进行团队协作 （暂时不需要了解）

主要记录和学习下这个章节，多人协作目前还不涉及，工具么，**知道基础概念之后**，剩下的具体操作用到的时候再看即可，用不上的“高端”操作看了也白看。

### 47 | 创建团队的项目

### 48 | 怎样选择适合自己团队的工作流？

### 49 | 如何挑选合适的分支集成策略？

### 50 | 启用issue跟踪需求和任务

### 51 | 如何用project管理issue？

### 52 | 项目内部怎么实施code review？

### 53 | 团队协作时如何做多分支的集成？

### 54 | 怎样保证集成的质量？

### 55 | 怎样把产品包发布到GitHub上？

### 56 | 怎么给项目增加详细的指导文档？

## 第八章 GitLab实践

### 57 | 国内互联网企业为什么喜欢GitLab？

### 58 | GitLab有哪些核心的功能？

### 59 | GitLab上怎么做项目管理？

### 60 | GitLab上怎么做code review？

### 61 | GitLab上怎么保证集成的质量？

### 62 | 怎么把应用部署到AWS上？
