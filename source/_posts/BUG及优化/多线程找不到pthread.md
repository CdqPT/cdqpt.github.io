---
title: 多线程找不到pthread
date: 2019-09-16 23:29:00
categories:
- bug
tags:
- 多线程
img: /medias/paperimg/bug.jpg
toc: true
summary: 多线程找不到pthread.h
---

# 一、VS提示找不到pthread.h

下载源码：
https://www.mirrorservice.org/sites/sourceware.org/pub/pthreads-win32/pthreads-w32-2-9-1-release.zip
解压得到三个文件夹：
pthreads.2 里面包含了pthreads 的源代码；

Pre-build.2 里面包含了pthreads for win32 的头文件和已编译好的库文件；

QueueUserAPCEx 里面是一个alert的driver，编译需要DDK 。Windows Device Driver Kit (NTDDK.h) 需要额外单独安装。

这里只使用`pthreads.2`即可，即以下内容都是pthreads.2中的文件。
*配置头文件*
把include文件夹下的头文件拷贝到vs2017安装目录下
D:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\VC\Tools\MSVC\14.11.25503\include\

*配置静态链接库*
把lib文件夹下的静态库文件拷贝到vs2017安装目录下

D:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\VC\Tools\MSVC\14.11.25503\lib

*配置动态链接库*
Pre-built.2\dll\x86下的文件拷贝到C:\Windows\SysWOW64目录下
Pre-built.2\dll\x64下的文件拷贝到C:\Windows\System32目录下

在解决方案目录新建了个ThirdPartyLib目录，与项目目录同级，并把Pre-built.2下的三个文件夹拷过来。

右键项目 - 属性 - 配置属性 - C/C++ - 所有配置，所有平台 - 添加附加包含目录
…\ThirdPartyLib\include;

右键项目 - 属性 - 配置属性 - 链接器 - 所有配置，Win32 - 附加库目录
…\ThirdPartyLib\lib\x86;

右键项目 - 属性 - 配置属性 - 链接器 - 所有配置，Win64 -附加库目录
…\ThirdPartyLib\lib\x64;

右键项目 - 属性 - 配置属性 - 调试 - 环境 - 所有配置，Win32 - 环境
path=%path%;…/ThirdPartyLib/dll/x86;

右键项目 - 属性 - 配置属性 - 调试 - 环境 - 所有配置，Win64 - 环境
path=%path%;…/ThirdPartyLib/dll/x64;


# 二、VS提示：编译错误C2011 “timespec”:“struct”类型重定义

可修改pthread.h文件，在
```c
#if !defined( PTHREAD_H )
#define PTHREAD_H
下面加上一行宏定义
#define HAVE_STRUCT_TIMESPEC
可以解决“timespec”:“struct”类型重定义错误
```

# 三、VS运行的时候提示：`错误 1 error LNK2019: 无法解析的外部符号 __imp__pthread_create，该符号在函数 _main 中被引用`

此时需要在代码中加入
```c
#pragma comment(lib, "pthreadVC2.lib")
```

# 四、VS提示：由于找不到MSVCR100.dll，无法继续执行代码
32位程序下载此链接：
https://www.microsoft.com/zh-TW/download/details.aspx?id=8328
64位程序下载此链接：
https://www.microsoft.com/zh-TW/download/details.aspx?id=13523

这回好使了。

测试程序：
```c

#include <iostream>
// 必须的头文件
#include <pthread.h>

using namespace std;

#pragma comment(lib, "pthreadVC2.lib")
#define NUM_THREADS 5

// 线程的运行函数
void* say_hello(void* args)
{
	cout << "Hello Runoob！" << endl;
	return 0;
}

int main()
{
	// 定义线程的 id 变量，多个变量使用数组
	pthread_t tids[NUM_THREADS];
	for (int i = 0; i < NUM_THREADS; ++i)
	{
		//参数依次是：创建的线程id，线程参数，调用的函数，传入的函数参数
		int ret = pthread_create(&tids[i], NULL, say_hello, NULL);
		if (ret != 0)
		{
			cout << "pthread_create error: error_code=" << ret << endl;
		}
	}
	//等各个线程退出后，进程才结束，否则进程强制结束了，线程可能还没反应过来；
	pthread_exit(NULL);
}
```
```
Hello Runoob！Hello Runoob！

Hello Runoob！Hello Runoob！
Hello Runoob！


C:\Users\cdq\source\repos\thread\Debug\thread.exe (进程 8768)已退出，返回代码为: 0。
若要在调试停止时自动关闭控制台，请启用“工具”->“选项”->“调试”->“调试停止时自动关闭控制台”。
按任意键关闭此窗口...
```

参考链接：
https://blog.csdn.net/cry1994/article/details/79115394
https://blog.csdn.net/weixin_42950931/article/details/81676157
https://blog.csdn.net/oncealong/article/details/85620492