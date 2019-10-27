---
title: Java学习笔记
date: 2019-06-29 09:24:00
categories: 笔记
tags: Java
toc: 
img: /medias/paperimg/java.jpg
summary: Java学习笔记。
author: 陈德强
top: 
password: 
cover: 
---

1. Java常量
在 Java 中使用 final 关键字来修饰常量，声明方式和变量类似：

```java
final double PI = 3.1415927;
```

2. Java中的main
Java跟c不一样，java中的main方法不属于任何一个类，它仅仅是一个程序入口，所以你写到哪里都行，当然要在你的项目文件夹里才行

3. 基本语法
类名：对于所有的类来说，类名的首字母应该大写。如果类名由若干单词组成，那么每个单词的首字母应该大写，例如 MyFirstJavaClass 。
方法名：所有的方法名都应该以小写字母开头。如果方法名含有若干单词，则后面的每个单词首字母大写。
主方法入口：所有的 Java 程序由 public static void main(String []args) 方法开始执行。
一个源文件中只能有一个public类；
源文件的名称应该和public类的类名保持一致。例如：源文件中public类的类名是Employee，那么源文件应该命名为Employee.java。
如果一个类定义在某个包中，那么package语句应该在源文件的首行。
我的理解：java是一种面向对象的语言，因此运行的都是类文件，因此主函数main也是放在类中运行的，
Employee.java 
```java
import java.io.*;
 
public class Employee{
   String name;
   int age;
   String designation;
   double salary;
   // Employee 类的构造器
   public Employee(String name){
      this.name = name;
   }
   // 设置age的值
   public void empAge(int empAge){
      age =  empAge;
   }
   /* 设置designation的值*/
   public void empDesignation(String empDesig){
      designation = empDesig;
   }
   /* 设置salary的值*/
   public void empSalary(double empSalary){
      salary = empSalary;
   }
   /* 打印信息 */
   public void printEmployee(){
      System.out.println("名字:"+ name );
      System.out.println("年龄:" + age );
      System.out.println("职位:" + designation );
      System.out.println("薪水:" + salary);
   }
}
```
EmployeeTest.java
```java
import java.io.*;
public class EmployeeTest{
 
   public static void main(String[] args){
      /* 使用构造器创建两个对象 */
      Employee empOne = new Employee("RUNOOB1");
      Employee empTwo = new Employee("RUNOOB2");
 
      // 调用这两个对象的成员方法
      empOne.empAge(26);
      empOne.empDesignation("高级程序员");
      empOne.empSalary(1000);
      empOne.printEmployee();
 
      empTwo.empAge(21);
      empTwo.empDesignation("菜鸟程序员");
      empTwo.empSalary(500);
      empTwo.printEmployee();
   }
}
```
```
名字:RUNOOB1
年龄:26
职位:高级程序员
薪水:1000.0
名字:RUNOOB2
年龄:21
职位:菜鸟程序员
薪水:500.0
```

4. 抽象类和抽象声明
我的理解，抽象类和抽象声明就是只有名称没有内容的声明。

5. synchronized 修饰符
synchronized 关键字声明的方法同一时间只能被一个线程访问。

6. 继承
继承的关键字是`extends`
```java
public class Animal { 
    private String name;   
    private int id; 
    public Animal(String myName, String myid) { 
        //初始化属性值
    } 
    public void eat() {  //吃东西方法的具体实现  } 
    public void sleep() { //睡觉方法的具体实现  } 
} 
 
public class Penguin  extends  Animal{ 
}
```
在java中，不存在多继承，类的继承是单一的。但是可以使用`implements`关键字可以变相的使java具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口（接口跟接口之间采用逗号分隔）。
```java
public interface A {
    public void eat();
    public void sleep();
}
 
public interface B {
    public void show();
}
 
public class C implements A,B {
}
```

7. 重写
重写是子类对父类的允许访问的方法的实现过程进行重新编写, 返回值和形参都不能改变。即外壳不变，核心重写！

```java
class Animal{
   public void move(){
      System.out.println("动物可以移动");
   }
}
 
class Dog extends Animal{
   public void move(){
      System.out.println("狗可以跑和走");
   }
}
 
public class TestDog{
   public static void main(String args[]){
      Animal a = new Animal(); // Animal 对象
      Animal b = new Dog(); // Dog 对象
 
      a.move();// 执行 Animal 类的方法
 
      b.move();//执行 Dog 类的方法
   }
}
```
```
动物可以移动
狗可以跑和走
```

