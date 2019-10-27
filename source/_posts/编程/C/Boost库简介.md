---
title: Boost库简介
author: 陈德强
date: 2019-09-28 11:17:00
categories:
- 字典
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: Boost库简介。
---

# 一、简介

boost是一个准标准库，相当于STL的延续和扩充，它的设计理念和STL比较接近，都是利用泛型让复用达到最大化。不过对比STL，boost更加实用。STL集中在算法部分，而boost包含了不少工具类，可以完成比较具体的工作。

boost主要包含一下几个大类：字符串及文本处理、容器、迭代子(Iterator)、算法、函数对象和高阶编程、泛型编程、模板元编程、预处理元编程、并发编程、数学相关、纠错和测试、数据结构、输入/输出、跨语言支持、内存相关、语法分析、杂项。 有一些库是跨类别包含的，就是既属于这个类别又属于那个类别。

在文本处理部分，conversion/lexcial_cast类用于“用C++”的方法实现数字类型和字符串之间的转换。 主要是替代C标准库中的 atoi、 itoa之类的函数。当然其中一个最大的好处就是支持泛型了。

format库提供了对流的“printf-like”功能。printf里使用%d、%s等等的参数做替换的方法在很多情况下还是非常方便的，STL的iostream则缺乏这样的功能。format为stream增加了这个功能，并且功能比原始的printf更强。

regex，这个不多说了，正则表达式库。如果需要做字符串分析的人就会理解正则表达式有多么有用了。

spirit，这个是做LL分析的框架，可以根据EBNF规则对文件进行分析。（不要告诉我不知道什么是EBNF）。做编译器的可能会用到。一般人不太用的到。

tokenizer库。我以前经常在CSDN上看到有人问怎么把一个字符串按逗号分割成字符串数组。也许有些人很羡慕VB的split函数。现在，boost的tokenizer也有相同的功能了，如果我没记错的话，这个tokenizer还支持正则表达式，是不是很爽？

array: 提供了常量大小的数组的一个包装，喜欢用数组但是苦恼数组定位、确定数组大小等功能的人这下开心了。

dynamic_bitset，动态分配大小的bitset，我们知道STL里有个bitset，为位运算提供了不少方便。可惜它的大小需要在编译期指定。现在好了，运行期动态分配大小的bitset来了。

graph。提供了图的容器和相关算法。我还没有在程序中用到过图，需要用的人可以看看。

multi_array提供了对多维数组的封装，应该还是比较有用的。

并发编程里只有一个库，thread，提供了一个可移植的线程库，不过在Windows平台上我感觉用处不大。因为它是基于Posix线程的，在Windows里对Posix的支持不是很好。



接下来的 数学和数值 类里，包含了很多数值处理方面的类库，数学类我也不太熟，不过这里有几个类还是很有用的，比如rational分数类，random随机数类，等等。

static_assert，提供了编译器的assert功能。

test库，一个单元测试框架，非常不错。

concept_check提供了泛型编程时，对泛型量的一点检查，不是很完善，不过比没有好。

数据类型类any，一个安全的可以包含不同对象的类。把它作为容器的元素类型，那么这个容器就可以包含不同类型的元素。比用void *要安全。

compressed_pair，跟STL里的pair差不多。不过对空元素做了优化。

tuple，呵呵，也许是某些人梦寐以求的东西。可以让函数返回多个值。

跨语言支持：Python，呵呵，好东东啊，可以将C++的类和函数映射给python使用。以下为几个CSDN上的关于boost.python的中文资料：http://dev.csdn.net/article/19/19828.shtm，http://dev.csdn.net/article/19/19829.shtm，http://dev.csdn.net/article/19/19830.shtm，http://dev.csdn.net/article/19/19831.shtm

pool:内存池，呵呵，不用害怕频繁分配释放内存导致内存碎片，也不用自己辛辛苦苦自己实现了。

smart_ptr:智能指针，这下不用担心内存泄漏的问题了吧。不过，C++里的智能指针都还不是十全十美的，用的时候小心点了，不要做太技巧性的操作了。

