#### 1.Git 基础

##### 1.1Git版本库

Git的版本库存储 stage（或者叫index）的暂存区，Git自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![image-20250209165929222](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250209165929222.png)

##### 1.2Git 基本配置

```
git --version #返回版本号
git config --list #查看所有配置
git config --global user.name "用户名" #全局配置用户名，有分号
git config --global user.email 邮箱地址 #全局配置邮箱，无分号
git config user.email #查看邮箱，.gitconfig
git config --global alias.ci commit #设置别名 git ci
git config --global alias.pullom 'pull origin master' #设置组合命令 git pullom
```

##### 1.3Git基本操作

```
git init #创建一个空仓库，或者重新初始化一个已有仓库，生成 .git 管理仓库
git add #把文件添加到临时缓冲区，git add <file> 可反复多次添加文件，添加所有文件 git add .
git commit #提交改动至仓库，git commit -m "message"
	git commit -m “message” #-m 参数表示可以直接输入后面的 “message” ，不加 -m 参数会掉 vim 添加
	git commit -a -m “massage” #-a 参数将所有已跟踪文件的修改提交到本地仓库，即使没有添加到缓存区
	git commit --amend #追加提交，不增加新的 commit-id 将新修改的代码追加到前一次的 commit-id 中
git status #查看仓库状态
git log #打印提交日志，显示作者、时间和提交信息 git log --graph --pretty=oneline，git reflog
git branch #查看、添加、删除分支 git branch a 创建a分支，删除分支 git branch -d a，强制删除 -D
git checkout #切换分支、标签 git checkout -b a 建立分支后自动切换到该分支
git merge #合并分支，先切换到 master 分支 然后 git merge a 将a分支合并到master分支
git tag #新建、查看标签 git tag v0.1/v0.9/v1.0，git show v0.9
git clone <远程库地址>#下载仓库
git remote add origin <远程库地址> #把本地库与远程库关联
git push (-u) origin master #本地代码推到远程，git push origin master --force 本地版本落后远端
git pull origin master #远程最新的代码更新到本地，origin 远程仓库的默认名称
```

#### 2.Git进阶

##### 2.1版本回退

```
git diff #显示改动，git diff <file>
	git diff <$commit-id1> <$commit-id2> #比较两次提交之间的差异
    git diff <branch1>..<branch2> #在两个分支之间比较
    git diff --staged #比较暂存区和版本库差异
git reset #版本回退，远端修改需要git push
	git reset --hard HEAD^ #HEAD指向当前版本，HEAD^指向上一次修改
	git reset --hard <$commit-id> #抛弃当前工作区的修改
	git reset --soft <$commit-id> #回退到之前的版本，但保留当前工作区的修改，可以重新提交
git checkout -- readme.md #撤销readme.md文件在工作区的修改
	readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
	readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
git rm readme.md #删除文件后需要 git commit到版本库
```

##### 2.2分支管理

master分支应该是非常稳定的，也就是仅用来发布新版本；dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上。

![image-20250209162344964](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250209162344964.png)

```
git branch #查看分支
git branch <name> #创建分支
git remote -v #查看远程库信息
git checkout <name>/git switch <name> #切换分支
git checkout -b <name>/git switch -c <name> #创建+切换分支
git merge #合并某分支到当前分支
git branch -d <name> #删除分支
git log --graph #查看分支合并图
git merge --no-ff -m "merge no-ff" dev #--no-ff表示禁用Fast forward，本次合并创建新commit id
git branch --set-upstream-to=origin/dev dev #指定本地dev分支与远程origin/dev分支的链接
```

##### 2.3Bug 分支

dev开发分支未完成，没有提交，需要创建issue-101 分支修bug。

```
git status #on branch dev changes to be committed
git stash #Saved working directory and index state WIP on dev: f52c633 储存现场
git checkout master #假如在master分支上修复bug
git checkout -b issue-101 #Switched to a new branch "issue-101"
git add somefile
git commit -m "fix bug 101"
git switch master
git merge --no-ff -m "merged bug fix 101" issue-101 #合并分支
git switch dev
git status #on branch dev nothing to commit
git stash list #stash@{0}: WIP on dev: f52c633 add merge
git stash pop #回复现场同时删掉stash内容
git stash apply stash@{0} #多次stash时，恢复指定stash
git cherry-pick 4c805e2 #将bug在dev上修复
git rebase #可以把本地未push的分叉提交历史整理成直线

```

##### 2.4标签管理

```
git push origin <tagname> #推送一个本地标签
git push origin --tags #推送全部未推送过的本地标签
git tag -d <tagname> #删除一个本地标签
git push origin :refs/tags/<tagname> #删除一个远程标签

```

> [ **Github入门教程** ]： https://github.com/CatOneTwo/GitHub-Tutorial?tab=readme-ov-file 
>
> [ **Git入门教程** ]：https://liaoxuefeng.com/books/git/introduction/index.html 