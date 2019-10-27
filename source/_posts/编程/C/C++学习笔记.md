---
title: C++学习笔记
author: 陈德强
date: 2019-06-23 22:28:00
categories:
- 笔记
tags:
- CPlus
toc: true
top: false
img: /medias/paperimg/c.jpg
summary: 对C++的一些理解。
---

1. 私有函数和私有变量的存在是为了重复利用和避免修改，如很常用的变量i；
2. lambda表达式就是将一个结构体简化为一句话；
3. 构造函数与类同名，用于初始化类内部的变量和函数；
4. 别名的作用是，当资源(例如图片)被重命名后，被引用处不需要改变引用路径；
5. this指针用于指定此对象的作用域，限制在当前部件，如果不使用this指针，则是全局范围；
6. 模态就是阻塞其他窗口运行，非模态就是不阻塞其他窗口运行；
7. A.B则A为`对象`或者`结构体`；
   A->B则A为`指针`，->是成员提取，A->B是提取A中的成员B，A只能是指向类、结构、联合的指针；
   ::是`解析运算符`，一般用于在函数外初始化或者赋值。
8. 为什么要使用`using namespace std;`?  因为C++标准程序库中的所有标识符都被定义于一个名为std（standard）的namespace中。
9. 可以在类的外部使用范围解析运算符 :: 定义该函数，如下所示：

double Box::getVolume(void)
{
    return length * breadth * height;
}

10. 保护成员(protected)变量或函数与私有成员(private)十分相似，但有一点不同，保护成员在派生类（即子类）中是可访问的.
11. 类的构造函数是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。

```c
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```

输出如下：
Object is being created
Length of line : 6

12. 拷贝构造函数

拷贝构造函数的作用就类似于变量之间的赋值。举个栗子：
```c
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
 
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
 
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line1(10);
 

 //在此处将line1的所有值和函数都赋值给了line2，就是这么用的。
   Line line2 = line1; // 这里line1也调用了拷贝构造函数
 
   display(line1);
   display(line2);
 
   return 0;
}
```

13. 友元函数
类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。
友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。
如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 friend，

14. 内联函数
如果想把一个函数定义为内联函数，则需要在函数名前面放置关键字 inline，在调用函数之前需要对函数进行定义。
在类定义中的定义的函数都是内联函数，即使没有使用 inline 说明符。

15. this指针
在 C++ 中，每一个对象都能通过 this 指针来访问自己的地址。
我的理解：this是用来代替函数中传入的对象，具有普遍代替的作用。举个栗子：
```c
      int compare(Box box)
      {
         return this->Volume() > box.Volume();
      }
```
如果此处不使用this，那么就需要将this改为box。那么带来的问题就是如果传入的对象是box2，则还是需要改为box2，这样就很麻烦，this的就如就是具有普遍代替，不再需要改变底层。

16. 构造函数
新建对象的时候，参数要包含构造函数的参数和其他函数的参数，同时，构造函数的参数也可以进行初始化。

```c
class Box
{
   public:
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // Length of a box
      double breadth;    // Breadth of a box
      double height;     // Height of a box
};

int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // Declare box1
```
17. 指向类的指针
一个指向 C++ 类的指针与指向结构的指针类似，访问指向类的指针的成员，需要使用成员访问运算符 ->，就像访问指向结构的指针一样。与所有的指针一样，您必须在使用指针之前，对指针进行初始化。

```c
#include <iostream>
 
using namespace std;

class Box
{
   public:
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // Length of a box
      double breadth;    // Breadth of a box
      double height;     // Height of a box
};

int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // Declare box1
   Box Box2(8.5, 6.0, 2.0);    // Declare box2
   Box *ptrBox;                // Declare pointer to a class.

   // 保存第一个对象的地址
   ptrBox = &Box1;

   // 现在尝试使用成员访问运算符来访问成员
   cout << "Volume of Box1: " << ptrBox->Volume() << endl;

   // 保存第二个对象的地址
   ptrBox = &Box2;

   // 现在尝试使用成员访问运算符来访问成员
   cout << "Volume of Box2: " << ptrBox->Volume() << endl;
  
   return 0;
}
```
输出结果：
```
Constructor called.
Constructor called.
Volume of Box1: 5.94
Volume of Box2: 102
```

