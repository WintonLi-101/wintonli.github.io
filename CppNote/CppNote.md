#### 1.Git 基础

##### 1.1Git版本库

Git的版本库存储 stage（或者叫index）的暂存区，Git自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![](D:\Blogs\wintonli.github.io\CppNote\image-20250209165929222.png)

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

![](D:\Blogs\wintonli.github.io\CppNote\image-20250209162344964.png)

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

#### 2.C++ 基础

##### 1.1智能指针

独占所有权：auto_ptr 会转移自身所有权被移除，存在潜在内存崩溃，unique_ptr 对象转移需要使用 move 语义；

共享所有权：shared_ptr 内部采用引用计数，拷贝时引用计数+1，退出作用域或者释放引用计数-1，引用计数为0时自动释放管理对象；

弱共享所有权：weak_ptr 主要是为了解决shared_ptr的循环引用，并不会拥有资源，**不能访问对象所提供的成员函数**，不过可以通过weak_ptr.lock()来产生一个拥有访问权限的shared_ptr；

```
template<class T> 
class auto_ptr 
{ 
private:
    T* p; 
public: 
    auto_ptr(T* s) :p(s) {} 
    ~auto_ptr() { delete p; } 
    auto_ptr(auto_ptr& a) { 
      p = a.p; 
      a.p = NULL; 
    } 
    auto_ptr& operator=(auto_ptr& a) { 
      delete p; 
      p=a.p; 
      a.p = NULL; 
      return *this; 
    } 
    T& operator*() const { return *p; } 
    T* operator->() const { return p; } 
}; 

auto_ptr<std::string> p1(new string("hello"));
auto_ptr<std::string> p2 = p1;//p1==null
```

```
template<class T>
class auto_ptr
{
private:
    T* p;
public:
    explicit auto_ptr(T* s = nullptr) noexcept : p(s) {}
    ~auto_ptr() noexcept { delete p; }
    // 删除拷贝构造函数和拷贝赋值运算符
    auto_ptr(const auto_ptr&) = delete;
    auto_ptr& operator=(const auto_ptr&) = delete;
    // 移动构造函数
    auto_ptr(auto_ptr&& a) noexcept : p(a.p) {
        a.p = nullptr;
    }
    // 移动赋值运算符
    auto_ptr& operator=(auto_ptr&& a) noexcept {
        if (this != &a) {
            delete p;
            p = a.p;
            a.p = nullptr;
        }
        return *this;
    }
    T& operator*() const noexcept { return *p; }
    T* operator->() const noexcept { return p; }
    // 获取原始指针
    T* get() const noexcept { return p; }
    // 释放所有权
    T* release() noexcept {
        T* temp = p;
        p = nullptr;
        return temp;
    }
    // 重置指针
    void reset(T* s = nullptr) noexcept {
        if (p != s) {
            delete p;
            p = s;
        }
    }
};
```

1.四种智能指针: auto_ptr, unique_ptr, shared_ptr, weak_ptr 如何使用，如何自己实现，是否线程安全，循环引用；

2.C++ 内存模型；局部常量，全局常量，字面值常量；

3.引用和指针，引用传递和指针传递的区别，是否可以重载；野指针和悬空指针；函数指针；

4.static作用：局部变量、全局变量、函数、类、内部类；const 修饰基本数据类型，指针/引用数据类型，修饰函数，修时类；const和宏定义的区别 typedefine；define/const/typedef/inline；

5.重载、重写、隐藏；

6.C++四种强制类型转换；

7.C++继承中类对象内存模型，new/delete，malloc/free 区别；基本数据类型与引用数据类型；

8.volatile 和 extern；

9.虚函数与虚函数表；继承体系中虚函数表如何处理；虚析构函数，构造函数为什么不能为虚函数；构造函数或者析构函数中调用虚函数；纯虚函数；

10.深拷贝与浅拷贝；

11.什么情况会调用拷贝构造，为什么拷贝构造必须要传递引用，不能值传递；构造和析构函数是否可以抛异常；

12.内存对齐；

13.初始化列表；

14.手写智能指针；手写strcat/strcpy/strncpy/memset/memcopy;

15.大小端，大小端转换字符串；

16.Lambda 表达式；语法糖/函数对象/仿函数；

17.左值引用，右值引用，引用折叠；

#### 3.数据结构与算法

1.二叉树，二叉搜索树，平衡二叉树、AVL；

2.红黑树；

3.STL 容器与使用方法；vector/list/deque/stack/queue/heap/priority_queue/map/set 迭代器失效问题；

4.冒泡/选择/插入/.....排序算法；

5.二叉树前中后序遍历；

6.单例/工厂、观察者、装饰器模式，生产者消费者模型，MVC,MVVM模型；

#### 4.Android 基础

#### 5.操作系统基础

1.预处理、编译、汇编、链接；

2.动态编译，静态编译，动态链接，静态链接;



