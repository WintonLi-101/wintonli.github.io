#### 1.Git 基础

##### 1.1Git版本库

Git的版本库存储 stage（或者叫index）的暂存区，Git自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

![image-20250209165929222](D:\Blogs\wintonli.github.io\CppNote\image-20250209165929222.png)

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

![image-20250209162344964](D:\Blogs\wintonli.github.io\CppNote\image-20250209162344964.png)

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

**独占所有权：**auto_ptr 会转移自身所有权被移除，存在潜在内存崩溃，unique_ptr 禁止拷贝之允许转移，使用 move 语义；

**共享所有权：**shared_ptr 内部采用引用计数，拷贝时引用计数+1，退出作用域或者释放引用计数-1，引用计数为0时自动释放管理对象；

线程安全：

1.引用计数的加减操作是否线程安全；

在多线程环境下，管理同一个数据的shared_ptr在进行计数的增加或减少的时候是线程安全的，这是一波原子操作；

2.shared_ptr修改指向时是否线程安全；

修改同一对象，不安全(（需要额外的同步机制，如互斥锁或 `std::atomic<std::shared_ptr<T>>`）)  C++20 引入；

修改不同对象，线程安全，引用计数的修改是原子的;

**弱共享所有权：**weak_ptr 主要是为了解决shared_ptr的循环引用，并不会拥有资源，**不能访问对象所提供的成员函数**，不过可以通过weak_ptr.lock()来产生一个拥有访问权限的shared_ptr;

不具有普通指针的行为，没有重载operator*和operator->; 没有共享资源，它的构造不会引起引用计数增加; 用于协助shared_ptr来解决循环引用问题; 可以从一个shared_ptr或者另外一个weak_ptr对象构造，进而可以间接获取资源的弱共享权；

`std::weak_ptr` 本身**不能直接访问**其引用的对象资源，因为它是一个弱引用指针，不会增加对象的引用计数，也不会管理对象的生命周期。它的主要作用是观察 `std::shared_ptr` 管理的对象，而不会影响对象的销毁。

要访问 `std::weak_ptr` 引用的对象资源，必须先将 `std::weak_ptr` 转换为 `std::shared_ptr`，然后通过 `std::shared_ptr` 访问对象。这种设计是为了确保在访问对象时，对象仍然存在（即未被销毁）。

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
int main() {
    //基本用法
    auto_ptr<int> ptr1(new int(42));
    //移动语义
    auto_ptr<int> ptr2 = std::move(ptr1); // 所有权从 ptr1 转移到 ptr2
    //释放所有权
    int* raw_ptr = ptr2.release(); // 释放 ptr2 的所有权
    //重置指针
    auto_ptr<int> ptr3(new int(100));
    ptr3.reset(new int(200)); // 释放旧资源，管理新资源
    //检查是否为空
    auto_ptr<int> ptr4;
    std::cout << (ptr4 ? "not null" : "null") << std::endl; 
    return 0;
}
```

```
#include <utility>  // for std::swap
template <typename T, typename Deleter = std::default_delete<T>>
class unique_ptr {
private:
    T* ptr;          // 原始指针
    Deleter deleter; // 删除器
public:
    // 构造函数
    explicit unique_ptr(T* p = nullptr) noexcept : ptr(p) {}
    // 禁止拷贝构造函数
    unique_ptr(const unique_ptr&) = delete;
    // 移动构造函数
    unique_ptr(unique_ptr&& other) noexcept
        : ptr(other.release()), deleter(std::move(other.deleter)) {}
    // 析构函数
    ~unique_ptr() {
        reset();
    }
    // 禁止拷贝赋值运算符
    unique_ptr& operator=(const unique_ptr&) = delete;
    // 移动赋值运算符
    unique_ptr& operator=(unique_ptr&& other) noexcept {
        if (this != &other) {
            reset(other.release());
            deleter = std::move(other.deleter);
        }
        return *this;
    }
    // 释放所有权
    T* release() noexcept {
        T* temp = ptr;
        ptr = nullptr;
        return temp;
    }
    // 重置指针
    void reset(T* p = nullptr) noexcept {
        if (ptr != p) {
            deleter(ptr); // 调用删除器释放资源
            ptr = p;
        }
    }
    // 交换两个 unique_ptr
    void swap(unique_ptr& other) noexcept {
        std::swap(ptr, other.ptr);
        std::swap(deleter, other.deleter);
    }
    // 获取原始指针
    T* get() const noexcept {
        return ptr;
    }
    // 解引用操作符
    T& operator*() const noexcept {
        return *ptr;
    }
    // 箭头操作符
    T* operator->() const noexcept {
        return ptr;
    }
    // 检查是否为空
    explicit operator bool() const noexcept {
        return ptr != nullptr;
    }
};
int main() {
    // 使用默认删除器
    unique_ptr<int> ptr1(new int(42));
    //C++14 后推荐使用
    auto p1 = std::make_unique<double>(3.14);
	auto p2 = std::make_unique<double[]>(n);
    // 移动语义
    unique_ptr<int> ptr2 = std::move(ptr1);
    // 数组支持
    unique_ptr<int[]> arr(new int[3]{1, 2, 3});
    // 自定义删除器
    auto custom_deleter = [](int* p) {
        std::cout << "Custom deleter called!" << std::endl;
        delete p;
    };
    unique_ptr<int, decltype(custom_deleter)> ptr3(new int(100), custom_deleter);
    // 输出: Custom deleter called!（当 ptr3 离开作用域时）
    return 0;
}

