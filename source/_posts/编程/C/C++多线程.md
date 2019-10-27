---
title: C++多线程
author: 陈德强
date: 2019-10-05 16:00:00
categories:
- 笔记
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: C++多线程。
---



C++11 新标准中引入了四个头文件来支持多线程编程，他们分别是`<atomic> ,<thread>,<mutex>,<condition_variable>和<future>`。

`<atomic>`：该头文主要声明了两个类, std::atomic 和 std::atomic_flag，另外还声明了一套 C 风格的原子类型和与 C 兼容的原子操作的函数。 
`<thread>`：该头文件主要声明了 std::thread 类，另外 std::this_thread 命名空间也在该头文件中。 
`<mutex>`：该头文件主要声明了与互斥量(mutex)相关的类，包括 std::mutex 系列类，std::lock_guard, std::unique_lock, 以及其他的类型和函数。 
`<condition_variable>`：该头文件主要声明了与条件变量相关的类，包括 std::condition_variable 和 std::condition_variable_any。 
`<future>`：该头文件主要声明了 std::promise, std::package_task 两个 Provider 类，以及 std::future 和 std::shared_future 两个 Future 类，另外还有一些与之相关的类型和函数，std::async() 函数就声明在此头文件中。

# 一、<thread>库
|函数|作用|
|:---:|:---:|
|std::thread::thread(Function&& f, Args&&... args);|创建线程|
|std::thread::join();|等待线程结束|
|std::thread::detach();|脱离线程控制|
|std::thread::swap(thread& other);|交换线程|

## 1.1 基本用法
```c
#include <iostream>  
#include <thread>  

using namespace std;

void th_function()
{
   std::cout << "hello thread." << std::endl;
}

int main(int argc, char* argv[])
{
   std::thread t(th_function);
   t.join();

   return 0;
}
```
```
hello thread.
```

## 1.2 线程参数
```c
#include <thread>
#include <iostream>

void hello(const char *name) {
    std::cout << "Hello " << name << std::endl;
}

int main() {
    std::thread thread(hello, "C++11");
    thread.join();

    return 0;
}
```
```
Hello C++11
```

## 1.3 类成员函数做为线程入口
类成员函数做为线程入口时，仍然十分简单: 把this做为第一个参数传递进去即可。
```c
#include <thread>
#include <iostream>

class Greet
{
    const char *owner = "Greet";
public:
    void SayHello(const char *name) {
        std::cout << "Hello " << name << " from " << this->owner << std::endl;
    }
};
int main() {
    Greet greet;

    std::thread thread(&Greet::SayHello, &greet, "C++11");
    thread.join();

    return 0;
}
```
```
Hello C++11 from Greet
```
## 1.4 线程暂停
从外部让线程暂停，会引发很多并发问题。大家可以百度一下，此处不做引申。这大概也是std::thread并没有直接提供pause函数的原因。但有时线程在运行时，确实需要“停顿”一段时间怎么办呢？可以使用std::this_thread::sleep_for或std::this_thread::sleep_until
```c
#include <thread>
#include <iostream>
#include <chrono>

using namespace std::chrono;

void pausable() {
    // sleep 500毫秒
    std::this_thread::sleep_for(milliseconds(500));
    // sleep 到指定时间点
    std::this_thread::sleep_until(system_clock::now() + milliseconds(500));
}

int main() {
    std::thread thread(pausable);
    thread.join();

    return 0;
}
```
## 1.5 线程停止
一般情况下当线程函数执行完成后，线程“自然”停止。但在std::thread中有一种情况会造成线程异常终止，那就是：析构。当std::thread实例析构时，如果线程还在运行，则线程会被强行终止掉，这可能会造成资源的泄漏，因此尽量在析构前join一下，以确保线程成功结束。
如果确实想提前让线程结束怎么办呢？一个简单的方法是使用“共享变量”，线程定期地去检测该量，如果需要退出，则停止执行，退出线程函数。使用“共享变量”需要注意，在多核、多CPU的情况下需要使用“原子”操作，关于原子操作后面会有专题讲述。
## 1.6 拷贝
```c
std::thread a(foo);
std::thread b;
b = a;
```
当执行以上代码时，会发生什么？最终foo线程是由a管理，还是b来管理？答案是由b来管理。std::thread被设计为只能由一个实例来维护线程状态，以及对线程进行操作。因此当发生赋值操作时，会发生线程所有权转移。在macos下std::thread的赋值函数原型为:
```c
thread& operator=(thread&& a);
```
赋完值后，原来由a管理的线程改为由b管理，b不再指向任何线程(相当于执行了detach操作)。如果b原本指向了一个线程，那么这个线程会被终止掉。
## detach/joinable
detach是std::thread的成员函数，函数原型为：
```c
void detach();
bool joinable() const;
```
detach以后就失去了对线程的所有权，不能再调用join了，因为线程已经分离出去了，不再归该实例管了。判断线程是否还有对线程的所有权的一个简单方式是调用joinable函数，返回true则有，否则为无。

## 1.7 线程内部调用自身的join
自己等待自己执行结束？如果程序员真这么干，那这个程序员一定是脑子短路了。对于这种行为C++11只能抛异常了。

## 1.8 get_id

每个线程都有一个id，但此处的get_id与系统分配给线程的ID并不一是同一个东东。如果想取得系统分配的线程ID，可以调用native_handle函数。

## 1.9 逻辑运算

有些平台下std::thread还支持若干逻辑运算，比如Visual C++, 但这并不是标准库的行为，不要在跨平台的场景中使用。

# 二、<mutex>库
mutex又称互斥量，用于提供对共享变量的互斥访问。C++11中mutex相关的类都在<mutex>头文件中。共四种互斥类:

|序号|	名称|	用途|
|:---:|:---:|:---:|
|1|	std::mutex|	最基本也是最常用的互斥类|
|2|	std::recursive_mutex|	同一线程内可递归(重入)的互斥类|
|3|	std::timed_mutex	|除具备mutex功能外，还提供了带时限请求锁定的能力|
|4|	std::recursive_timed_mutex|	同一线程内可递归(重入)的timed_mutex|

与std::thread一样，mutex相关类不支持拷贝构造、不支持赋值。同时mutex类也不支持move语义(move构造、move赋值)。不用担心会误用这些操作，真要这么做了的话，编译器会阻止你的。

## 2.1  lock

锁住互斥量。调用lock时有三种情况:

1. 如果互斥量没有被锁住，则调用线程将该mutex锁住，直到调用线程调用unlock释放。
2. 如果mutex已被其它线程lock，则调用线程将被阻塞，直到其它线程unlock该mutex。
3. 如果当前mutex已经被调用者线程锁住，则std::mutex死锁，而recursive系列则成功返回。

## 2.2 try_lock
尝试锁住mutex，调用该函数同样也有三种情况：

1. 如果互斥量没有被锁住，则调用线程将该mutex锁住(返回true)，直到调用线程调用unlock释放。
2. 如果mutex已被其它线程lock，则调用线程将失败，并返回false。
3. 如果当前mutex已经被调用者线程锁住，则std::mutex死锁，而recursive系列则成功返回true。

## 2.3 unlock