当需要在子类中调用父类的被重写方法时，要使用`super`关键字。
```java
class Animal{
   public void move(){
      System.out.println("动物可以移动");
   }
}
 
class Dog extends Animal{
   public void move(){
      super.move(); // 应用super类的方法
      System.out.println("狗可以跑和走");
   }
}
 
public class TestDog{
   public static void main(String args[]){
 
      Animal b = new Dog(); // Dog 对象
      b.move(); //执行 Dog类的方法
 
   }
}
```

```
动物可以移动
狗可以跑和走
```

8. 重载
重载(overloading) 是在一个类里面，方法名字相同，而参数不同。返回类型可以相同也可以不同。
```java
public class Overloading {
    public int test(){
        System.out.println("test1");
        return 1;
    }
 
    public void test(int a){
        System.out.println("test2");
    }   
 
    //以下两个参数类型顺序不同
    public String test(int a,String s){
        System.out.println("test3");
        return "returntest3";
    }   
 
    public String test(String s,int a){
        System.out.println("test4");
        return "returntest4";
    }   
 
    public static void main(String[] args){
        Overloading o = new Overloading();
        System.out.println(o.test());
        o.test(1);
        System.out.println(o.test(1,"test3"));
        System.out.println(o.test("test4",1));
    }
}
```

9. 接口
接口是一系列抽象类和抽象变量的集合，关键字为`interface`，使用接口后，内部包含的所有内容不用在一一声明为abstract。

```java
/* 文件名 : Animal.java */
interface Animal {
   public void eat();
   public void travel();
}
```

10. 包
为了更好地组织类，Java 提供了包机制，用于区别类名的命名空间。

我的理解，包和命名空间还是类似吧我觉得，都是类似文件夹的东西，作用就是防止命名冲突。
```java
/* 文件名: Animal.java */
package animals;
 
interface Animal {
   public void eat();
   public void travel();
}
```
导入包使用`import`关键字
```java
import package1[.package2…].(classname|*);
```

11. 局部变量
局部变量没有默认值，所以局部变量被声明后，必须经过初始化，才可以使用。

12. continue 关键字
continue 适用于任何循环控制结构中。作用是让程序立刻跳转到下一次循环的迭代。
在 for 循环中，continue 语句使程序立即跳转到更新语句。
在 while 或者 do…while 循环中，程序立即跳转到布尔表达式的判断语句。

13. 获取日期
```java
import java.util.Date;

Date date = new Date();
System.out.println(date.toString());
```
例子：
```java
import java.util.Date;
  
public class DateDemo {
   public static void main(String args[]) {
       // 初始化 Date 对象
       Date date = new Date();
        
       // 使用 toString() 函数显示日期时间
       System.out.println(date.toString());
   }
}
```
```
Mon May 04 09:51:52 CDT 2013
```
其他日期的使用方法可参考：[https://www.runoob.com/java/java-date-time.html](https://www.runoob.com/java/java-date-time.html)

14. 输入与输出
输入：
```java
BufferedReader br = new BufferedReader(new 
                      InputStreamReader(System.in));
```
输出：
```java
System.out.println("abc");
```

举个栗子：
```java
//使用 BufferedReader 在控制台读取字符
 
import java.io.*;
 
public class BRRead {
    public static void main(String args[]) throws IOException {
        char c;
        // 使用 System.in 创建 BufferedReader
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        System.out.println("输入字符, 按下 'q' 键退出。");
        // 读取字符
        do {
            c = (char) br.read();
            System.out.println(c);
        } while (c != 'q');
    }
}
```

```
输入字符, 按下 'q' 键退出。
runoob
r
u
n
o
o
b


q
q
```
或者使用Scanner 类
举个栗子：
```java
import java.util.Scanner; 
 
public class ScannerDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        // 从键盘接收数据
 
        // next方式接收字符串
        System.out.println("next方式接收：");
        // 判断是否还有输入
        if (scan.hasNext()) {
            String str1 = scan.next();
            System.out.println("输入的数据为：" + str1);
        }
        scan.close();
    }
}
```
```
$ javac ScannerDemo.java
$ java ScannerDemo
next方式接收：
runoob com
输入的数据为：runoob
```

15. 异常处理
```
try
{
   // 程序代码
}catch(ExceptionName e1)
{
   //Catch 块
}
```