void fun1(double *);//接受原始指针
void fun2(std::unique<double> *);//接受unique_ptr的指针
void fun3(std::unique<double> &);//接受unique_ptr的引用
void fun4(std::unique<double> );//接受unique_ptr的值，通过移动语义
int main() {
  std::unique_ptr<double> p(new double(3.14));
  fun1(p.get());
  fun2(&p);
  fun3(p);//传递引用不转移所有权
  auto p2(p.release()); // 转移所有权
  auto p2.reset(new double(1.0));
  fun4(std::move(p2));//通过移动语义传递unique_ptr的值
  return 0;
}
```

```
// 前向声明 weak_ptr
template <class T>
class weak_ptr;
// 引用计数器类
class Counter {
public:
    Counter() : s_(0), w_(0) {}
    int s_; // shared_ptr 的引用计数
    int w_; // weak_ptr 的引用计数
};
// shared_ptr 类
template <class T>
class shared_ptr {
public:
    // 构造函数
    explicit shared_ptr(T* p = nullptr) : ptr_(p), cnt_(new Counter()) {
        if (ptr_) {
            cnt_->s_ = 1;
        }
    }
    // 析构函数
    ~shared_ptr() {
        release();
    }
    // 拷贝构造函数
    shared_ptr(const shared_ptr<T>& other) noexcept : ptr_(other.ptr_), cnt_(other.cnt_) {
        if (cnt_) {
            cnt_->s_++;
        }
    }
    // 移动构造函数
    shared_ptr(shared_ptr<T>&& other) noexcept : ptr_(other.ptr_), cnt_(other.cnt_) {
        other.ptr_ = nullptr;
        other.cnt_ = nullptr;
    }
    // 拷贝赋值运算符
    shared_ptr<T>& operator=(const shared_ptr<T>& other) noexcept {
        if (this != &other) {
            release(); // 释放当前资源
            ptr_ = other.ptr_;
            cnt_ = other.cnt_;
            if (cnt_) {
                cnt_->s_++;
            }
        }
        return *this;
    }
    // 移动赋值运算符
    shared_ptr<T>& operator=(shared_ptr<T>&& other) noexcept {
        if (this != &other) {
            release(); // 释放当前资源
            ptr_ = other.ptr_;
            cnt_ = other.cnt_;
            other.ptr_ = nullptr;
            other.cnt_ = nullptr;
        }
        return *this;
    }
    // 解引用运算符
    T& operator*() const noexcept {
        return *ptr_;
    }
    // 箭头运算符
    T* operator->() const noexcept {
        return ptr_;
    }
    // 获取原始指针
    T* get() const noexcept {
        return ptr_;
    }
    // 获取引用计数
    int use_count() const noexcept {
        return cnt_ ? cnt_->s_ : 0;
    }
    // 重置指针
    void reset(T* p = nullptr) {
        release(); // 释放当前资源
        ptr_ = p;
        cnt_ = new Counter();
        if (ptr_) {
            cnt_->s_ = 1;
        }
    }
    // 释放资源
    void release() {
        if (cnt_) {
            cnt_->s_--;
            if (cnt_->s_ == 0) {
                delete ptr_;
                ptr_ = nullptr;
                if (cnt_->w_ == 0) {
                    delete cnt_;
                    cnt_ = nullptr;
                }
            }
        }
    }
    // 友元类 weak_ptr
    friend class weak_ptr<T>;
private:
    T* ptr_;        // 原始指针
    Counter* cnt_;  // 引用计数器
};
```

```
template <class T>
class weak_ptr {
public:
    // 默认构造函数
    weak_ptr() noexcept : ptr_(nullptr), cnt_(nullptr) {}
    // 构造函数（从 shared_ptr 构造）
    weak_ptr(const shared_ptr<T>& s) noexcept : ptr_(s.ptr_), cnt_(s.cnt_) {
        if (cnt_) {
            cnt_->w_++;
        }
    }
    // 拷贝构造函数
    weak_ptr(const weak_ptr<T>& other) noexcept : ptr_(other.ptr_), cnt_(other.cnt_) {
        if (cnt_) {
            cnt_->w_++;
        }
    }
    // 移动构造函数
    weak_ptr(weak_ptr<T>&& other) noexcept : ptr_(other.ptr_), cnt_(other.cnt_) {
        other.ptr_ = nullptr;
        other.cnt_ = nullptr;
    }
    // 析构函数
    ~weak_ptr() {
        release();
    }
    // 拷贝赋值运算符
    weak_ptr<T>& operator=(const weak_ptr<T>& other) noexcept {
        if (this != &other) {
            release();
            ptr_ = other.ptr_;
            cnt_ = other.cnt_;
            if (cnt_) {
                cnt_->w_++;
            }
        }
        return *this;
    }
    // 移动赋值运算符
    weak_ptr<T>& operator=(weak_ptr<T>&& other) noexcept {
        if (this != &other) {
            release();
            ptr_ = other.ptr_;
            cnt_ = other.cnt_;
            other.ptr_ = nullptr;
            other.cnt_ = nullptr;
        }
        return *this;
    }
    // 从 shared_ptr 赋值
    weak_ptr<T>& operator=(const shared_ptr<T>& s) noexcept {
        release();
        ptr_ = s.ptr_;
        cnt_ = s.cnt_;
        if (cnt_) {
            cnt_->w_++;
        }
        return *this;
    }
    // 提升为 shared_ptr
    shared_ptr<T> lock() const noexcept {
        if (cnt_ && cnt_->s_ > 0) {
            return shared_ptr<T>(*this);
        }
        return shared_ptr<T>();
    }
    // 检查是否过期
    bool expired() const noexcept {
        return !cnt_ || cnt_->s_ == 0;
    }
    // 获取引用计数
    int use_count() const noexcept {
        return cnt_ ? cnt_->s_ : 0;
    }
    // 释放资源
    void release() {
        if (cnt_) {
            cnt_->w_--;
            if (cnt_->w_ == 0 && cnt_->s_ == 0) {
                delete cnt_;
                cnt_ = nullptr;
            }
        }
    }
    // 友元类 shared_ptr
    friend class shared_ptr<T>;
private:
    T* ptr_;        // 原始指针
    Counter* cnt_;  // 引用计数器
};
```

```
#include <iostream>
#include <memory>
class Controller {//循环引用
public:
    Controller() = default;
    ~Controller() {
        std::cout << "in ~Controller" << std::endl;
    }
    class SubController {
    public:
        SubController() = default;
        ~SubController() {
            std::cout << "in ~SubController" << std::endl;
        }
        std::shared_ptr<Controller> controller_; // SubController 持有 Controller 的 shared_ptr
    };
    std::shared_ptr<SubController> sub_controller_; // Controller 持有 SubController 的 shared_ptr
};

