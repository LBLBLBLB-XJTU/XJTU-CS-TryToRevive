# Git

- [Git](#git)
    * [简述](#--)
    * [一些主要会用到的命令](#----------)
    * [所有命令](#----)
        + [基础操作](#----)
        + [分支与合并](#-----)
        + [远端操作](#----)
        + [撤销](#--)
        + [高级操作](#----)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## 简述
版本控制系统 (VCSs) 是一类用于追踪源代码（或其他文件、文件夹）改动的工具。顾名思义，这些工具可以帮助我们管理代码的修改历史；不仅如此，它还可以让协作编码变得更方便。VCS通过一系列的快照将某个文件夹及其内容保存了起来，每个快照都包含了文件或文件夹的完整状态。同时它还维护了快照创建者的信息以及每个快照的相关信息等等。  
想象一下，你有一个需要持续修改的项目（例如现代的公司项目，或者是需要持续一个学期的不断修改的课程实验），你每次都需要对之前的版本进行修改，但是如果本次的修改过于不尽如人意，你想从头再来，这就麻烦了。你可以每次都在本地文件夹中保存一个备份，但是无疑这很麻烦，会导致混淆。在这种情况下，VCS便发挥了作用，利用快照可以迅速的恢复到以前的版本。Git还有分布式的优点，使协作变得更简单。

***毫无疑问，绝大部分人都会去看git的原理，然后似懂非懂，最终只会敲命令。这完全没有问题，因为我至少看了5次原理，但不影响我只会敲命令。***

## 一些主要会用到的命令
按一般的执行顺序
1. git clone 从远程仓库克隆下来（GitHub上的，或者公司的）
2. git add . 所有修改过的文件添加到暂存区
3. git commit -m "评论" 提交
4. git status 显示当前的仓库状态
5. git push 推送到远程仓库
6. PR 在远程仓库中进行pull request。这一步往往在仓库的网页上进行手动操作。仓库的所有者（建立者或者是公司的主管）会进行审查后决定是否进行。

## 所有命令
### 基础操作
1. git help <command>: 获取 git 命令的帮助信息
2. git init: 创建一个新的 git 仓库，其数据会存放在一个名为 .git 的目录下
3. git status: 显示当前的仓库状态
4. git add <filename>: 添加文件到暂存区
5. git commit: 创建一个新的提交
6. 如何编写 良好的提交信息!
7. 为何要 编写良好的提交信息
8. git log: 显示历史日志
9. git log --all --graph --decorate: 可视化历史记录（有向无环图）
10. git diff <filename>: 显示与暂存区文件的差异
11. git diff <revision> <filename>: 显示某个文件两个版本之间的差异
12. git checkout <revision>: 更新 HEAD 和目前的分支
### 分支与合并
1. git branch: 显示分支
2. git branch <name>: 创建分支
3. git checkout -b <name>: 创建分支并切换到该分支，相当于 git branch <name>; git checkout <name>
4. git merge <revision>: 合并到当前分支 
5. git mergetool: 使用工具来处理合并冲突 
6. git rebase: 将一系列补丁变基（rebase）为新的基线
### 远端操作
1. git remote: 列出远端 
2. git remote add <name> <url>: 添加一个远端 
3. git push <remote> <local branch>:<remote branch>: 将对象传送至远端并更新远端引用 
4. git branch --set-upstream-to=<remote>/<remote branch>: 创建本地和远端分支的关联关系 
5. git fetch: 从远端获取对象/索引 
6. git pull: 相当于 git fetch; git merge 
7. git clone: 从远端下载仓库
### 撤销
1. git commit --amend: 编辑提交的内容或信息 
2. git reset HEAD <file>: 恢复暂存的文件 
3. git checkout -- <file>: 丢弃修改 
4. git restore: git2.32版本后取代git reset 进行许多撤销操作
### 高级操作
1. git config: Git 是一个高度可定制的工具，可以通过修改gitconfig文件来修改配置 
2. git clone --depth=1: 浅克隆（shallow clone），不包括完整的版本历史信息 
3. git add -p: 交互式暂存 
4. git rebase -i: 交互式变基 
5. git blame: 查看最后修改某行的人 
6. git stash: 暂时移除工作目录下的修改内容 
7. git bisect: 通过二分查找搜索历史记录 
8. .gitignore: 通过修改.gitignore文件来指定故意不追踪的文件