18. 类的静态成员
我们可以使用 static 关键字来把类成员定义为静态的。当我们声明类的成员为静态时，这意味着无论创建多少个类的对象，静态成员都只有一个副本。
静态成员在类的所有对象中是共享的。如果不存在其他的初始化语句，在创建第一个对象时，所有的静态数据都会被初始化为零。我们不能把静态成员的初始化放置在类的定义中，但是可以在类的外部通过使用范围解析运算符 :: 来重新声明静态变量从而对它进行初始化.

```c
#include <iostream>
 
using namespace std;
 
class Box
{
   public:
      static int objectCount;
      // 构造函数定义
      Box(double l=2.0, double b=2.0, double h=2.0)
      {
         cout <<"Constructor called." << endl;
         length = l;
         breadth = b;
         height = h;
         // 每次创建对象时增加 1
         objectCount++;
      }
      double Volume()
      {
         return length * breadth * height;
      }
   private:
      double length;     // 长度
      double breadth;    // 宽度
      double height;     // 高度
};
 
// 初始化类 Box 的静态成员
int Box::objectCount = 0;
 
int main(void)
{
   Box Box1(3.3, 1.2, 1.5);    // 声明 box1
   Box Box2(8.5, 6.0, 2.0);    // 声明 box2
 
   // 输出对象的总数
   cout << "Total objects: " << Box::objectCount << endl;
 
   return 0;
}
```
输出结果：
```
Constructor called.
Constructor called.
Total objects: 2
```

19. 静态成员函数

如果把函数成员声明为静态的，就可以把函数与类的任何特定对象独立开来。静态成员函数即使在类对象不存在的情况下也能被调用，静态函数只要使用类名加范围解析运算符 :: 就可以访问。

 在C++中，静态成员是属于整个类的而不是某个对象，静态成员变量只存储一份供所有对象共用。所以在所有对象中都可以共享它。使用静态成员变量实现多个对象之间的数据共享不会破坏隐藏的原则，保证了安全性还可以节省内存。

静态成员的定义或声明要加个关键static。静态成员可以通过双冒号来使用即<类名>::<静态成员名>。

我的理解：类的静态变量和静态函数都是为了提供一个在类对象成员之间共享的属性。

```c
class Point
{
public: 
  void init()
  {  
  }
  static void output()
  {
  }
};
void main()
{
  Point::init();
  Point::output();
}
```

20. 继承

基类是我们创建的第一个类，此后继承基类的类都叫做派生类。派生类可以继承基类的`所有`属性，除了`private`外。

```c
#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 派生类
class Rectangle: public Shape
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   return 0;
}
```

```
Total area: 35
```

21. 多态

在子类中定义和基类相同函数名的函数时，子类中的函数定义不能覆盖父类函数。举个栗子：

```c
#include <iostream> 
using namespace std;
 
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      int area()
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};
class Rectangle: public Shape{
   public:
      Rectangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Rectangle class area :" <<endl;
         return (width * height); 
      }
};
class Triangle: public Shape{
   public:
      Triangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Triangle class area :" <<endl;
         return (width * height / 2); 
      }
};
// 程序的主函数
int main( )
{
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);
 
   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();
 
   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();
   
   return 0;
}
```
```
Parent class area
Parent class area
```

但是使用关键字`virtual`就可以让子类中的函数覆盖父类中的函数：

```c
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      virtual int area()
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
};
```

```
Rectangle class area
Triangle class area
```

22. 纯虚函数
虚函数就是使用了virtual关键字的函数，这种函数是动态编译。纯虚函数是通过在声明中使用 "= 0" 来指定的。如果我们在基类中只想定义一个虚函数，而不想定义实际内容，因为字类中的函数会覆盖父类中的函数，因为父类中的虚函数是不需要内容的，那么就要使用虚函数了。

```c
#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
public:
   // 提供接口框架的纯虚函数
   virtual int getArea() = 0;
   void setWidth(int w)
   {
      width = w;
   }
   void setHeight(int h)
   {
      height = h;
   }
protected:
   int width;
   int height;
};
 
// 派生类
class Rectangle: public Shape
{
public:
   int getArea()
   { 
      return (width * height); 
   }
};
class Triangle: public Shape
{
public:
   int getArea()
   { 
      return (width * height)/2; 
   }
};
 
int main(void)
{
   Rectangle Rect;
   Triangle  Tri;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
   // 输出对象的面积
   cout << "Total Rectangle area: " << Rect.getArea() << endl;
 
   Tri.setWidth(5);
   Tri.setHeight(7);
   // 输出对象的面积
   cout << "Total Triangle area: " << Tri.getArea() << endl; 
 
   return 0;
}
```
```
Total Rectangle area: 35
Total Triangle area: 17
```