解锁mutex，释放对mutex的所有权。值得一提的时，对于recursive系列mutex，unlock次数需要与lock次数相同才可以完全解锁。
下面给出一个mutex小例子
```c
#include <iostream>
#include <thread>
#include <mutex>

void inc(std::mutex &mutex, int loop, int &counter) {
    for (int i = 0; i < loop; i++) {
        mutex.lock();
        ++counter;
        mutex.unlock();
    }
}
int main() {
    std::thread threads[5];
    std::mutex mutex;
    int counter = 0;

    for (std::thread &thr: threads) {
        thr = std::thread(inc, std::ref(mutex), 1000, std::ref(counter));
    }
    for (std::thread &thr: threads) {
        thr.join();
    }

    // 输出：5000，如果inc中调用的是try_lock，则此处可能会<5000
    std::cout << counter << std::endl;

    return 0;
}
```

## 2.4 try_lock_for, try_lock_until
这两个函数仅用于timed系列的mutex(std::timed_mutex, std::recursive_timed_mutex)，函数最多会等待指定的时间，如果仍未获得锁，则返回false。除超时设定外，这两个函数与try_lock行为一致。
```c
// 等待指定时长
template <class Rep, class Period>
    try_lock_for(const chrono::duration<Rep, Period>& rel_time);
// 等待到指定时间
template <class Clock, class Duration>
    try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);
```
try_lock_for相关代码
```c
#include <iostream>
#include <thread>
#include <mutex>
#include <chrono>

void run500ms(std::timed_mutex &mutex) {
    auto _500ms = std::chrono::milliseconds(500);
    if (mutex.try_lock_for(_500ms)) {
        std::cout << "获得了锁" << std::endl;
    } else {
        std::cout << "未获得锁" << std::endl;
    }
}
int main() {
    std::timed_mutex mutex;

    mutex.lock();
    std::thread thread(run500ms, std::ref(mutex));
    thread.join();
    mutex.unlock();

    return 0;
}
//输出：未获得锁
```
## 2.5 lock_guard
lock_guard利用了C++ RAII的特性，在构造函数中上锁，析构函数中解锁。lock_guard是一个模板类，其原型为
```
template <class Mutex> class lock_guard
```
模板参数Mutex代表互斥量，可以是上一篇介绍的std::mutex, std::timed_mutex, std::recursive_mutex, std::recursive_timed_mutex中的任何一个，也可以是std::unique_lock(下面即将介绍)，这些都提供了lock和unlock的能力。
lock_guard仅用于上锁、解锁，不对mutex承担供任何生周期的管理，因此在使用的时候，请确保lock_guard管理的mutex一直有效。
同其它mutex类一样，locak_guard不允许拷贝，即拷贝构造和赋值函数被声明为delete。
```
lock_guard(lock_guard const&) = delete;
lock_guard& operator=(lock_guard const&) = delete;
```
lock_guard的设计保证了即使程序在锁定期间发生了异常，也会安全的释放锁，不会发生死锁。
```c
#include <iostream>
#include <mutex>

std::mutex mutex;

void safe_thread() {
    try {
        std::lock_guard<std::mutex> _guard(mutex);
        throw std::logic_error("logic error");
    } catch (std::exception &ex) {
        std::cerr << "[caught] " << ex.what() << std::endl;
    }
}
int main() {
    safe_thread();
    // 此处仍能上锁
    mutex.lock();
    std::cout << "OK, still locked" << std::endl;
    mutex.unlock();

    return 0;
}
```
```
[caught] logic error
OK, still locked 
```
## 2.6  unique_lock
lock_guard提供了简单上锁、解锁操作，但当我们需要更灵活的操作时便无能为力了。这些就需要unique_lock上场了。unique_lock拥有对Mutex的所有权，一但初始化了unique_lock，其就接管了该mutex, 在unique_lock结束生命周期前(析构前)，其它地方就不要再直接使用该mutex了。unique_lock提供的功能较多，此处不一一列举，下面列出unique_lock的类声明，及部分注释，供大家参考
```c
template <class Mutex>
class unique_lock
{
public:
    typedef Mutex mutex_type;
    // 空unique_lock对象
    unique_lock() noexcept;
    // 管理m, 并调用m.lock进行上锁，如果m已被其它线程锁定，由该构造了函数会阻塞。
    explicit unique_lock(mutex_type& m);
    // 仅管理m，构造函数中不对m上锁。可以在初始化后调用lock, try_lock, try_lock_xxx系列进行上锁。
    unique_lock(mutex_type& m, defer_lock_t) noexcept;
    // 管理m, 并调用m.try_lock，上锁不成功不会阻塞当前线程
    unique_lock(mutex_type& m, try_to_lock_t);
    // 管理m, 该函数假设m已经被当前线程锁定，不再尝试上锁。
    unique_lock(mutex_type& m, adopt_lock_t);
    // 管理m, 并调用m.try_lock_unitil函数进行加锁
    template <class Clock, class Duration>
        unique_lock(mutex_type& m, const chrono::time_point<Clock, Duration>& abs_time);
    // 管理m，并调用m.try_lock_for函数进行加锁
    template <class Rep, class Period>
        unique_lock(mutex_type& m, const chrono::duration<Rep, Period>& rel_time);
    // 析构，如果此前成功加锁(或通过adopt_lock_t进行构造)，并且对mutex拥有所有权，则解锁mutex
    ~unique_lock();

    // 禁止拷贝操作
    unique_lock(unique_lock const&) = delete;
    unique_lock& operator=(unique_lock const&) = delete;

    // 支持move语义
    unique_lock(unique_lock&& u) noexcept;
    unique_lock& operator=(unique_lock&& u) noexcept;

    void lock();
    bool try_lock();

    template <class Rep, class Period>
        bool try_lock_for(const chrono::duration<Rep, Period>& rel_time);
    template <class Clock, class Duration>
        bool try_lock_until(const chrono::time_point<Clock, Duration>& abs_time);

    // 显示式解锁，该函数调用后，除非再次调用lock系列函数进行上锁，否则析构中不再进行解锁
    void unlock();

    // 与另一个unique_lock交换所有权
    void swap(unique_lock& u) noexcept;
    // 返回当前管理的mutex对象的指针，并释放所有权
    mutex_type* release() noexcept;

    // 当前实例是否获得了锁
    bool owns_lock() const noexcept;
    // 同owns_lock
    explicit operator bool () const noexcept;
    // 返回mutex指针，便于开发人员进行更灵活的操作
    // 注意：此时mutex的所有权仍归unique_lock所有，因此不要对mutex进行加锁、解锁操作
    mutex_type* mutex() const noexcept;
};
```
## 2.7 std::call_once
该函数的作用顾名思义：保证call_once调用的函数只被执行一次。该函数需要与std::once_flag配合使用。std::once_flag被设计为对外封闭的，即外部没有任何渠道可以改变once_flag的值，仅可以通过std::call_once函数修改。一般情况下我们在自己实现call_once效果时，往往使用一个全局变量，以及双重检查锁(DCL)来实现，即便这样该实现仍然会有很多坑(多核环境下)。有兴趣的读者可以搜索一下DCL来看，此处不再赘述。
C++11为我们提供了简便的解决方案，所需做的仅仅像下面这样使用即可。
```c
#include <iostream>
#include <thread>
#include <mutex>

void initialize() {
    std::cout << __FUNCTION__ << std::endl;
}

std::once_flag of;
void my_thread() {
    std::call_once(of, initialize);
}

int main() {
    std::thread threads[10];
    for (std::thread &thr: threads) {
        thr = std::thread(my_thread);
    }
    for (std::thread &thr: threads) {
        thr.join();
    }
    return 0;
}
// 仅输出一次：initialize
```
## 2.8 std::try_lock
当有多个mutex需要执行try_lock时，该函数提供了简便的操作。try_lock会按参数从左到右的顺序，对mutex顺次执行try_lock操作。当其中某个mutex.try_lock失败(返回false或抛出异常)时，已成功锁定的mutex都将被解锁。
需要注意的是，该函数成功时返回-1， 否则返回失败mutex的索引，索引从0开始计数。
```c
template <class L1, class L2, class... L3>
  int try_lock(L1&, L2&, L3&...);
```
## 2.9 std::lock