date_time，这个是平台、类库无关的实现，如果程序需要跨平台，可以考虑用这个。

timer，提供了一个计时器，虽然不是Windows里那种基于消息的计时器，不过据说可以用来测量语句执行时间。

uitlity里提供了一个noncopyable类，可以实现“无法复制”的类。很多情况下，我们需要避免一个类被复制，比如代表文件句柄的类，文件句柄如果被两个实例共享，操作上会有很多问题，而且语义上也说不过去。一般的避免实例复制的方法是把拷贝构造和operator=私有化，现在只要继承一下这个类就可以了，清晰了很多。

value_initialized：数值初始化，可以保证声明的对象都被明确的初始化，不过这个真的实用吗？似乎写这个比直接写初始化还累。呵呵，仁者见仁了。

这里面除了regex、python和test需要编译出库才能用，其他的大部分都可以直接源代码应用，比较方便。其实这些库使用都不难。最主要的原因是有些库的使用需要有相关的背景知识，比如元编程、STL、泛型编程等等。

--------------

# 二、百度百科

按照实现的功能，Boost可为大致归入以下20个分类，在下面的分类中，有些库同时归入几种类别。
## 2.1 字符串和文本处理库
a) Conversion库：对C++类型转换的增强，提供更强的类型安全转换、更高效的类型安全保护、进行范围检查的数值转换和词法转换。
b) Format库：实现类似printf的格式化对象，可以把参数格式化到一个字符串，而且是完全类型安全的。
c) IOStream库 ：扩展C++标准库流处理，建立一个流处理框架。
d) Lexical Cast库：用于字符串、整数、浮点数的字面转换。
e) Regex 库：正则表达式，已经被TR1所接受。
f) Spirit库：基于EBNF范式的LL解析器框架
g) String Algo库：一组与字符串相关的算法
h) Tokenizer库：把字符串拆成一组记号的方法
i) Wave库：使用spirit库开发的一个完全符合C/C++标准的预处理器
j) Xpressive 库：无需编译即可使用的正则表达式库
## 2.2 容器库
a) Array 库：对C语言风格的数组进行包装
b) Bimap 库：双向映射结构库
c) Circular Buffer 库：实现循环缓冲区的数据结构
d) Disjoint Sets库 ：实现不相交集的库
e) Dynamic Bitset 库：支持运行时调整容器大小的位集合
f) GIL 库：通用图像库
g) Graph 库：处理图结构的库
h) ICL 库：区间容器库，处理区间集合和映射
i) Intrusive 库：侵入式容器和算法
j) Multi-Array 库：多维容器
k) Multi-Index 库：实现具有多个STL兼容索引的容器
l) Pointer Container 库：容纳指针的容器
m) Property Map 库：提供键/值映射的属性概念定义
n) Property Tree 库：保存了多个属性值的树形数据结构
o) Unordered 库：散列容器，相当于hash_xxx
p) Variant 库：简单地说，就是持有string, vector等复杂类型的联合体
## 2.3 迭代器库
a) GIL 库：通用图像库
b) Graph 库：处理图结构的库
c) Iterators 库：为创建新的迭代器提供框架
d) Operators 库：允许用户在自己的类里仅定义少量的操作符，就可方便地自动生成其他操作符重载，而且保证正确的语义实现
e) Tokenizer 库：把字符串拆成一组记号的方法
## 2.4 算法库
a) Foreach库：容器遍历算法
b) GIL库：通用图像库
c) Graph库：处理图结构的库
d) Min-Max库：可在同一次操作中同时得到最大值和最小值
e) Range库：一组关于范围的概念和实用程序
f) String Algo库：可在不使用正则表达式的情况下处理大多数字符串相关算法操作
g) Utility库：小工具的集合
## 2.5 函数对象和高阶编程库
a) Bind库：绑定器的泛化，已被收入TR1
b) Function库：实现一个通用的回调机制，已被收入TR1
c) Functional库：适配器的增强版本
d) Functional/Factory库：用于实现静态和动态的工厂模式
e) Functional/Forward库：用于接受任何类型的参数
f) Functional/Hash库：实现了TR1中的散列函数
g) Lambda库：Lambda表达式，即未命名函数
h) Member Function库：是STL中mem_fun和mem_fun_ref的扩展
i) Ref库：包装了对一个对象的引用，已被收入TR1
j) Result Of库：用于确定一个调用表达式的返回类型，已被收入TR1
k) Signals库：实现线程安全的观察者模式
l) Signals2库：基于Signal的另一种实现
m) Utility库：小工具的集合
n) Phoenix库：实现在C++中的函数式编程。
## 2.6 泛型编程库
a) Call Traits库：封装可能是最好的函数传参方式
b) Concept Check库：用来检查是否符合某个概念
c) Enable If库:允许模板函数或模板类在偏特化时仅针对某些特定类型有效
d) Function Types库：提供对函数、函数指针、函数引用和成员指针等类型进行分类分解和合成的功能
e) GIL库：通用图像库
f) In Place Factory, Typed In Place Factory库：工厂模式的一种实现
g) Operators库：允许用户在自己的类里仅定义少量的操作符，就可方便地自动生成其他操作符重载，而且保证正确的语义实现
h) Property Map库：提供键值映射的属性概念定义
i) Static Assert库：把断言的诊断时刻由运行期提前到编译期，让编译器检查可能发生的错误
j) Type Traits库：在编译时确定类型是否具有某些特征
k) TTI库：实现类型萃取的反射功能。
## 2.7 模板元编程
a) Fusion库：提供基于tuple的编译期容器和算法
b) MPL库：模板元编程框架
c) Proto库：构建专用领域嵌入式语言
d) Static Assert库：把断言的诊断时刻由运行期提前到编译期，让编译器检查可能发生的错误
e) Type Traits库：在编译时确定类型是否具有某些特征
## 2.8 预处理元编程库
a) Preprocessors库：提供预处理元编程工具
## 2.9 并发编程库
a) Asio库：基于操作系统提供的异步机制，采用前摄设计模式实现了可移植的异步IO操作
b) Interprocess库：实现了可移植的进程间通信功能，包括共享内存、内存映射文件、信号量、文件锁、消息队列等
c) MPI库：用于高性能的分布式并行开发
d) Thread库：为C++增加线程处理能力，支持Windows和POSIX线程
e) Context库：提供了在单个线程上的协同式多任务处理的支持。该库可以用于实现用户级的多任务处理的机制，比如说协程coroutines，用户级协作线程或者类似于C#语言中yield关键字的实现。 [1] 
f) Atomic库：实现C++11样式的atomic<>，提供原子数据类型的支持和对这些原子类型的原子操作的支持。
g)Coroutine库：实现对协程的支持。协程与线程的不同之处在于，协程是基于合作式多任务的，而多线程是基于抢先式多任务的。
h)Lockfree库：提供对无锁数据结构的支持。
## 2.10 数学和数字库
a) Accumulators库：用于增量计算的累加器的框架
b) Integer库：提供一组有关整数处理的类
c) Interval库：处理区间概念的数学问题
d) Math库：数学领域的模板类和算法
e) Math Common Factor库：用于支持最大公约数和最小公倍数
f) Math Octonion库 ：用于支持八元数
g) Math Quaternion库：用于支持四元数
h) Math/Special Functions库：数学上一些常用的函数
i) Math/Statistical Distributions库：用于单变量统计分布操作
j) Multi-Array库：多维容器
k) Numeric Conversion库：用于安全数字转换的一组函数
l) Operators库：允许用户在自己的类里仅定义少量的操作符，就可方便地自动生成其他操作符重载，而且保证正确的语义实现
m) Random库：专注于伪随机数的实现，有多种算法可以产生高质量的伪随机数
n) Rational库：实现了没有精度损失的有理数
o) uBLAS库：用于线性代数领域的数学库
p) Geometry库：用于解决几何问题的概念、原语和算法。
q) Ratio库：根据C++ 0x标准N2661号建议 [2]  ，实现编译期的分数操作。
r)Multiprecision库：提供比C++内置的整数、分数和浮点数精度更高的多精度数值运算功能。 [3] 
s)Odeint库：用于求解常微分方程的初值问题。 [4] 
## 2.11 排错和测试库
a) Concept Check库 ：用来检查是否符合某个概念
b) Static Assert库 ：把断言的诊断时刻由运行期提前到编译期，让编译器检查可能发生的错误
c) Test库：提供了一个用于单元测试的基于命令行界面的测试套件
## 2.12 数据结构库
a) Any库：支持对任意类型的值进行类型安全的存取
b) Bimap库：双向映射结构库
c) Compressed Pair库：优化的对pair对象的存储
d) Fusion库：提供基于tuple的编译期容器和算法
e) ICL库：区间容器库，处理区间集合和映射
f) Multi-Index库：为底层的容器提供多个索引
g) Pointer Container库：容纳指针的容器
h) Property Tree库：保存了多个属性值的树形数据结构
i) Tuple库：元组，已被TR1接受
j) Uuid库：用于表示和生成UUID
k) Variant库：有类别的泛型联合类
l) Heap库：对std::priority_queue扩展，实现优先级队列。
m) Type Erasure: 实现运行时的多态。
## 2.13 图像处理库
a) GIL库：通用图像库
## 2.14 输入输出库
a) Assign库：用简洁的语法实现对STL容器赋值或者初始化
b) Format库：实现类似printf的格式化对象，可以把参数格式化到一个字符串，而且是完全类型安全的
c) IO State Savers库：用来保存流的当前状态，自动恢复流的状态等
d) IOStreams库：扩展C++标准库流处理，建立一个流处理框架
e) Program Options库：提供强大的命令行参数处理功能
f) Serialization库：实现C++数据结构的持久化
## 2.15 跨语言混合编程库
a) Python库：用于实现Python和C++对象的无缝接口和混合编程
## 2.16 内存管理库
a) Pool库：基于简单分隔存储思想实现了一个快速、紧凑的内存池库
b) Smart Ptr库：智能指针
c) Utility库：小工具的集合
## 2.17 解析库
a) Spirit库：基于EBNF范式的LL解析器框架
## 2.18 编程接口库
a) Function库：实现一个通用的回调机制，已被收入TR1
b) Parameter库：提供使用参数名来指定函数参数的机制
## 2.19 综合类库
a) Compressed Pair库：优化的对pair对象的存储
b) CRC库：实现了循环冗余校验码功能
c) Date Time 库：一个非常全面灵活的日期时间库
d) Exception库：针对标准库中异常类的缺陷进行强化，提供<<操作符重载，可以向异常传入任意数据
e) Filesystem库：可移植的文件系统操作库，可以跨平台操作目录、文件，已被TR2接受
f) Flyweight 库：实现享元模式，享元对象不可修改，只能赋值
g) Lexical Cast 库：用于字符串、整数、浮点数的字面转换
h) Meta State Machine库：用于表示UML2有限状态机的库
i) Numeric Conversion 库：用于安全数字转换的一组函数
j) Optional 库：使用容器的语义，包装了可能产生无效值的对象，实现了未初始化的概念
k) Polygon 库：处理平面多边形的一些算法
l) Program Options库：提供强大的命令行参数处理功能
m) Scope Exit库：使用preprocessor库的预处理技术实现在退出作用域时资源自动释放
n) Statechart库：提供有限自动状态机框架
o) Swap库：为交换两个变量的值提供便捷方法
p) System库：使用轻量级的对象封装操作系统底层的错误代码和错误信息，已被TR2接受
q) Timer库：提供简易的度量时间和进度显示功能，可以用于性能测试等需要计时的任务
r) Tribool库：三态布尔逻辑值，在true和false之外引入indeterminate不确定状态
s) Typeof库：模拟C++0x新增加的typeof和auto关键字，以减轻变量类型声明的工作，简化代码
t) Units库：实现了物理学的量纲处理
u) Utility库：小工具集合
v) Value Initialized库：用于保证变量在声明时被正确初始化
w) Chrono库：实现了C++ 0x标准中N2661号建议 [2]  所支持的时间功能。
x) Log库：实现日志功能。
y) Predef库：提供一批统一兼容探测其他宏的预定义宏。 [5] 
## 2.20 编译器问题的变通方案库
a) Compatibility库：为不符合标准库要求的环境提供帮助
b) Config库：将程序的编译配置分解为三个部分：平台、编译器和标准库，帮助库开发者解决特定平台特定编译器的兼容问题

