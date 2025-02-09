#### 1.Git 基础

##### 1.1Git 基本配置

```
git --version #返回版本号
git config --list #查看所有配置
git config --global user.name "用户名" #全局配置用户名，有分号
git config --global user.email 邮箱地址 #全局配置邮箱，无分号
git config user.email #查看邮箱，.gitconfig
git config --global alias.ci commit #设置别名 git ci
git config --global alias.pullom 'pull origin master' #设置组合命令 git pullom
```

##### 1.2Git基本操作

```
git init #创建一个空仓库，或者重新初始化一个已有仓库，生成 .git 管理仓库
git add #把文件添加到临时缓冲区，git add <file> 可反复多次添加文件，添加所有文件 git add .
git commit #提交改动至仓库，git commit -m "message"
	git commit -m “message” #-m 参数表示可以直接输入后面的 “message” ，不加 -m 参数会掉 vim 添加
	git commit -a -m “massage” #-a 参数将所有已跟踪文件的修改提交到本地仓库，即使没有添加到缓存区
	git commit --amend #追加提交，不增加新的 commit-id 将新修改的代码追加到前一次的 commit-id 中
git status #查看仓库状态
git diff #显示改动，git diff <file>
	git diff <$commit-id1> <$commit-id2> #比较两次提交之间的差异
    git diff <branch1>..<branch2> #在两个分支之间比较
    git diff --staged #比较暂存区和版本库差异
git reset #版本回退，远端修改需要git push
	git reset --hard <$commit-id> #抛弃当前工作区的修改
	git reset --soft <$commit-id> #回退到之前的版本，但保留当前工作区的修改，可以重新提交
git log #打印提交日志，显示作者、时间和提交信息
git branch #查看、添加、删除分支 git branch a 创建a分支，删除分支 git branch -d a，强制删除 -D
git checkout #切换分支、标签 git checkout -b a 建立分支后自动切换到该分支
git merge #合并分支，先切换到 master 分支 然后 git merge a 将a分支合并到master分支
git tag #新建、查看标签 git tag v0.1/v0.9/v1.0
git clone #下载仓库 git clone git@github.com:project/repo.git
git remote add origin 远程库地址 #把本地库与远程库关联
git push origin master #本地代码推到远程，git push origin master --force 本地版本落后远端
git pull origin master #远程最新的代码更新到本地
```

#### 2.Git进阶

##### 2.1设置别名

[Github入门教程]: https://github.com/CatOneTwo/GitHub-Tutorial?tab=readme-ov-file
[Git入门教程]: https://liaoxuefeng.com/books/git/introduction/index.html

