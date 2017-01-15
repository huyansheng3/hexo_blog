---
title: Git学习总结
date: 2016-04-14 00:12:59
tags: git 
---

之前其实有稍微学习一点git，不过很久没用导致忘了很多，这次重新捡起来。

<!--more-->

## 1.Git本地的创建和使用
- 初始化仓库-`git    init`，项目目录下生成.git隐藏文件夹即为git仓库，其中项目目录即为工作区，版本库中分为暂存区 *stage* 和分支版本库。
![Git仓库](http://www.liaoxuefeng.com/files/attachments/001384907702917346729e9afbf4127b6dfbae9207af016000/0)
- `git add [filename]` 即指将该文件加入暂存区中，而 `git commit -m "commit message"`则表示将暂存区中的内容加入仓库中。
- 版本控制  
    - `git status`用于查看当前状态，若工作区无修改则会显示工作区为干净的。
    - `git log`显示 commit 的记录，`git log --graph`可以看到分支合并图。
    - `git diff [filename]`用户查看该文件修改前后的变化。
    - `git reset --hard HEAD^`可用于回滚到上一版本。`git reflog`查看命令历史。
    - `git checkout -- file`可以丢弃工作区的修改
    - 命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区，再使用`git checkout -- file`丢弃工作的修改。
- 命令`git rm`用于删除一个文件，若发现误删， `git checkout -- file`用版本库中版本替换工作区中的版本，无论工作区是修改还是删除，都可以一键还原。
- `git branch -b <name>`创建并切换分支，b应指buid的意思。删除分支：`git branch -d <name>`,切换分支：`git checkout <name>`注意跟用版本库中的内容替换工作区的命令区分。
- 分支合并。`git merge <name>`，若有冲突`git status`提示冲突文件，手动解决冲突后再次合并即可。
- 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`(`git stash apply`和`git stash drop`来删除)，回到工作现场。
- 创建和删除分支速度非常快的原因是仅增加了一个新的分支指针。可使用下图进行理解。
![理解分支](http://www.liaoxuefeng.com/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

## 2.Git远程仓库的使用
- 使用`ssh-keygen -t rsa -C "youremail@example.com"`创建SSH公钥和密钥，id_rsa和id_rsa.pub两个文件在*C:\Users\fengmanlou\.ssh*目录下。
- 要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`，并会将当前的master分支与远程版本库的分支关联起来。若 *origin* 设置错误，使用`git remote remove origin`删除配置的远程版本库。
- 关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；*origin* 即指此前设置好的远程版本库地址。
- 每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；
- 用git remote -v显示更详细的远程库信息
- 团队合作的分支图如下：
![实际工作](http://www.liaoxuefeng.com/files/attachments/001384909239390d355eb07d9d64305b6322aaf4edac1e3000/0)
- 多人协作的工作模式
    1.首先，可以试图用`git push origin branch-name`推送自己的修改；
    2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
    3.如果合并有冲突，则解决冲突，并在本地提交；
    4.没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；   
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；


