---
title: Git
---
# git branch
git branch有很多用法：
1.创建新分支，即创建一个可以移动的新指针。例如，创建一个testing分支需要用到git branch命令：
```
git branch testing
```
2.删除分支：
```
git branch -d <your branch>
```
会删除你想要删除的指针
3.git branch不加任何参数运行它，会得到当前所有分支的一个列表，加上一个-v参数：
```
git branch -v
```
可以查看每一个分支的最后一次提交
```
git branch --merged
git branch --no-merged
```
分别的作用是查看哪些分支已经合并到当前分支，另一个命令可以查看所有包含未合并工作的分支。
4.在远程分支中，git branch也有其它用法：
```
git branch -u <>
```
设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，可以使用-u选项运行git branch来显示设置。
如果想要查看设置的所有跟踪分支，可以使用 git branch 的 -vv 选项。

# 对于git branch的理解
在我的理解中，git branch可以和其它命令一起进行主题分支（短期分支）的开发，可以让项目往不同的的方向的发展。可以保留完全稳定的代码的同时完成后续开发和测试稳定性，稳定分支的指针总是落后提交历史。
git分支在原本的master分支上正常工作以及提交，在碰到项目需要添加的功能但稳定性不高的情况下，开发者可以通过git branch <new branch name>来创建一个新的分支并切换到新的分支上进行修改工作，git checkout <new branch name>
以及第二种情况，在开发新功能时，原本发布的项目出现了bug，这时需要紧急处理的情况下，可以切换回master分支，git checkout master,新创建一个分支git branch <fix branch name>,来进行修补bug的工作，当bug修复之后提交任务后，可以先把master分支，和这条分支先合并，
git checkout master
git merger <fix branch name>，因为<fix branch name>分支的父分支是master所以此时的指针只是进行了向前移动的操作。此时<fix branch name>分支和master分支指向同一个地方，所以可以删除<fix branch name>指针，git branch -d <fix branch name>.
之后在切换回新功能开发的分支上：git checkout <new branch name>,当功能开发完成，提交了之后，就可以进行合并了，因为master在之前工作中发生了改变，所以<new branch name>分支的父分支不是master了，此时会进行简单的三方合并，git checkout master,git merge <new branch name>,
此时项目已经合并完成，也就不需要<new branch name>指针了，git branch -d <new branch name>.