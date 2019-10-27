
---
title: Windows多线程编程入门笔记
date: 2019-09-14 10:18:00
categories: 笔记
tags: 多线程
toc: 
img: /medias/paperimg/c.jpg
summary: Windows多线程编程入门笔记。
author: 陈德强
top: 
password: 
cover: 
---

每次处理并行任务时，如果要等待用户输入或依赖外部（如与灿亨控制器响应），就应该为类似的操作单独创建一个线程，这样我们的程序才不会挂起无响应。
*静态库和动态库*
静态库是指在程序运行前就编译完成的库，如#include行为；
动态库指在程序运行时进行链接的库，如user32.dll.

# 一、事件处理器和消息传递接口
该程序演示了一个只有关闭功能的窗口
```c

#include <Windows.h> //处理一些视觉特效,例如窗口,控件,枚举,样式

//在创建一个应用程序之前,必须先声明一个窗口过程的原型才能在窗口结构中使用它
LRESULT CALLBACK WndProc(HWND hWnd, UINT uMsg, WPARAM wParam, LPARAM lParam);

//WINAPI或stdcall意味着栈的清理工作由被调用函数来完成
//hThis是应用程序当前实例的句柄;hPrev是应用程序上一个实例的句柄;
//szCmdLine是应用程序的命令行,包括该程序的名称;iCmdShow控制如何显示窗口
int WINAPI WinMain(HINSTANCE hThis, HINSTANCE hPrev, LPSTR szCmdLine, int iCmdShow)
{
	UNREFERENCED_PARAMETER(hPrev); //告诉编译器不能使用某些参数,方便编译器进行一些额外的优化
	UNREFERENCED_PARAMETER(szCmdLine);

	WNDCLASSEX wndEx = { 0 }; //实例化窗口结构
	wndEx.cbClsExtra = 0; //实例化窗口类后分配的额外字节数
	wndEx.cbSize = sizeof(wndEx); //窗口结构的大小(字节为单位)
	wndEx.cbWndExtra = 0; //实例化窗口实例后分配的额外字节数
	wndEx.hbrBackground = (HBRUSH)(COLOR_WINDOW + 1); //窗口类背景画刷的句柄
	wndEx.hCursor = LoadCursor(NULL, IDC_ARROW); //窗口类光标的句柄
	wndEx.hIcon = LoadIcon(NULL, IDI_APPLICATION); //窗口类图标的句柄
	wndEx.hIconSm = LoadIcon(NULL, IDI_APPLICATION); //窗口类图标的句柄
	wndEx.hInstance = hThis; //窗口过程的实例句柄
	wndEx.lpfnWndProc = WndProc; //指向窗口过程的指针
	wndEx.lpszClassName = TEXT("GUIProject"); //指向以空字符结尾的字符串或原子的指针
	wndEx.lpszMenuName = NULL; //指向以空字符结尾的字符串的指针,该字符串指定了窗口类菜单的资源名
	wndEx.style = CS_HREDRAW | CS_VREDRAW;

	if (!RegisterClassEx(&wndEx))
	{
		return -1;
	}

	HWND hWnd = CreateWindow( wndEx.lpszClassName, TEXT("GUI Project"), WS_OVERLAPPEDWINDOW,
		                      200, 200, 400, 300, HWND_DESKTOP, NULL, hThis, 0);
	if (!hWnd)
	{
		return -1;
	}

	UpdateWindow(hWnd);

	ShowWindow(hWnd, iCmdShow); //设置指定窗口的显示状态
	MSG msg = { 0 }; //显示窗口消息

	while (GetMessage(&msg, NULL, NULL, NULL))
	{
		TranslateMessage(&msg); //把虚拟键消息翻译成字符消息
		DispatchMessage(&msg); //分发一条消息给窗口过程
	}

	DestroyWindow(hWnd);
	UnregisterClass(wndEx.lpszClassName, hThis); //注销窗口类,释放该类占用的内存
	return (int)msg.wParam; //从应用程序消息队列中返回一个成功推出代码或最后一个消息代码
}

//hWnd表示窗口标识,uMsg窗口消息代码(无符号整数),wParam和lParam传递应用程序定义的数据(64位长整型数)
//函数返回64位有符号长整型
LRESULT CALLBACK WndProc(HWND hWnd, UINT uMsg, WPARAM wParam, LPARAM lParam)
{
	switch (uMsg)
	{
	case WM_CLOSE:
	{
		PostQuitMessage(0); //释放系统资源,并安全关闭该应用程序
		break;
	}
	default:
	{
		//处理应用程序未处理的窗口信息,函数确保每个消息都被处理
		return DefWindowProc(hWnd, uMsg, wParam, lParam); //默认窗口过程
	}
	}
	return 0;
}

```
出现如下错误:
```
严重性	代码	说明	项目	文件	行	禁止显示状态
警告	C28251	“WinMain”的批注不一致: 此实例包含 无批注。请参见 c:\program files (x86)\windows kits\10\include\10.0.18362.0\um\winbase.h(933)。	GUIProject	C:\USERS\CDQ\SOURCE\REPOS\GUIPROJECT\GUIPROJECT\MAIN.CPP	6	
```
原因是建立的是控制台项目而不是win32项目，解决方法：
```
配置属性-->链接器-->系统-->子系统-->窗口，之后点击确定。
```
参考自：https://bbs.csdn.net/topics/392512424?list=63174050

