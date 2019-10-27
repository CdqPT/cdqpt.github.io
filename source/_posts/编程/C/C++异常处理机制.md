
---
title: C++异常处理机制
author: 陈德强
date: 2019-09-07 20:43:00
categories:
- 笔记
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: C++异常处理机制。
---

# 一、默认异常处理机制about()
默认情况下，当程序出现异常，将调用about()异常处理函数，直接终止程序。

# 二、exit()
exit()将刷新缓存区，但不显示消息。

# 三、try-catch-throw
try{}：当代码块内出现异常，并不会终止程序，而是将程序跳转到catch代码块，
catch(){}：正常情况下，catch代码块不会执行，只有当程序异常时才会执行，
throw(){}：此时如果异常程序内使用了throw来处理,则throw会将信息作为参数传递给catch，catch处理信息后继续执行程序。
通常情况下，catch通过传递类来处理异常。

```c

//.h
class bad
{
    private:
        double v1;
        double v2;
    public:
        bad(int a=0,int b=0):v1(0),v2(0){}
        void msg(){std::cout<<"hmean("<<v1<<","<<v2<<"):"<<"invaliud argument:a=-b;\n"}
};

//.c

int hmean(int a,int b);

int main()
{

    try
    {
        c=hmean(a,b);
    }
    catch(bad& bg)
    {
        bg.msg();
        std::cout<<"Error!"
        continue;
    }
    std::cout<<"Bye!";
    return 0;
}

int hnean(int a,int b)
{
    return a/b;
}
```

# 四、exception类
exception类包含在<exception.h>头文件中，
exception类是一种异常类的基类，派生出两种类：logic_error和runtime_error,
*logic_error和runtime_error*
logic_error描述了典型的逻辑错误（可修复），runtime_error描述了运行时错误（不可避免）。
*logic_error*
· domain_error,定义域错误，参数超出取值范围
· invalid_error,无效参数，参数类型错误
· length_error,没有足够的空间执行操作
· out_of_bounds,索引错误
*runtime_error*
· range_error,值域错误，计算结果不在不在函数允许范围内，但没有发生上溢错误和下溢错误
· overflow_error,上溢错误，计算结果超出类型能表示的最大值
· underflow_error,下溢错误，发生在浮点运算中，一般浮点运算存在最小值，超出这个最小值将发生下溢错误
*bad_alloc*
当使用new导致内存分配问题时，将引发bad_alloc异常，
bad_alloc位于头文件<new>中，
```c
try
{
    int* ptr=new int[100000000];
}
catch(bad_alloc& ba)
{
    std::cout<<ba.what()<<std::endl;
    exit(EXIT_FALLURE)；
}
```

# 五、异常嵌套类
将异常类作为嵌套类放在要原类中可以精简代码，增强可读性。

# 六、未捕获异常
当发生异常try-catch未能捕获异常时，程序不会立即终止，而是发生以下行为：
调用unexceptd(),
调用terminate(),
调用about().

*重定向terminate()*
```c
#include<exception>

void myQuit()
{
    std::cout<<"myError!"<<std::endl;
    exit(5);
}

//然后在程序开头使用set_terminate()重定向terminate()
set_terminate(myQuit);
```

相似的unexceptd()也可重定向，但是只能重定向为terminate()，about()和exit()来终止程序
```c
#include<exception>

void myUnexcepted()
{
    //为了捕获所有异常，可以定向为bad_exception
    throw std::bad_exception();
}

//然后在程序开头使用set_terminate()重定向terminate()
set_unexcepted(myUnexceptd);
```
# 七、注意事项
为防止使用new造成内存泄漏，建议使用智能指针。




