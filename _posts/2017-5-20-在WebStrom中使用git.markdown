---
title:  "在WebStorm中使用Git"
categories: [git]
tags: [Git]
date: 2017/05/20
---

总结一下在WebStrom中使用git的一些方法吧

>[WebStorm 有哪些过人之处？](https://www.zhihu.com/question/20936155) <br>
>[Git远程操作详解](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

---
> 引用阮一峰大神的一张图
![]({{ site.baseurl }}/images/20170520/bg2014061202.jpg)
* commit 提交改变到本地仓库
* push 推送到远程分仓库
* pull(fetch + merge) 从远程仓库拉取最新代码到本地

### WebStorm中自带的版本控制器
![]({{ site.baseurl }}/images/20170520/9.png)

1. Local Changes ==> 记录本地发生改变的文件
2. Log ==> 所有人更新、拉取、合并代码的操作记录
3. Console ==> 控制台
4. Update Info ==> 拉取到的最新代码的信息

### commit
`修改项目之后,提交修改的文件到本地仓库`
* 指令 git commit  -m "本次提交的描述信息"
* WebStorm快捷键command + k

![]({{ site.baseurl }}/images/20170520/7.png)
1. 在项目目录中右键项目名
2. 找到git目录中最上面的Commit Directory 点击

![]({{ site.baseurl }}/images/20170520/5.png)
3. 上面是本地发生改变得所有的文件,勾选表示本次提交
4. 中间是本次的提交说明
5. 下面是每一个发生改变的文件对比,左边是改变前,右边是改变后
6. 点击右下角 commit
7. WebStorm编辑器右下角会提示successful或者failed

### push
`将本地仓库的更新推送到远程仓库`
* 指令 git push
* WebStorm快捷键 command + shift + k

1. 右键项目名进入菜单,点击git,进入Repository菜单,点击push

![]({{ site.baseurl }}/images/20170520/8.png)

2. 可以选择push到不同的远程分支
3. 点击→后面的名称可以看到不同的远程分支,选择你想要的分支 右下角push
4. WebStorm编辑器右下角会提示successful或者failed

### git pull
`拉取远程仓库的最新代码并与本地仓库合并`
* 指令 git pull
* WebStorm快捷键 好像没有

1. 右键项目名进入菜单,点击git,进入Repository菜单,点击pull

![]({{ site.baseurl }}/images/20170520/10.png)
2. 如果没有冲突,你会在版本控制器的Update Info看到本次拉取的代码

**如果有人和你修改了同一个文件并且已经push到远程仓库中,那么拉取远程代码的时候可能会出现冲突**,如果解决不好然后又直接推到远程仓库会造成后续问题。

* 如果有冲突,WebStorm会提示哪些文件有冲突并选择merge,点击merge

![]({{ site.baseurl }}/images/20170520/2.png)
![]({{ site.baseurl }}/images/20170520/1.png)

* WebStorm将冲突代码分成3块可视区域,并且改动内容根据颜色区分
* 左边 ==> 本地修改  右边 ==> 远程仓库的修改  中间 ==> 合并之后的代码
* 绿色 ==> 增加的内容  蓝色 ==> 改动的内容  红色 ==> 冲突的内容

点击箭头可以将两边的代码添加到中间
**谨慎合并红色区域代码,最好和改动这个文件的人一起合并**

然后点击Apply,合并完成

合并之后还需要提交代码并推送到远程仓库来保持代码同步