# 二、解释进程模型
传统操作系统必须提供创建进程和终止进程的方法，下面列出4个引发创建进程的主要事件：
1. 系统初始化；
2. 正在运行的进程执行创建进程的系统调用；
3. 用户要求创建新进程；
4. 启动批处理工作。

创建一个打开Windows笔记本的进程：
```c
#include <iostream>
#include<windows.h>
int main()
{
    std::cout << "Hello World!\n";

	STARTUPINFO startupinfo = { 0 };
	PROCESS_INFORMATION processInfomation = {0};
        //CreateProcess函数用于创建一个新进程和其主线程。
	BOOL bSucess = CreateProcess(TEXT("C:\\windows\\notepad.exe"), NULL, NULL, NULL, FALSE, NULL, NULL, NULL, &startupinfo, &processInfomation);
	if (bSucess)
	{
		std::cout << "Process started." << std::endl
			<< "Process ID:\t" << processInfomation.dwProcessId << std::endl;
	}
	else
	{
		std::cout << "Cannont create process!" << std::endl
			<< "Error code:\t" << GetLastError() << std::endl;
	}
	return system("pause");
}
```
终止进程的较好办法是调用ExtiProcess函数。
# 三、进程状态
运行：该进程正在使用CPU；
就绪：该进程可运行，但是没有CPU可用，还在排队等待其他进程放弃CPU的使用权；
阻塞：在某些外部事件发生前，该进程不能运行。

```c

#include <iostream>
#include<windows.h>
//winternl包含了Windows内部函数大部分原型和数据类型
#include<winternl.h>

using std::cout;
using std::endl;

typedef NTSTATUS(WINAPI* QUERYINFORMATIONPROCESS)(
	HANDLE processHandle,
	PROCESSINFOCLASS processInformationClass,
	PVOID processInformation,
	ULONG processInformationLength,
	PULONG returnLength);

int main()
{
	std::cout << "Hello World!\n";

	STARTUPINFO startupinfo = { 0 };
	PROCESS_INFORMATION processInfomation = { 0 };

	BOOL bSuccess = CreateProcess(TEXT("C:\\windows\\notepad.exe"), NULL, NULL, NULL,
		FALSE, NULL, NULL, NULL, &startupinfo, &processInfomation);
	if (bSuccess)
	{
		std::cout << "Process started." << std::endl
			<< "Process ID:\t" << processInfomation.dwProcessId << std::endl;

		PROCESS_BASIC_INFORMATION pbi;
		ULONG ulength = 0;

		HMODULE hDll = LoadLibrary(TEXT("C:\\windows\\System32\\ntdll.dll"));

		if (hDll)
		{
			QUERYINFORMATIONPROCESS QueryInformationProcess = (QUERYINFORMATIONPROCESS)GetProcAddress(hDll, "NtQueryInformationProcess");
			if (QueryInformationProcess)
			{
				NTSTATUS ntStatus = QueryInformationProcess(processInfomation.hProcess, PROCESSINFOCLASS::ProcessBasicInformation, &pbi, sizeof(pbi), &ulength);
				if (NT_SUCCESS(ntStatus))
				{
					cout << "Process ID (from PCB):\t" << pbi.UniqueProcessId << endl;
				}
				else
				{
					cout << "Cannot open PCB!" << endl
						<< "Error code:\t" << GetLastError() << endl;
				}
			}
			else
			{
				cout << "Cannot get NTQueryInformationProcess function!" << endl
					<< "Error code:\t" << GetLastError() << endl;
			}
			FreeLibrary(hDll);
		}
		else
		{
			cout << "Cannnot load ntdll.dll" << endl
				<< "Error code:\t" << GetLastError() << endl;
		}
	}
	else
	{
		std::cout << "Cannont create process!" << std::endl
			<< "Error code:\t" << GetLastError() << std::endl;
	}
	//CloseHandle(processInfomation.hProcess);
	//CloseHandle(processInfomation.hThread);
	return 0;
}
```

```
Hello World!
Process started.
Process ID:     11492
Process ID (from PCB):  11492
```