int main() {
    // 创建 Controller 和 SubController 对象，引用计数为 1
    auto controller = std::make_shared<Controller>();
    auto subController = std::make_shared<Controller::SubController>();
    // 设置互相引用，引用计数变为 2
    controller->sub_controller_ = subController;
    subController->controller_ = controller;
    // main 函数结束时，Controller 和 SubController 的引用计数减为 1，导致内存泄漏
    return 0;
}
```

注: 裸指针和智能指针不要混用；智能指针间不要混用；不要管理同一个裸指针；避免使用get() 获取原生指针；之管理堆上的对象；

如何解决循环引用？

1.使用 std: weak_ptr

```
std::weak_ptr<Controller> controller_; // 使用 weak_ptr 打破循环引用
```

2.手动打破循环引用

```
controller->sub_controller_.reset(); // main 函数中手动打破循环引用
```

> [**智能指针使用、使用、避坑和实现**]：https://mp.weixin.qq.com/s/dnYIOVsYtWlPlnhoQBPUfQ

##### 1.2.内存模型

栈、堆区别；局部常量，全局常量，字面值常量；

##### 1.3.引用和指针

引用传递和指针传递的区别，是否可以重载；野指针和悬空指针；函数指针；

语法区别：引用不能为空，不能重新绑定，不能获取引用地址；指针可以为nullptr，可以重新指向其他对象，可以获取指针地址；

语义区别：引用时对象别名，必须初始化且不能为空，语法更简洁，常用于函数参数传递和返回值优化；指针是一个变量，存储对象的地址，可以为空，更灵活，常用于动态内存管理，数组操作和低级编程；

使用场景：引用 函数参数传递避免拷贝，返回值优化返回引用避免拷贝，操作符重载如 operate[]; 指针 动态内存分配，数组操作，多态

可以重载引用传递和指针传递的函数

野指针：定义：未初始化或指向无效内存的指针。风险：访问野指针会导致未定义行为。

```
int* ptr; // 未初始化，野指针
*ptr = 10; // 未定义行为
```

悬空指针：定义：指向已释放内存的指针。风险：访问悬空指针会导致未定义行为。

```
int* ptr = new int(10);
delete ptr; // 释放内存
*ptr = 20; // 悬空指针，未定义行为
```

函数指针：是指向函数的指针，可以用于调用函数或作为回调函数。

```
返回类型 (*指针名)(参数列表);
using FuncType = int(int, int); // 定义函数类型
FuncType* funcPtr = add;        // 定义函数指针
```

##### 1.4.static/const作用

局部变量、全局变量、函数、类、内部类；const 修饰基本数据类型，指针/引用数据类型，修饰函数，修时类；const和宏定义的区别 typedefine；define/const/typedef/inline；

static 修饰局部变量、全局变量、修饰函数、修饰类、类成员、类函数；

const 修饰基本数据类型，修饰指针/引用变量、修饰函数、修饰类中常量和类成员函数、修饰类对象、定义常量对象；

`const` 成员函数不能直接修改类的成员变量。如果你需要在 `const` 成员函数中修改某个变量，可以使用 `mutable` 关键字来标记该变量，使其在 `const` 成员函数中仍可被修改。

##### 1.5.重载/重写/隐藏

静态绑定，动态绑定；

##### 1.6.四种强制类型转换

**static_cast**: 用于编译时的类型转换。不进行运行时类型检查。适用于基本数据类型之间的转换、父子类指针/引用的转换（向上转型安全或向下转型）。不能用于去除 const 或 volatile 属性。

```
int a = 10;
double b = static_cast<double>(a); // 基本类型转换