std::lock是较智能的上批量上锁方式，采用死锁算法来锁定给定的mutex列表，避免死锁。该函数对mutex列表的上锁顺序是不确定的。该函数保证: 如果成功，则所有mutex全部上锁，如果失败，则全部解锁。
```c
template <class L1, class L2, class... L3>
  void lock(L1&, L2&, L3&...);
```
# 三、条件变量
前面我们介绍了线程(std::thread)和互斥量(std::mutex)，互斥量是多线程间同时访问某一共享变量时，保证变量可被安全访问的手段。在多线程编程中，还有另一种十分常见的行为：线程同步。线程同步是指线程间需要按照预定的先后次序顺序进行的行为。C++11对这种行为也提供了有力的支持，这就是条件变量。条件变量位于头文件condition_variable下。本章我们将简要介绍一下该类，在文章的最后我们会综合运用std::mutex和std::condition_variable，实现一个chan类，该类可在多线程间安全的通信，具有广泛的应用场景。

## 3.1 std::condition_variable
条件变量提供了两类操作：wait和notify。这两类操作构成了多线程同步的基础。
### 3.1.1 wait
wait是线程的等待动作，直到其它线程将其唤醒后，才会继续往下执行。下面通过伪代码来说明其用法：
```c
std::mutex mutex;
std::condition_variable cv;

// 条件变量与临界区有关，用来获取和释放一个锁，因此通常会和mutex联用。
std::unique_lock lock(mutex);
// 此处会释放lock，然后在cv上等待，直到其它线程通过cv.notify_xxx来唤醒当前线程，cv被唤醒后会再次对lock进行上锁，然后wait函数才会返回。
// wait返回后可以安全的使用mutex保护的临界区内的数据。此时mutex仍为上锁状态
cv.wait(lock)
```
需要注意的一点是, wait有时会在没有任何线程调用notify的情况下返回，这种情况就是有名的[**spurious wakeup**](https://docs.microsoft.com/zh-cn/windows/desktop/api/synchapi/nf-synchapi-sleepconditionvariablecs)。因此当wait返回时，你需要再次检查wait的前置条件是否满足，如果不满足则需要再次wait。wait提供了重载的版本，用于提供前置检查。
```c
template <typename Predicate>
void wait(unique_lock<mutex> &lock, Predicate pred) {
    while(!pred()) {
        wait(lock);
    }
}
```
除wait外, 条件变量还提供了wait_for和wait_until，这两个名称是不是看着有点儿眼熟，std::mutex也提供了_for和_until操作。在C++11多线程编程中，需要等待一段时间的操作，一般情况下都会有xxx_for和xxx_until版本。前者用于等待指定时长，后者用于等待到指定的时间。
### 3.1.2
了解了wait，notify就简单多了：唤醒wait在该条件变量上的线程。notify有两个版本：notify_one和notify_all。

*   notify_one 唤醒等待的一个线程，注意只唤醒一个。
*   notify_all 唤醒所有等待的线程。使用该函数时应避免出现[惊群效应](https://blog.csdn.net/lyztyycode/article/details/78648798?locationNum=6&fps=1)。

其使用方式见下例：
```c
std::mutex mutex;
std::condition_variable cv;

std::unique_lock lock(mutex);
// 所有等待在cv变量上的线程都会被唤醒。但直到lock释放了mutex，被唤醒的线程才会从wait返回。
cv.notify_all(lock)
```
## 3.2 线程间通信
有了上面的基础我们就可以设计我们的线程间通讯工具"chan"了。我们的设计目标：

在线程间安全的传递数据。golang社区有一句经典的话：不要通过共享内存来通信，要通过通信来共享内存。
消除线程线程同步带来的复杂性。
我们先来看一下chan的实际使用效果, 生产者-消费者（一个生产者，多个消费者）
```c
#include <stdio.h>
#include <thread>
#include "chan.h"  // chan的头文件

using namespace std::chrono;

// 消费数据 
void consume(chan<int> ch, int thread_id) {
    int n;
    while(ch >> n) {
        printf("[%d] %d\n", thread_id, n);
        std::this_thread::sleep_for(milliseconds(100));
    }
}

int main() {
    chan<int> chInt(3);
    
    // 消费者
    std::thread consumers[5];
    for (int i = 0; i < 5; i++) {
        consumers[i] = std::thread(consume, chInt, i+1);
    }

    // 生产数据 
    for (int i = 0; i < 16; i++) {
        chInt << i;
    }
    chInt.close();  // 数据生产完毕

    for (std::thread &thr: consumers) {
        thr.join();
    }

    return 0;
}
```

# 四、异步运行之std::promise
前面介绍了C++11的std::thread、std::mutex以及std::condition_variable，并实现了一个多线程通信的chan类，虽然由于篇幅的限制，该实现有些简陋，甚至有些缺陷，但对于一般情况应该还是够用了。在C++11多线程系列的最后会献上chan的最终版本，敬请期待。
本文将介绍C++11的另一大特性：异步运行(std::async)。async顾名思义是将一个函数A移至另一线程中去运行。A可以是静态函数、全局函数，甚至类成员函数。在异步运行的过程中，如果A需要向调用者输出结果怎么办呢？std::async完美解决了这一问题。在了解async的解决之道前，我们需要一些知识储备，那就是：std::promise、std::packaged_task和std::future。异步运行涉及的内容较多，我们会分几节来讲。

std::promise是一个模板类: template<typename R> class promise。其泛型参数R为std::promise对象保存的值的类型，R可以是void类型。std::promise保存的值可被与之关联的std::future读取，读取操作可以发生在其它线程。std::promise允许move语义(右值构造，右值赋值)，但不允许拷贝(拷贝构造、赋值)，std::future亦然。std::promise和std::future合作共同实现了多线程间通信。

## 4.1 设置std::promise的值
通过成员函数set_value可以设置std::promise中保存的值，该值最终会被与之关联的std::future::get读取到。需要注意的是：set_value只能被调用一次，多次调用会抛出std::future_error异常。事实上std::promise::set_xxx函数会改变std::promise的状态为ready，再次调用时发现状态已要是reday了，则抛出异常。
```c
#include <iostream> // std::cout, std::endl
#include <thread>   // std::thread
#include <string>   // std::string
#include <future>   // std::promise, std::future
#include <chrono>   // seconds
using namespace std::chrono;

void read(std::future<std::string> *future) {
    // future会一直阻塞，直到有值到来
    std::cout << future->get() << std::endl;
}

int main() {
    // promise 相当于生产者
    std::promise<std::string> promise;
    // future 相当于消费者, 右值构造
    std::future<std::string> future = promise.get_future();
    // 另一线程中通过future来读取promise的值
    std::thread thread(read, &future);
    // 让read等一会儿:)
    std::this_thread::sleep_for(seconds(1));
    // 
    promise.set_value("hello future");
    // 等待线程执行完成
    thread.join();

    return 0;
}
// 控制台输: hello future
```
与std::promise关联的std::future是通过std::promise::get_future获取到的，自己构造出来的无效。一个std::promise实例只能与一个std::future关联共享状态，当在同一个std::promise上反复调用get_future会抛出future_error异常。
共享状态。在std::promise构造时，std::promise对象会与共享状态关联起来，这个共享状态可以存储一个R类型的值或者一个由std::exception派生出来的异常值。通过std::promise::get_future调用获得的std::future与std::promise共享相同的共享状态。

## 4.2 当std::promise不设置值时
如果promise直到销毁时，都未设置过任何值，则promise会在析构时自动设置为std::future_error，这会造成std::future.get抛出std::future_error异常。
```c
#include <iostream> // std::cout, std::endl
#include <thread>   // std::thread
#include <future>   // std::promise, std::future
#include <chrono>   // seconds
using namespace std::chrono;

void read(std::future<int> future) {
    try {
        future.get();
    } catch(std::future_error &e) {
        std::cerr << e.code() << "\n" << e.what() << std::endl;
    }
}

int main() {
    std::thread thread;
    {
        // 如果promise不设置任何值
        // 则在promise析构时会自动设置为future_error
        // 这会造成future.get抛出该异常
        std::promise<int> promise;
        thread = std::thread(read, promise.get_future());
    }
    thread.join();

    return 0;
}
```
```
future:4
The associated promise has been destructed prior to the associated state becoming ready.
```
## 4.3 通过std::promise让std::future抛出异常
通过std::promise::set_exception函数可以设置自定义异常，该异常最终会被传递到std::future，并在其get函数中被抛出。
```c
#include <iostream>
#include <future>
#include <thread>
#include <exception>  // std::make_exception_ptr
#include <stdexcept>  // std::logic_error

void catch_error(std::future<void> &future) {
    try {
        future.get();
    } catch (std::logic_error &e) {
        std::cerr << "logic_error: " << e.what() << std::endl;
    }
}

int main() {
    std::promise<void> promise;
    std::future<void> future = promise.get_future();

    std::thread thread(catch_error, std::ref(future));
    // 自定义异常需要使用make_exception_ptr转换一下
    promise.set_exception(
        std::make_exception_ptr(std::logic_error("caught")));
    
    thread.join();
    return 0;
}
// 输出：logic_error: caught
```
std::promise虽然支持自定义异常，但它并不直接接受异常对象：
```c
// std::promise::set_exception函数原型
void set_exception(std::exception_ptr p);
```
自定义异常可以通过位于头文件exception下的std::make_exception_ptr函数转化为std::exception_ptr。
## 4.4 std::promise<void>
通过上面的例子，我们看到std::promise<void>是合法的。此时std::promise.set_value不接受任何参数，仅用于通知关联的std::future.get()解除阻塞。
## 4.5 std::promise所在线程退出时
std::async(异步运行)时，开发人员有时会对std::promise所在线程退出时间比较关注。std::promise支持定制线程退出时的行为：

std::promise::set_value_at_thread_exit 线程退出时，std::future收到通过该函数设置的值。
std::promise::set_exception_at_thread_exit 线程退出时，std::future则抛出该函数指定的异常。
关于std::promise就是这些，本文从使用角度介绍了std::promise的能力以及边界，读者如果想更深入了解该类，可以直接阅读一下源码。

# 五、异步运行之std::packaged_task
上一篇介绍的std::promise通过set_value可以使得与之关联的std::future获取数据。本篇介绍的std::packaged_task则更为强大，它允许传入一个函数，并将函数计算的结果传递给std::future，包括函数运行时产生的异常。下面我们就来详细介绍一下它。

在开始std::packaged_task之前我们先看一段代码，对std::packaged_task有个直观的印象，然后我们再进一步介绍。
```c
#include <thread>   // std::thread
#include <future>   // std::packaged_task, std::future
#include <iostream> // std::cout

int sum(int a, int b) {
    return a + b;
}

int main() {
    std::packaged_task<int(int,int)> task(sum);
    std::future<int> future = task.get_future();

    // std::promise一样，std::packaged_task支持move，但不支持拷贝
    // std::thread的第一个参数不止是函数，还可以是一个可调用对象，即支持operator()(Args...)操作
    std::thread t(std::move(task), 1, 2);
    // 等待异步计算结果
    std::cout << "1 + 2 => " << future.get() << std::endl;

    t.join();
    return 0;
}
/// 输出: 1 + 2 => 3
```
std::packaged_task位于头文件#include <future>中，是一个模板类

```c
template <class R, class... ArgTypes>
class packaged_task<R(ArgTypes...)>
```
其中R是一个函数或可调用对象，ArgTypes是R的形参。与std::promise一样，std::packaged_task支持move，但不支持拷贝(copy)。std::packaged_task封装的函数的计算结果会通过与之联系的std::future::get获取(当然，可以在其它线程中异步获取)。关联的std::future可以通过std::packaged_task::get_future获取到，get_future仅能调用一次，多次调用会触发std::future_error异常。
std::package_task除了可以通过可调用对象构造外，还支持缺省构造(无参构造)。但此时构造的对象不能直接使用，需通过右值赋值操作设置了可调用对象或函数后才可使用。判断一个std::packaged_task是否可使用，可通过其成员函数valid来判断。
## 5.1 std::packaged_task::valid
该函数用于判断std::packaged_task对象是否是有效状态。当通过缺省构造初始化时，由于其未设置任何可调用对象或函数，valid会返回false。只有当std::packaged_task设置了有效的函数或可调用对象，valid才返回true。
```c
#include <future>   // std::packaged_task, std::future
#include <iostream> // std::cout

int main() {
    std::packaged_task<void()> task; // 缺省构造、默认构造
    std::cout << std::boolalpha << task.valid() << std::endl; // false

    std::packaged_task<void()> task2(std::move(task)); // 右值构造
    std::cout << std::boolalpha << task.valid() << std::endl; // false

    task = std::packaged_task<void()>([](){});  // 右值赋值, 可调用对象
    std::cout << std::boolalpha << task.valid() << std::endl; // true

    return 0;
}
```
```
false
false
true
```
## 5.2 std::packaged_task::operator()(ArgTypes...)
该函数会调用std::packaged_task对象所封装可调用对象R，但其函数原型与R稍有不同:
```c
void operator()(ArgTypes... );
```
operator()的返回值是void，即无返回值。因为std::packaged_task的设计主要是用来进行异步调用，因此R(ArgTypes...)的计算结果是通过std::future::get来获取的。该函数会忠实地将R的计算结果反馈给std::future，即使R抛出异常(此时std::future::get也会抛出同样的异常)。
```c
#include <future>   // std::packaged_task, std::future
#include <iostream> // std::cout

int main() {
    std::packaged_task<void()> convert([](){
        throw std::logic_error("will catch in future");
    });
    std::future<void> future = convert.get_future();

    convert(); // 异常不会在此处抛出

    try {
        future.get();
    } catch(std::logic_error &e) {
        std::cerr << typeid(e).name() << ": " << e.what() << std::endl;
    }

    return 0;
}
/// Clang下输出: St11logic_error: will catch in future
```
为了帮忙大家更好的了解该函数，下面将Clang下精简过的operator()(Args...)的实现贴出，以便于更好理解该函数的边界，明确什么可以做，什么不可以做。
```c
template<class _Rp, class ..._ArgTypes>
class packaged_task<_Rp(_ArgTypes...)> {
    __packaged_task_function<_Rp_(_ArgTypes...)> __f_;
    promise<_Rp> __p_;  // 内部采用了promise实现

public:
    // 构造、析构以及其它函数...

    void packaged_task<_Rp(_ArgTypes...)>::operator()(_ArgTypes... __args) {
        if (__p_.__state_ == nullptr)
            __throw_future_error(future_errc::no_state);
        if (__p_.__state_->__has_value())  // __f_不可重复调用
            __throw_future_error(future_errc::promise_already_satisfied);

        try {
            __p_.set_value(__f_(std::forward<_ArgTypes>(__args)...));
        } catch (...) {
            __p_.set_exception(current_exception());
        }
    }
};
```
## 5.3 让std::packaged_task在线程退出时再将结果反馈给std::future
std::packaged_task::make_ready_at_thread_exit函数接收的参数与operator()(_ArgTypes...)一样，行为也一样。只有一点差别，那就是不会将计算结果立刻反馈给std::future，而是在其执行时所在的线程结束后，std::future::get才会取得结果。
## 5.4  std::packaged_task::reset
与std::promise不一样， std::promise仅可以执行一次set_value或set_exception函数，但std::packagged_task可以执行多次，其奥秘就是reset函数
```c
template<class _Rp, class ..._ArgTypes>
void packaged_task<_Rp(_ArgTypes...)>::reset()
{
    if (!valid())
        __throw_future_error(future_errc::no_state);
    __p_ = promise<result_type>();
}
```
通过重新构造一个promise来达到多次调用的目的。显然调用reset后，需要重新get_future，以便获取下次operator()执行的结果。由于是重新构造了promise，因此reset操作并不会影响之前调用的make_ready_at_thread_exit结果，也即之前的定制的行为在线程退出时仍会发生。

std::packaged_task就介绍到这里，下一篇将会完成本次异步运行的整体脉络，将std::async和std::future一起介绍结大家。

# 六、异步运行之最终篇(future+async)
前面两章多次使用到std::future，本章我们就来揭开std::future庐山真面目。最后我们会引出std::async，该函数使得我们的并发调用变得简单，优雅。
前面我们多次使用std::future的get方法来获取其它线程的结果，那么除这个方法外，std::future还有哪些方法呢。
```c
enum class future_status
{
    ready,
    timeout,
    deferred
};
template <class R>
class future
{
public:
    // retrieving the value
    R get();
    // functions to check state
    bool valid() const noexcept;

    void wait() const;
    template <class Rep, class Period>
    future_status wait_for(const chrono::duration<Rep, Period>& rel_time) const;
    template <class Clock, class Duration>
    future_status wait_until(const chrono::time_point<Clock, Duration>& abs_time) const;

    shared_future<R> share() noexcept;
};
```
以上代码去掉了std::future构造、析构、赋值相关的代码，这些约束我们之前都讲过了。下面我们来逐一了解上面这些函数。
## 6.1 get
这个函数我们之前一直使用，该函数会一直阻塞，直到获取到结果或异步任务抛出异常。

## 6.2 share
std::future允许move，但是不允许拷贝。如果用户确有这种需求，需要同时持有多个实例，怎么办呢? 这就是share发挥作用的时候了。std::shared_future通过引用计数的方式实现了多实例共享同一状态，但有计数就伴随着同步开销(无锁的原子操作也是有开销的)，性能会稍有下降。因此C++11要求程序员显式调用该函数，以表明用户对由此带来的开销负责。std::shared_future允许move，允许拷贝，并且具有和std::future同样的成员函数，此处就不一一介绍了。当调用share后，std::future对象就不再和任何共享状态关联，其valid函数也会变为false。
## 6.3 wait
等待，直到数据就绪。数据就绪时，通过get函数，无等待即可获得数据。
## 6.4 wait_for和wait_until
wait_for、wait_until主要是用来进行超时等待的。wait_for等待指定时长，wait_until则等待到指定的时间点。返回值有3种状态：

* ready - 数据已就绪，可以通过get获取了。
* timeout - 超时，数据还未准备好。
* deferred - 这个和std::async相关，表明无需wait，异步函数将在get时执行。

## 6.5 valid
判断当前std::future实例是否有效。std::future主要是用来获取异步任务结果的，作为消费方出现，单独构建出来的实例没意义，因此其valid为false。当与其它生产方(Provider)通过共享状态关联后，valid才会变得有效，std::future才会发挥实际的作用。C++11中有下面几种Provider，从这些Provider可获得有效的std::future实例：

* std::async
* std::promise::get_future
* std::packaged_task::get_future
既然std::future的各种行为都依赖共享状态，那么什么是共享状态呢?
# 七、共享状态
共享状态其本质就是单生产者-单消费者的多线程并发模型。无论是std::promise还是std::packaged_task都是通过共享状态，实现与std::future通信的。还记得我们在std::condition_variable一节给出的chan类么。共享状态与其类似，通过std::mutex、std::condition_variable实现了多线程间通信。共享状态并非C++11的标准，只是对std::promise、std::future的实现手段。回想我们之前的使用场景，共享状态可能具有如下形式(c++11伪代码):
```c
template<typename T>
class assoc_state {
protected:
    mutable mutex mut_;
    mutable condition_variable cv_;
    unsigned state_ = 0;
    // std::shared_future中拷贝动作会发生引用计数的变化
    // 当引用计数降到0时，实例被delete
    int share_count_ = 0;
    exception_ptr exception_; // 执行异常
    T value_;  // 执行结果

public:
    enum {
        ready = 4,  // 异步动作执行完，数据就绪
        // 异步动作将延迟到future.get()时调用
        // (实际上非异步，只不过是延迟执行)
        deferred = 8,
    };

    assoc_state() {}
    // 禁止拷贝
    assoc_state(const assoc_state &) = delete;
    assoc_state &operator=(const assoc_state &) = delete;
    // 禁止move
    assoc_state(assoc_state &&) = delete;
    assoc_state &operator=(assoc_state &&) = delete;

    void set_value(const T &);
    void set_exception(exception_ptr p);
    // 需要用到线程局变存储
    void set_value_at_thread_exit(const T &);
    void set_exception_at_thread_exit(exception_ptr p);

    void wait();
    future_status wait_for(const duration &) const;
    future_status wait_until(const time_point &) const;

    T &get() {
        unique_lock<mutex> lock(this->mut_);
        // 当_state为deferred时，std::async中
        // 的函数将在sub_wait中调用
        this->sub_wait(lock);
        if (this->_exception != nullptr)
            rethrow_exception(this->_exception);
        return _value;
    }
private:
    void sub_wait(unique_lock<mutex> &lk) {
        if (state_ != ready) {
            if (state_ & static_cast<unsigned>(deferred)) {
                state_ &= ~static_cast<unsigned>(deferred);
                lk.unlock();
                __execute();  // 此处执行实际的函数调用
            } else {
                cv_.wait(lk, [this](){return state == ready;})
            }
        }
    }
};
```
以上给出了get的实现(伪代码)，其它部分虽然没实现，但assoc_state应该具有的功能，以及对std::promise、std::packaged_task、std::future、std::shared_future的支撑应该能够表达清楚了。未实现部分还请读者自行补充一下，权当是练手了。
有兴趣的读者可以阅读[llvm-libxx](https://github.com/llvm-mirror/libcxx)([https://github.com/llvm-mirror/libcxx](https://github.com/llvm-mirror/libcxx))的源码，以了解更多细节，对共享状态有更深掌握。
# 九、std::async
std::async可以看作是对std::packaged_task的封装(虽然实际并一定如此，取决于编译器的实现，但共享状态的思想是不变的)，有两种重载形式:
```c
#define FR typename result_of<typename decay<F>::type(typename decay<Args>::type...)>::type

// 不含执行策略
template <class F, class... Args>
future<FR> async(F&& f, Args&&... args);
// 含执行策略
template <class F, class... Args>
future<FR> async(launch policy, F&& f, Args&&... args);
```
define部分是用来推断函数F的返回值类型，我们先忽略它，以后有机再讲。两个重载形式的差别是一个含执行策略，而另一个不含。那么什么是执行策略呢？执行策略定义了async执行F(函数或可调用求对象)的方式，是一个枚举值：
```c
enum class launch {
    // 保证异步行为，F将在单独的线程中执行
    async = 1,
    // 当其它线程调用std::future::get时，
    // 将调用非异步形式, 即F在get函数内执行
    deferred = 2,
    // F的执行时机由std::async来决定
    any = async | deferred
};
```
不含加载策略的版本，使用的是std::launch::any，也即由std::async函数自行决定F的执行策略。那么C++11如何确定std::any下的具体执行策略呢，一种可能的办法是：优先使用async策略，如果创建线程失败，则使用deferred策略。实际上这也是Clang的any实现方式。std::async的出现大大减轻了异步的工作量。使得一个异步调用可以像执行普通函数一样简单。
```c
#include <iostream> // std::cout, std::endl
#include <future>   // std::async, std::future
#include <chrono>   // seconds
using namespace std::chrono;

int main() {
    auto print = [](char c) {
        for (int i = 0; i < 10; i++) {
            std::cout << c;
            std::cout.flush();
            std::this_thread::sleep_for(milliseconds(1));
        }
    };
    // 不同launch策略的效果
    std::launch policies[] = {std::launch::async, std::launch::deferred};
    const char *names[] = {"async   ", "deferred"};
    for (int i = 0; i < sizeof(policies)/sizeof(policies[0]); i++) {
        std::cout << names[i] << ": ";
        std::cout.flush();
        auto f1 = std::async(policies[i], print, '+');
        auto f2 = std::async(policies[i], print, '-');
        f1.get();
        f2.get();
        std::cout << std::endl;
    }

    return 0;
}
```
```
async   : +-+-+-+--+-++-+--+-+
deferred: ++++++++++----------
```
进行到现在，C++11的async算是结束了，尽管还留了一些疑问，比如共享状态如何实现set_value_at_thread_exit效果。我们将会在下一章节介绍[C++11的线程局部存储](https://www.jianshu.com/p/8df45004bbcb)，顺便也解答下该疑问。

# 十、局部存储

线程局部存储在其它语言中都是以库的形式提供的(库函数或类)。但在C++11中以关键字的形式，做为一种存储类型出现，由此可见C++11对线程局部存储的重视。C++11中有如下几种存储类型:
|序号|	类型|	备注|
|:---:|:---:|:---:|
|1|	auto	|该关键字用于两种情况：1. 声明变量时： 根据初始化表达式自动推断变量类型。2. 声明函数作为函数返回值的占位符。|
|2|	static|	static变量只初始化一次，除此之外它还有可见性的属性：1. static修饰函数内的“局部”变量时，表明它不需要在进入或离开函数时创建或销毁。且仅在函数内可见。2. static修饰全局变量时，表明该变量仅在当前(声明它的)文件内可见。3. static修饰类的成员变量时，则该变量被该类的所有实例共享.|
|3|	register|	寄存器变量。该变量存储在CPU寄存器中，而不是RAM(栈或堆)中。该变量的最大尺寸等于寄存器的大小。由于是存储于寄存器中，因此不能对该变量进行取地址操作。|
|4|	extern|	引用一个全局变量。当在一个文件中定义了一个全局变量时，就可以在其它文件中使用extern来声明并引用该变量。|
|5|	mutable	|仅适用于类成员变量。以mutable修饰的成员变量可以在const成员函数中修改。参见上一章chan.simple.h中对mutex的使用。|
|6|	thread_local|	线程周期|

thread_local修饰的变量具有如下特性:

* 变量在线程创建时生成(不同编译器实现略有差异，但在线程内变量第一次使用前必然已构造完毕)。
* 线程结束时被销毁(析构，利用析构特性，thread_local变量可以感知线程销毁事件)。
* 每个线程都拥有其自己的变量副本。
* thread_local可以和static或extern联合使用，这将会影响变量的链接属性。
下面代码演示了thread_local变量在线程中的生命周期
```c
// thread_local.cpp
#include <iostream>
#include <thread>

class A {
public:
  A() {
    std::cout << std::this_thread::get_id()
              << " " << __FUNCTION__
              << "(" << (void *)this << ")"
              << std::endl;
  }

  ~A() {
    std::cout << std::this_thread::get_id()
              << " " << __FUNCTION__
              << "(" << (void *)this << ")"
              << std::endl;
  }

  // 线程中，第一次使用前初始化
  void doSth() {
  }
};

thread_local A a;

int main() {
  a.doSth();
  std::thread t([]() {
    std::cout << "Thread: "
              << std::this_thread::get_id()
              << " entered" << std::endl;
    a.doSth();
  });

  t.join();

  return 0;
}
```
```
01 A(0xc00720)
Thread: 02 entered
02 A(0xc02ee0)
02 ~A(0xc02ee0)
01 ~A(0xc00720)
```
# 十一、原子操作
前面我们讲了C++11下的多线程及相关操作，这些操作在绝大多数情况下应该够用了。但在某些极端场合，如需要高性能的情况下，我们还需要一些更高效的同步手段。本节介绍的原子操作是一种lock free的操作，不需要同步锁，具有很高的性能。在化学中原子不是可分割的最小单位，引申到编程中，原子操作是不可打断的最低粒度操作，是线程安全的。C++11中原子类提供的成员函数都是原子的，是线程安全的。
原子操作中最简单的莫过于atomic_flag，只有两种操作：test and set、clear。我们的原子操作就从这种类型开始。
## 11.1 std::atomic_flag
C++11中所有的原子类都是不允许拷贝、不允许Move的，atomic_flag也不例外。atomic_flag顾名思议，提供了标志的管理，标志有三种状态：clear、set和未初始化状态。
### 11.1.1  atomic_flag实例化
缺省情况下atomic_flag处于未初始化状态。除非初始化时使用了ATOMIC_FLAG_INIT宏，则此时atomic_flag处于clear状态。
### 11.1.2 std::atomic_flag::clear
调用该函数将会把atomic_flag置为clear状态。clear状态您可以理解为bool类型的false，set状态可理解为true状态。clear函数没有任何返回值:
```c
void clear(memory_order m = memory_order_seq_cst) volatile noexcept;
void clear(memory_order m = memory_order_seq_cst) noexcept;
```
对于memory_order我们会在后面的章节中详细介绍它，现在先列出其取值及简单释义

|序号|	值	|意义|
|:---:|:---:|:---:|
|1|	memory_order_relaxed|	宽松模型，不对执行顺序做保证|
|2|	memory_order_consume|	当前线程中,满足happens-before原则。当前线程中该原子的所有后续操作,必须在本条操作完成之后执行|
|3|	memory_order_acquire|	当前线程中,读操作满足happens-before原则。所有后续的读操作必须在本操作完成后执行|
|4|	memory_order_release|	当前线程中,写操作满足happens-before原则。所有后续的写操作必须在本操作完成后执行|
|5|	memory_order_acq_rel|	当前线程中，同时满足memory_order_acquire和memory_order_release|
|6|	memory_order_seq_cst	|最强约束。全部读写都按顺序执行|
### 11.1.3 test_and_set
该函数会检测flag是否处于set状态，如果不是，则将其设置为set状态，并返回false；否则返回true。
[test_and_set](https://en.wikipedia.org/wiki/Test-and-set)是典型的*read-modify-write(RMW)*模型，保证多线程环境下只被设置一次。下面代码通过10个线程，模拟了一个计数程序，第一个完成计数的会打印"win"。
```c
#include <atomic>    // atomic_flag
#include <iostream>  // std::cout, std::endl
#include <list>      // std::list
#include <thread>    // std::thread

void race(std::atomic_flag &af, int id, int n) {
    for (int i = 0; i < n; i++) {
    }
    // 第一个完成计数的打印：Win
    if (!af.test_and_set()) {
        printf("%s[%d] win!!!\n", __FUNCTION__, id);
    }
}

int main() {
    std::atomic_flag af = ATOMIC_FLAG_INIT;

    std::list<std::thread> lstThread;
    for (int i = 0; i < 10; i++) {
        lstThread.emplace_back(race, std::ref(af), i + 1, 5000 * 10000);
    }

    for (std::thread &thr : lstThread) {
        thr.join();
    }

    return 0;
}
```
```
race[7] win!!!

```
## 11.2 std::atomic<T>
std::atomic是一个模板类，它定义了一些atomic应该具有的通用操作，我们一起来看一下:
### 11.2.1 is_lock_free
```c
bool is_lock_free() const noexcept;
bool is_lock_free() const volatile noexcept;
```
atomic是否无锁操作。如果是，则在多个线程访问该对象时不会导致线程阻塞(可能使用某种事务内存transactional memory方法实现lock-free的特性)。
事实上该函数可以做为一个静态函数。所有指定相同类型T的atomic实例的is_lock_free函数都会返回相同值。
### 11.2.2 store
```c
void store(T desr, memory_order m = memory_order_seq_cst) noexcept;
void store(T desr, memory_order m = memory_order_seq_cst) volatile noexcept;
T operator=(T d) noexcept;
T operator=(T d) volatile noexcept;
```
赋值操作。operator=实际上内部调用了store，并返回d。

```c
T operator=(T d) volatile noexpect {
    store(d);
    return d;
}
```
注：有些编译器，在实现store时限定m只能取以下三个值：memory_order_consume，memory_order_acquire，memory_order_acq_rel。
### 11.2.3 load
```c
T load(memory_order m = memory_order_seq_cst) const volatile noexcept;
T load(memory_order m = memory_order_seq_cst) const noexcept;
operator T() const volatile noexcept;
operator T() const noexcept;
```
读取，加载并返回变量的值。operator T是load的简化版，内部调用的是load(memory_order_seq_cst)形式。
### 11.2.4 exchange
```c
T exchange(T desr, memory_order m = memory_order_seq_cst) volatile noexcept;
T exchange(T desr, memory_order m = memory_order_seq_cst) noexcept;
```
交换，赋值后返回变量赋值前的值。exchange也称为read-modify-write操作。

### 11.2.5 compare_exchange_weak
```c
bool compare_exchange_weak(T& expect, T desr, memory_order s, memory_order f) volatile noexcept;
bool compare_exchange_weak(T& expect, T desr, memory_order s, memory_order f) noexcept;
bool compare_exchange_weak(T& expect, T desr, memory_order m = memory_order_seq_cst) volatile noexcept;
bool compare_exchange_weak(T& expect, T desr, memory_order m = memory_order_seq_cst) noexcept;
```
该函数直接比较原子对象所封装的值与expect的物理内容，在某些情况下，对象的比较操作在使用 operator==() 判断时相等，但 compare_exchange_weak 判断时却可能失败，因为对象底层的物理内容中可能存在位对齐或其他逻辑表示相同但是物理表示不同的值(比如 true 和 5，它们在逻辑上都表示"真"，但在物理上两者的表示并不相同)。
与strong版本不同，weak版允许返回伪false，即使原子对象所封装的值与expect的物理内容相同，也仍然返回false。但它在某些平台下会取得更好的性能，在某些循环算法中这种行为也是可接受的。对于非循环算法建议使用compare_exchange_strong。
### 11.2.6 compare_exchange_strong
```c
bool compare_exchange_strong(T& expect, T desr, memory_order s, memory_order f) volatile noexcept;
bool compare_exchange_strong(T& expect, T desr, memory_order s, memory_order f) noexcept;
bool compare_exchange_strong(T& expect, T desr, memory_order m = memory_order_seq_cst) volatile noexcept;
bool compare_exchange_strong(T& expc, T desr, memory_order m = memory_order_seq_cst) noexcept;
```
compare_exchange的strong版本，进行compare时，与weak版一样，都是比较的物理内容。与weak版不同的是，strong版本不会返回伪false。即：原子对象所封装的值如果与expect在物理内容上相同，strong版本一定会返回true。其所付出的代价是：在某些需要循环检测的算法，或某些平台下，其性能较compare_exchange_weak要差。但对于某些不需要采用循环检测的算法而言, 通常采用compare_exchange_strong 更好。

## 11.3 std::atomic特化
我知道计算擅长处理整数以及指针，并且X86架构的CPU还提供了指令级的CAS操作。C++11为了充分发挥计算的特长，针对非浮点数值(std::atmoic<integral>)及指针(std::atomic<T*>)进行了特化，以提高原子操作的性能。特化后的atomic在通用操作的基础上，还提供了更丰富的功能。
### 11.3.1 fetch_add
```c
// T is integral
T fetch_add(T v, memory_order m = memory_order_seq_cst) volatile noexcept;
T fetch_add(T v, memory_order m = memory_order_seq_cst) noexcept;
// T is pointer
T fetch_add(ptrdiff_t v, memory_order m = memory_order_seq_cst) volatile noexcept;
T fetch_add(ptrdiff_t v, memory_order m = memory_order_seq_cst) noexcept;
```
该函数将原子对象封装的值加上v，同时返回原子对象的旧值。其功能用伪代码表示为：

```c
auto old = contained
contained += v
return old
```
其中contained为原子对象封装值，本文后面均使用contained代表该值。注： 以上是为了便于理解的伪代码，实际实现是原子的不可拆分的。

### 11.3.2 fetch_sub
```c
// T is integral
T fetch_sub(T v, memory_order m = memory_order_seq_cst) volatile noexcept;
T fetch_sub(T v, memory_order m = memory_order_seq_cst) noexcept;
// T is pointer
T fetch_sub(ptrdiff_t v, memory_order m = memory_order_seq_cst) volatile noexcept;
T fetch_sub(ptrdiff_t v, memory_order m = memory_order_seq_cst) noexcept;
```
该函数将原子对象封装的值减去v，同时返回原子对象的旧值。其功能用伪代码表示为：
```c
auto old = contained
contained -= v
return old
```
### 11.3.3 ++, --, +=, -=
不管是基于整数的特化，还是指针特化，atomic均支持这四种操作。其用法与未封装时一样，此处就不一一列举其函数原型了。

## 11.4 独属于数值型特化的原子操作 - 位操作
### 11.4.1 fetch_and，fetch_or，fetch_xor
位操作，将contained按指定方式进行位操作，并返回contained的旧值。
```c
integral fetch_and(integral v, memory_order m = memory_order_seq_cst) volatile noexcept;
integral fetch_and(integral v, memory_order m = memory_order_seq_cst) noexcept;
integral fetch_or(integral v, memory_order m = memory_order_seq_cst) volatile noexcept;
integral fetch_or(integral v, memory_order m = memory_order_seq_cst) noexcept;
integral fetch_xor(integral v, memory_order m = memory_order_seq_cst) volatile noexcept;
integral fetch_xor(integral v, memory_order m = memory_order_seq_cst) noexcept;
```
以xor为例，其操作相当于
```c
auto old = contained
contained ^= v
return old
```
## 11.4.2 operator &=，operator |=，operator ^=
与相应的fetch_*操作不同的是，operator操作返回的是新值:
```c
T operator &=(T v) volatile noexcept {return fetch_and(v) & v;}
T operator &=(T v) noexcept {return fetch_and(v) & v;}
T operator |=(T v) volatile noexcept {return fetch_or(v) | v;}
T operator |=(T v) noexcept {return fetch_or(v) | v;}
T operator ^=(T v) volatile noexcept {return fetch_xor(v) ^ v;}
T operator ^=(T v) noexcept {return fetch_xor(v) ^ v;}
```
## 11.5  std::atomic的限制：trivially copyable
上面我们提到std::atomic提供了通用操作，其实这些操作可以应用到所有trivially copyable的类型。trivially copyable在cppreference中文站被译为“可平凡复制”。网上也有人译作拷贝不变。一个类型如果是trivially copyable，则使用memcpy这种方式把它的数据从一个地方拷贝出来会得到相同的结果。因此本文使用拷贝不变这个中文翻译，请大家不要纠结中文翻译，明白本文所表达的意思即可。编译器如何判断一个类型是否trivially copyable呢？C++标准把trivial类型定义如下，一个拷贝不变(trivially copyable)类型是指：

* 没有non-trivial 的拷贝构造函数
* 没有non-trivial的move构造函数
* 没有non-trivial的赋值操作符
* 没有non-trivial的move赋值操作符
有一个trivial的析构函数
一个trivial class类型是指有一个trivial类型的默认构造函数，而且是拷贝不变的(trivially copyable)的class。特别注意，拷贝不变类型和trivial类型都不能有虚机制。那么trivial和non-trivial类型到底是什么呢？这里给出一个非官方、不严谨的判断方式，方便大家对trivially copyable有一个直观的认识。一个trivial copyable类在四个点上没有自定义动作，也没有编译器加入的额外动作(如虚指针初始化就属额外动作)，这四个点是:

* 缺省构造。类必须支持缺省构造，同时类的非静态成员也不能有自定义或编译器加入的额外动作，否则编译器势必会隐式插入额外动作来初始化非静态成员。
* 拷贝构造、拷贝赋值
* move构造、move赋值
* 析构
为了加深理解，我们来看一下下面的例子(所有的类都是trivial的)：
```c
// 空类
struct A1 {};

// 成员变量是trivial的
struct A2 {
    int x;
};

// 基类是trivial的
struct A3 : A2 {
    // 非用户自定义的构造函数(使用编译器提供的default构造)
    A3() = default;
    int y;
};

struct A4 {
    int a;
private: // 对防问限定符没有要求，A4仍然是trivial的
    int b;
};

struct A5 {
    A1 a;
    A2 b;
    A3 c;
    A4 d;
};

struct A6 {
    A2 a[16];
};

struct A7 {
    A6 c;
    void f(); // 普通成员函数是允许的
};

struct A8 {
     int x;
    // 对静态成员无要求(std::string是non-trivial的)
     static std::string y;
};

struct A9 {
    // 非用户自定义
    A9() = default;
    // 普通构造函数是可以的(前提是我们已经有了非定义的缺省构造函数)
    A9(int x) : x(x) {};
    int x;
};
```
而下面这些类型都是non-trivial的

```c
struct B {
    // 有虚函数(编译器会隐式生成缺省构造，同时会初始化虚函数指针)
    virtual f();
};

struct B2 {
    // 用户自定义缺省构造函数
    B2() : z(42) {}
    int z;
};

struct B3 {
    B3();
    int w;
};
// 虽然使用了default，但在缺省构造声明处未指定，因此被判断为non-trivial的
NonTrivial3::NonTrivial3() = default;

struct B4 {
   // 虚析构是non-trivial的
    virtual ~B4();
};
```
struct B {
    // 有虚函数(编译器会隐式生成缺省构造，同时会初始化虚函数指针)
    virtual f();
};

struct B2 {
    // 用户自定义缺省构造函数
    B2() : z(42) {}
    int z;
};

STL在其头文件<type_traits>中定义了对trivially copyable类型的检测：

```c
template <typename T>
struct std::is_trivially_copyable;
```
判断类A是否trivially copyable:std::is_trivially_copyable<A>::value，该值是一个const bool类型，如果为true则是trivially copyable的，否则不是。
参考链接：
https://blog.csdn.net/coolwriter/article/details/79883253
[https://www.jianshu.com/p/dcce068ee32b](https://www.jianshu.com/p/dcce068ee32b)