23. 读写实例

```c
#include <fstream>
#include <iostream>
using namespace std;
 
int main ()
{
    
   char data[100];
 
   // 以写模式打开文件
   ofstream outfile;
   outfile.open("afile.dat");
 
   cout << "Writing to the file" << endl;
   cout << "Enter your name: "; 
   cin.getline(data, 100);
 
   // 向文件写入用户输入的数据
   outfile << data << endl;
 
   cout << "Enter your age: "; 
   cin >> data;
   cin.ignore();
   
   // 再次向文件写入用户输入的数据
   outfile << data << endl;
 
   // 关闭打开的文件
   outfile.close();
 
   // 以读模式打开文件
   ifstream infile; 
   infile.open("afile.dat"); 
 
   cout << "Reading from the file" << endl; 
   infile >> data; 
 
   // 在屏幕上写入数据
   cout << data << endl;
   
   // 再次从文件读取数据，并显示它
   infile >> data; 
   cout << data << endl; 
 
   // 关闭打开的文件
   infile.close();
 
   return 0;
}
```
```
$./a.out
Writing to the file
Enter your name: Zara
Enter your age: 9
Reading from the file
Zara
9
```

24. 异常处理
C++ 异常处理涉及到三个关键字：try、catch、throw。

详见: (https://www.runoob.com/cplusplus/cpp-exceptions-handling.html) [https://www.runoob.com/cplusplus/cpp-exceptions-handling.html] 

25. 动态内存new
栈：在函数内部声明的所有变量都将占用栈内存。
堆：这是程序中未使用的内存，在程序运行时可用于动态分配内存。

使用关键字“new”来动态分配堆内的内存，然后可以使用“delete”运算符删除动态内存。

```c
#include <iostream>
using namespace std;
 
int main ()
{
   double* pvalue  = NULL; // 初始化为 null 的指针
   pvalue  = new double;   // 为变量请求内存
 
   *pvalue = 29494.99;     // 在分配的地址存储值
   cout << "Value of pvalue : " << *pvalue << endl;
 
   delete pvalue;         // 释放内存
 
   return 0;
}
```
```
Value of pvalue : 29495
```

26. 命名空间
定义命名空间：
```c
#include <iostream>
using namespace std;
 
// 第一个命名空间
namespace first_space{
   void func(){
      cout << "Inside first_space" << endl;
   }
}
// 第二个命名空间
namespace second_space{
   void func(){
      cout << "Inside second_space" << endl;
   }
}
int main ()
{
 
   // 调用第一个命名空间中的函数
   first_space::func();
   
   // 调用第二个命名空间中的函数
   second_space::func(); 
 
   return 0;
}
```
```
Inside first_space
Inside second_space
```

27. 多线程使用情况

为什么需要多线程（解释何时考虑使用线程）
从用户的角度考虑，就是为了得到更好的系统服务；从程序自身的角度考虑，就是使目标任务能够尽可能快的完成，更有效的利用系统资源。综合考虑，一般以下场合需要使用多线程：
1、 程序包含复杂的计算任务时
主要是利用多线程获取更多的CPU时间（资源）。
2、 处理速度较慢的外围设备
比如：打印时。再比如网络程序，涉及数据包的收发，时间因素不定。使用独立的线程处理这些任务，可使程序无需专门等待结果。
3、 程序设计自身的需要
WINDOWS系统是基于消息循环的抢占式多任务系统，为使消息循环系统不至于阻塞，程序需要多个线程的来共同完成某些任务。
每个正在系统上运行的程序都是一个进程。每个进程包含一到多个线程。进程也可能是整个程序或者是部分程序的动态执行。线程是一组指令的集合，或者是程序的特殊段，它可以在程序里独立执行。也可以把它理解为代码运行的上下文。所以线程基本上是轻量级的进程，它负责在单个程序里执行多任务。通常由操作系统负责多个线程的调度和执行。