class Base {};
class Derived : public Base {};
Base* basePtr = new Derived;
Derived* derivedPtr = static_cast<Derived*>(basePtr); // 向下转型，不安全
```

**daynamic_cast**: 用于运行时类型检查。主要用于多态类型（即基类有虚函数）的向下转型（从基类指针/引用转换为派生类指针/引用）。

如果转换失败，返回 nullptr（对指针）或抛出 std::bad_cast 异常（对引用）。需要 RTTI（Run-Time Type Information）支持，可能影响性能。

**const_cast**: 用于添加或移除 const 或 volatile 属性。不能改变变量的类型，只能改变其 const 或 volatile 属性。常用于修改原本是 const 的变量（需谨慎使用，避免未定义行为）。

**reinterpret_cast**: 用于低级别的类型转换，通常是将一种类型的指针或引用转换为另一种完全不相关的类型。不进行任何类型检查，可能导致未定义行为。通常用于底层编程（如硬件操作、序列化等）。

```
int a = 10;
int* ptr = &a;
char* charPtr = reinterpret_cast<char*>(ptr); // 将 int* 转换为 char*

uintptr_t address = reinterpret_cast<uintptr_t>(ptr); // 将指针转换为整数
```

##### 1.7.类对象内存模型

new/delete，malloc/free 区别；基本数据类型与引用数据类型；

new: 1.分配未初始化内存空间(malloc), 分配失败则抛 std：bad_alloc 2. 构造函数对空间初始化，构造对象异常时自动调用delete 释放内存；

delete：1.使用析构函数析构对象那个，2.回收内存 free

new得到经过初始化的空间，malloc未初始化，delete不仅释放空间还析构对象；new/delete 是操作符，malloc/free 库函数；

##### 1.8.volatile 和 extern

 define/const/typedef/inline 区别（编译/运行阶段/安全性/内存占用）；做一个关键字总结

##### 1.9.虚函数与虚函数表

虚函数指针；继承体系中虚函数表如何处理；虚析构函数，构造函数为什么不能为虚函数，虚表指针在构造函数中被初始化；构造函数或者析构函数中调用虚函数；纯虚函数；std::string 能否被继承，不能会内存泄漏；

编译器如何处理虚函数表；

- 拷贝基类的虚函数表，如果是多继承，就拷贝每个虚函数基类的虚函数表；
- 还有一个基类的虚函数表和派生类自身的虚函数表共用了一个虚函数表，也成为某个基类为派生类的主基类；
- 查看派生类中是否有重写基类中虚函数，如果有就替换成已经重写的虚函数地址；查看派生类是否有自身的虚函数，如果有就追加自身的虚函数到自身的虚函数表中。

![Snipaste_2025-03-02_17-27-39](D:\Blogs\wintonli.github.io\CppNote\Snipaste_2025-03-02_17-27-39.jpg)

10.深拷贝与浅拷贝；

是否从堆内存中另外申请空间来存储数据；

11.什么情况会调用拷贝构造，为什么拷贝构造必须要传递引用，不能值传递；构造和析构函数是否可以抛异常；

12.内存对齐；

13.初始化列表；

14.手写strcat/strcpy/strncpy/memset/memcopy;

```
char* my_strcat(char* dest, const char* src) {
    char* ptr = dest;
    // 找到目标字符串的末尾
    while (*ptr != '\0') {
        ptr++;
    }
    // 将源字符串复制到目标字符串末尾
    while (*src != '\0') {
        *ptr++ = *src++;
    }
    *ptr = '\0'; // 添加字符串结束符
    return dest;
}
```

```
char* my_strcpy(char* dest, const char* src) {
    char* ptr = dest;
    // 逐个字符复制
    while (*src != '\0') {
        *ptr++ = *src++;
    }
    *ptr = '\0'; // 添加字符串结束符
    return dest;
}
//考虑内存重叠的字符串拷贝函数，memmove
char* my_strcpy(char* dest, const char* src) {
	char* ptr = dest;
	memmove(dest, src, strlen(src)+1);
	return dest
}

