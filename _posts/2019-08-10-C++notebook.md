---
layout:     post                    # 使用的布局（不需要改）
title:      My First Post           # 标题 
subtitle:   C++笔记                  #副标题
date:       2019-08-10              # 时间
author:     WHY                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 学习
---

## 一：继承和派生
### 1\继承和派生
   - class 基类名
   - {}；
   - class 派生类名：public 基类名
   - {}；
   -【注：派生类对象中包含基类对象（变量+函数），体积为所有对象的体积之和】
### 2\复合关系“有”和继承关系”是“
   - 复合关系：
  * 单向复合写法：
      - class 类1{  
         - 对象；
      - }；
      - class 类2{
         - 类1 对象；
      - }；
  - 【注：切记循环定义，即互相复合】
  * 互相复合写法：
      - class 类2；
      - class 类1{
         - 类2 *对象1；
      - }；
      - class 类2{
         - 类1 *对象2;
      - };
### 3\当派生类与基类同名
   - 1>派生类不能访问基类的私有成员，【一般不推荐同名】
     - class 基类
      - {
         - i；
      - }；
      - class 派生类:public 基类
      - {
         - i
      - }；
      - void 派生类::成员函数（）
      -{
         - i=5；         // 派生类的对象
         - 基类::i=5；   // 基类的对象
      - }
      - 派生类 对象；
      - 对象.i=1；       // 是派生类对象赋值
      - 对象.基类::i=1;
### 4\访问范围说明
   - 1>基类private成员被访问：
      - 基类的成员函数
      - 基类的友元函数
   - 2>基类public成员被访问：
      - 基类/派生类的成员/友元函数
      - 其它函数
   - 3>基类的protected成员被访问：
      - 基类成员/友元函数
      * 派生类成员函数可以访问【当前对象】的基类的保护成员
### 5\派生类的构造函数
   * 派生类对象包含基类对象
   * 执行派生类构造函数之前：
      - 1>先执行基类的构造函数
        - 初始化【派生类对象中从基类继承的成员】
      - 2>调用成员对象类的构造函数
        - 初始化【派生类对象中的成员对象】
   * 执行完派生类的析构函数后，自动调用基类的析构函数
   * 派生类交代基类初始化【即先初始化基类的对象】，具体形式：
      - 派生类::构造函数名(形参表):基类名(基类构造函数实参表) // 初始化列表里可以出现派生类构造函数的参数、常量
      - {
      - }
    * 调用基类构造函数的两种形式
      - 显式：派生类::构造函数名(形参表):基类名(基类构造函数实参表)
      - 隐式：省略构造函数时，自动调用基类的默认构造函数
### 6\public继承的赋值兼容规则（仅适用于public）
   * 派生类对象可以【赋值给基类对象，反之不行】
   * 派生类对象可以【初始化基类引用】
   * 派生类对象的【地址可以赋值给基类指针】
   
   * 直接基类与间接基类：声明派生类只需列出基类    
## 二：多态与虚函数
### 1\虚函数：实现多态的机制。允许在派生类中重新定义与基类同名的函数，并且可以通过【基类指针】或【引用】来访问基类和派生类中的【同名函数】。
   * 类定义中，前面有virtual关键字的成员函数【只能用在类定义中】
   - class base{
      - virtual int get();
   - };
   - int base::get(){}
   - 【构造函数和静态成员函数不能是虚函数】
### 2\多态【增强程序的可扩充性，即程序改动和增加的代码较少】
    - 1>*派生类指针可以赋值给基类
      - *基类指针调用同名虚函数（指向指针的指针用 **）
         - 对象->虚函数； // 调用哪个虚函数取决于p指向哪种类型的对象
    - 2>基类引用调用同名虚函数
         - 对象.虚函数；  // 调用哪个虚函数取决于r引用哪种类型的对象
 - 【用'基类指针数组'存放指向'各种派生类对象的指针'，然后遍历该数组，就能对各个派生类对象做各种操作，常用操作】
  - 【在非构造函数，非析构函数的成员函数中调用虚函数，是多态！！！否则不是多态】   
   - 【派生类中和基类中虚函数同名同参数表的函数，不加virtual也自动成为虚函数】  