------------

# 三、维基百科
>* 算法
>* 并行计算
thread - 线程
context - 用户层级上下文交换
>* 容器
array - STL的数组容器
Boost Graph Library (BGL) - 通用的图容器，组件和算法
multi-array - N维数组
multi-index containers - 多索引容器
pointer containers - 指针容器
property map - 属性Map
variant - 安全的，基于泛型的，支持访问者模式的联合
fusion - 基于tuple的容器和算法集合
>* 正当性与测试
concept check - 检查模板参数是否满足模板的要求
static assert - 编译期的断言检查
Boost Test Library - C++ 单元测试框架
>* 数据结构
dynamic_bitset - std::bitset-的动态转型
>* 仿函数与高端函数（含无名関数）
bind and mem_fn - 函数的绑定
function - 函数。
functional - C++标准函数之强化。包含以下的内容。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  function object traits
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	negators
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	binders
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	adapters for pointers to functions
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	adapters for pointers to member functions
hash - C++ Technical Report 1（TR1）定义的散列表
lambda - λ演算的实现
ref - 标准C++参照（call by reference）的加强、特别强化与函数的调用
result_of - 函数类型与回传值
signals2 - 信号和槽回调的实现托管
>* 泛型
>* 图
>* I/O
>* 语言之间的支持（Python用）
>* 迭代器
iterators
operators
tokenizer
>* 数学和计算
>* 内存（memory）
pool - 内存池，boost提供4种内存池模型供使用：pool、object_pool、singleton_pool、pool_allocator/fast_pool_allocator
smart_ptr - boost的smart_ptr中提供了4种智能指针，作为std::auto_ptr的补充
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	scoped_ptr - 与auto_ptr类似，但不能转让所有权，用于确保离开作用域能够正确地删除动态分配的对象
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	scoped_array - 配合scoped_ptr使用
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	shared_ptr -
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	shared_array - 配合shared_ptr使用
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	weak_ptr - shared_ptr 的观察者，避免shared_ptr循环引用，是一种辅助指针
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	intrusive_ptr - 比 shared_ptr 更好的智能指针
utility - 以下是utility类型的定义。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	base from member idiom -
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	checked delete - 保证在摧毁一个对象时，必须对该对象的类型有充份了解
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	next and prior functions -
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	noncopyable - 把copy constructor和assign operaotr 宣告为private，不加以实现
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	addressof - 用于获得变量的地址
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;	result_of - 指涉函数回返类型
>* 其他
>* 语法分析器
>* 预处理元编程
>* 字符串与文字处理（正则表达式等）
lexical_cast - lexical_cast 类别模板
format - 文字格式化，类似printf
iostreams - 新式iostream的补强
regex - 正规表示法（Regular expression）
Spirit - 根据EBNF规则对文件进行分析
string algorithms - 文字列算法
tokenizer - 把字符串序列分解成一系列标记（tokens）
wave -
>* 模板元编程（Template Metaprogramming）
mpl - 模板元编程框架
static assert - 静态断言
type traits - 类型的基本属性的模板


参考链接：
https://www.cnblogs.com/findumars/p/7257415.html
https://wikipedia.sogou.se/wiki/Boost_C%2B%2B_Libraries