```

```
char* my_strncpy(char* dest, const char* src, size_t n) {
    char* ptr = dest;
    size_t i = 0;
    // 复制最多 n 个字符
    while (i < n && *src != '\0') {
        *ptr++ = *src++;
        i++;
    }
    // 如果 n 大于源字符串长度，填充 '\0'
    while (i < n) {
        *ptr++ = '\0';
        i++;
    }
    return dest;
}
```

```
void* my_memset(void* dest, int ch, size_t count) {
    unsigned char* ptr = static_cast<unsigned char*>(dest);
    for (size_t i = 0; i < count; i++) {
        ptr[i] = static_cast<unsigned char>(ch);
    }
    return dest;
}
```

```
void* my_memcpy(void* dest, const void* src, size_t count) {
    unsigned char* d = static_cast<unsigned char*>(dest);
    const unsigned char* s = static_cast<const unsigned char*>(src);
    for (size_t i = 0; i < count; i++) {
        d[i] = s[i];
    }
    return dest;
}s
```

```
const char* my_strstr(const char* haystack, const char* needle) {
    if (*needle == '\0') {
        return haystack; // 如果 needle 是空字符串，返回 haystack
    }
    for (; *haystack != '\0'; haystack++) {
        // 检查当前位置是否匹配 needle 的第一个字符
        if (*haystack == *needle) {
            const char* h = haystack;
            const char* n = needle;

            // 逐个字符比较
            while (*h != '\0' && *n != '\0' && *h == *n) {
                h++;
                n++;
            }

            // 如果 needle 完全匹配，返回当前位置
            if (*n == '\0') {
                return haystack;
            }
        }
    }
    return nullptr; // 未找到
}
```

```
void* my_memmove(void* dest, const void* src, size_t n) {
    if (dest == nullptr || src == nullptr || n == 0) {
        return dest; // 处理空指针或长度为 0 的情况
    }
    unsigned char* d = static_cast<unsigned char*>(dest);
    const unsigned char* s = static_cast<const unsigned char*>(src);
    // 检查内存是否重叠
    if (d > s && d < s + n) {
        // 如果目标地址在源地址范围内，从后往前复制
        for (size_t i = n; i > 0; i--) {
            d[i - 1] = s[i - 1];
        }
    } else {
        // 如果没有重叠，从前往后复制
        for (size_t i = 0; i < n; i++) {
            d[i] = s[i];
        }
    }
    return dest;
}
```

15.大小端，大小端转换字符串；

```
大端模式（主机字节序）：高位字节存储在低地址，低位字节存储在高地址, 0x12345678 在内存中的存储顺序为：12 34 56 78。
小端模式（网络字节序）：低位字节存储在低地址，高位字节存储在高地址。0x12345678 在内存中的存储顺序为：78 56 34 12。
bool isLittleEndian() {
    int num = 1; // 0x00000001
    // 将 int 指针转换为 char 指针，检查第一个字节
    char* ptr = reinterpret_cast<char*>(&num);
    return (*ptr == 1); // 如果第一个字节是 1，说明是小端模式
}
// 将 32 位整数从主机字节序转换为网络字节序
uint32_t htonl(uint32_t hostLong) {
    return ((hostLong & 0xFF000000) >> 24) |
           ((hostLong & 0x00FF0000) >> 8) |
           ((hostLong & 0x0000FF00) << 8) |
           ((hostLong & 0x000000FF) << 24);
}
#include <arpa/inet.h> // 包含 htons, htonl, ntohs, ntohl
int main() {
    uint16_t hostShort = 0x1234;
    uint32_t hostLong = 0x12345678;
    uint16_t netShort = htons(hostShort);
    uint32_t netLong = htonl(hostLong);
    }
