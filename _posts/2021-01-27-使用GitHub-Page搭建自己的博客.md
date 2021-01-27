---
layout: default
title: 使用GitHub Page搭建自己的博客
date: 2021-01-27 11:20:26 -0000
categories: CATEGORY-1 CATEGORY-2
tags:tag2
published: true
---

### 创建GitHub Pages仓库

### GitHub的项目同步到本地

#### Git的基本操作

```shell
git config --list

##基本工作流程 所有操作cd到root directory下进行
git init # 创建.git文件 内含一个版本库 储存项目本身与历史
git add #将需要提交的文件加入
git rm #删除文件
git status #显示自上次提交发生以来的所有更改 下次提交时这些更改将会被加入新的版本中
git diff #显示详细被更改的详细内容
git commit #提交到新的分支(新的版本) 生成本次提交的散列值

##远端与本地的同步
git clone #用这个从github上拉仓库 自动生成.git以及.git/config中一些有关push pull的url配置(会存储原版本的路劲)
git pull
git push <repository> <refspec>
#没有指定参数的情况下提交到origin master 
#master是remote仓库的主分支分支这两个变量的实际值保存在.git/config文件中的
#github上的仓库相当于bare repository 本地是work repository
#<repository> 是push的目的地 remote仓库的地址 可以是URL或者name of remote "eg. origin"
#<refspec> 指定要用哪个源对象更新哪个目标引用。格式为+<src>:<dst> 通常会写想要push到的分支的分支名称

#其他信息的展示
git log --graphic
git log --oneline #喜欢这个的输出样式
git remote show origin
```

有关.git文件夹下的文件结构与作用参考
https://www.cnblogs.com/codingdevops/p/CODING.html

### 选择Jekyll主题

####Jekyll的介绍以及配置文件
