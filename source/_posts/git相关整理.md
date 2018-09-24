---
title: git相关整理
toc: false
date: 2018-09-24 20:42:55
categories:
- Web
tags:
- git
---

## git merge 和 git merge --no--ff有什么区别?

`git merge`命令用于合并指定分支到当前分支。默认情况下，执行`快进式合并`（fast-farward merge），直接通过把master指向feature来将两个分支并为一个分支，只保存master的分支信息。

`git merge --no--ff`执行正常合并，在master分支上生成新的节点，可以就可以保存之前的feature分支历史。能够更好的查看merge历史和branch状态。

因此为了保证版本演进的清晰，推荐使用`--no--ff`的方法。

![img](http://www.ruanyifeng.com/blogimg/asset/201207/bg2012070505.png)

![Image result for git merge -- no ff](https://i.stack.imgur.com/FMD5h.png)

## 工作区与暂存区

**工作区**：workspace，git管理的当前文件夹

**暂存区**：stage/index，工作区与分支之间的中转站

![工作区与暂存区](http://www.runoob.com/wp-content/uploads/2015/02/1352126739_7909.jpg)

`git add`——将修改添加到暂存区

`git checkout -- filename`——撤销工作区中指定文件的修改

`git checkout .`  ——撤销工作区中当前目录中的所有更改

`git rm --cached filename`——删除暂存区中的指定文件，但保留本地文件

`git rm filename`——删除暂存区中的指定文件，同时删除本地文件

## 版本回退

### reset

`git reset --soft` ——只回退commit，暂存区和工作区不做出改变。

`git reset --hard`——回退commit、暂存区、工作区，即本地的代码也会回退，**慎用！**

`git reset --mixed`——回退commit和暂存区，以上两种情况的中和版本，reset不带参数的**默认方式**

<u>关于版本：</u>

`HEAD`——当前版本

`HEAD^`——上一个版本

`HEAD^^`——上上一个版本

`HEAD^^^`——上上上一个版本

`HEAD~n`——上n个版本

也可以使用`commitID`

<u>一个栗子：</u>

`git reset --soft HEAD^`

只回退commit到上一个版本

**reset回退不会保留回退到的版本之后的所有commit**，因此在push时会因为落后于远程commit而报错，若想强制覆盖，可以为push命令加上`--force`或`-f`来进行强制操作。

### revert

revert用一个新提交来消除一个历史提交所做的所有修改，即

**revert不影响以前的所有commit**

```shell
git revert HEAD // 撤销最近一次提交
git revert HEAD^ // 撤销上上次提交
git revert commitID // 撤销指定id的提交
```

## 回退暂存区的修改

`git reset HEAD`，命令具体含义看上边的版本回退。

## 删除分支

`git branch -d BranchName`——删除本地分支

`git push origin --delete BranchName`/`git push origin :BranchName`——删除远程分支

## git pull 与 git fetch 区别

`git fetch`——拉取远程分支并更新到`origin/BranchName`分支中

`git pull`——拉取远程分支后与本地当前分支合并

```shell
git fetch origin master // 保存在本地'origin/master'分支中
git merge origin/master // 将fetch到的分支合并到本地的当前分支中

git pull origin master // 以上两句命令相当于这一句命令
```

## 合并时出现冲突的解决办法

`git merge`显示冲突时，

使用`git status`查看冲突的文件，

冲突部分用`<<<<<<< =======  >>>>>>>`标示，

编辑冲突的文件：

```js
// ……

<<<<<<< HEAD // 合并当前分支的内容
// ……
=======
// ……
>>>>>>> BranchName // 合并前要合并的分支的内容

// ……
```

然后`git add`冲突文件发现成功了！秒啊！