```

16.Lambda 表达式；语法糖/函数对象/仿函数；

17.左值引用，右值引用，引用折叠；

18.类如何实现只能在堆/栈上创建

(1)只能在堆上创建

​    编译器在为类对象分配栈空间时,会先检查类的析构函数的访问性,其实不光是析构函数,只要是非静态的函

数,编译器都会进行检查。如果类的析构函数是私有的,则编译器不会在栈空间上为类对象分配内存。

(2)只能在栈上创建

​    只需要 让new操作符无法使用即可,要做到这件事,我们可以将 new操作符重载并设置为私有访问即可。

重载new的同时最好重载delete。

19.初始化列表初始化

##### 2.1C++1新特性

2.1 nullptr

2.2 Lambda 表达式

2.3 右值引用

2.4 constexpr

2.5 初始化列表

2.6 auto/decltype

2.7 for each

2.8 构造函数委托

2.9 final/override

2.10 default/delete

2.11 static_assert

2.12 智能指针

2.13 正则表达式

2.14 元组

2.15 哈希表 unordered_set/multiset/map/multimap

##### 3.1STL 容器与使用方法

3.1容器，算法，迭代器，仿函数，配接器，配置器；

![Snipaste_2025-03-02_20-24-41](D:\Blogs\wintonli.github.io\CppNote\Snipaste_2025-03-02_20-24-41.jpg)

3.2.STL 容器与使用方法；vector/list/deque/stack/queue/heap/priority_queue/map/set 迭代器失效问题；

3.3 vector 的自己实现；

#### 3.数据结构与算法

1.二叉树，二叉搜索树，平衡二叉树、AVL；

2.红黑树；

3.冒泡/选择/插入/.....排序算法；

![Snipaste_2025-03-02_20-13-43](D:\Blogs\wintonli.github.io\CppNote\Snipaste_2025-03-02_20-13-43.jpg)



4.二叉树前/中/后序遍历；

5.单例/工厂、观察者、装饰器模式，生产者消费者模型，MVC,MVVM模型；

#### 4.Android 基础

#### 5.操作系统基础

1.预处理、编译、汇编、链接；

2.动态编译，静态编译，动态链接，静态链接;

3.fork/wait/exec

4.进程间通信方式，线程间通信方式

5.页段，虚拟内存

#### 6.计算机网络

#### 7.设计模式

1.单例模式/工厂模式/观察者模式/装饰器模式