## 三：文件操作
### 1\c++标准库：ifstream（读） ofstream（写） fstream（读写） 3个文件流类



## 四：多线程
### 1、<mutex>
   - mutex互斥锁是一个可锁的对象，它被设计成在关键代码段需要独占访问时发出信号，从而【阻止具有相同保护其它线程并发执行】并【访问相同的内存位置】
   - std::mutex 
      - 不允许拷贝；互斥对象不能被复制移动，初始状态为锁定
         - default（1） constexpr mutex() noexcept;
         - copy[deleted](2)mutex (const mutex&) = delete;
      - lock锁住互斥对象
         - 如果互斥对象当前没有被任何线程锁定，则调用线程锁定它（从这一点开始，直到它的成员解锁被调用，线程拥有互斥对象）
         - 如果互斥当前被另一个线程锁定，则调用线程的执行将被阻塞，直到其它线程解锁（其它非锁定线程继续执行它们）
         - 如果互斥锁当前被相同的线程锁定，调用此函数，则会产生死锁（未定义的行为）
   - unique_lock  提供上锁和解锁
      - 定义：template<class Mutex> class unique_lock;
      - unique_lock对象一独占所有全的方式，管理mutex对象的上锁和解锁操作，独占所有权，就没有其它的unique_lock对象同时拥有某个mutex对象的所有权
      - unique_lock对象在析构的时候一定保证互斥量为解锁状态；因此他做为自动持续时间的对象特别有用，因为它保证在抛出异常时，互斥对象被正确解锁，不过，注意unique_lock 对象并不以任何方式管理互斥对象的生命周期，互斥对象的持续时间将延长至少知道unique_lock管理它的毁灭。
      * 构造函数（constructor)
         - default (1)  unique_lock() noexcept;
            -  默认构造函数：unique_lock 对象不管理任何Mutex对象m
         - locking (2)  explicit unique_lock (mutex_type& m);
            - locking初始化：【unique_lock 对象管理Mutex对象m，并调用m.lock()对Mutex对象进行上锁，如果其它unique_lock对象管理了m，该线程将会被阻塞。】
         - try-locking (3)  unique_lock (mutex_type& m, try_to_lock_t tag);
            - try-locking 初始化： unique_lock对象管理 Mutex 对象 m，并调用 m.try_lock() 对 Mutex 对象进行上锁，但如果上锁不成功，不会阻塞当前线程
         - deferred (4) unique_lock (mutex_type& m, defer_lock_t tag) noexcept;
            - unique_lock 对象管理 Mutex 对象 m并不锁住m。 m 是一个没有被当前线程锁住的 Mutex 对象。
         - adopting (5) unique_lock (mutex_type& m, adopt_lock_t tag);
            - unique_lock 对象管理 Mutex 对象 m， m 应该是一个已经被当前线程锁住的 Mutex 对象。(当前unique_lock 对象拥有对锁(lock)的所有权)。
         - locking for (6) template <class Rep, class Period>
            - 新创建的 unique_lock 对象管理 Mutex 对象 m，通过调用 m.try_lock_for(rel_time) 来锁住 Mutex 对象一段时间(rel_time)。
         - unique_lock (mutex_type& m, const chrono::duration<Rep,Period>& rel_time);
         - locking until (7) template <class Clock, class Duration>
         - unique_lock (mutex_type& m, const chrono::time_point<Clock,Duration>& abs_time);
         - copy [deleted] (8)  unique_lock (const unique_lock&) = delete;
            - unique_lock 对象不能被拷贝构造
         - move (9)  unique_lock (unique_lock&& x);
            - 新创建的 unique_lock 对象获得了由 x 所管理的 Mutex 对象的所有权(包括当前 Mutex 的状态)。调用 move 构造之后， x 对象如同通过默认构造函数所创建的，就不再管理任何 Mutex 对象了
- 链接：https://www.jianshu.com/p/8ff671d22aa8
         

   









