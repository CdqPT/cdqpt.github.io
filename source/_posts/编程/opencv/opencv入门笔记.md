---
title: opencv入门笔记
author: 陈德强
date: 2019-10-06 16:44:00
categories:
- 笔记
tags:
- opencv
toc: true
top: false
img: /medias/paperimg/opencv.jpg
summary: opencv入门笔记。
---

# 一、图片基本操作
## 1.1 显示图片
```c
#include <opencv2/opencv.hpp>  //头文件
using namespace cv;  //包含cv命名空间

void main( )
{    
	// 【1】读入一张图片，载入图像
	Mat srcImage = imread("1.jpg");
	// 【2】显示载入的图片
	imshow("【原始图】",srcImage);
	// 【3】等待任意按键按下，正数为倒数时间，0和负数为无限
	waitKey(0);
}  
```

## 1.2 图像腐蚀
腐蚀，即用图像中的暗色部分“腐蚀”掉图像中的亮色部分。

```c
#include <opencv2/highgui/highgui.hpp>
#include <opencv2/imgproc/imgproc.hpp>

using namespace cv;


int main()
{
   //载入原图  
   Mat srcImage = imread("C:/Users/Administrator/Desktop/pic.jpg");
   //显示原图
   imshow("【原图】腐蚀操作", srcImage);
   //进行腐蚀操作 
   Mat element = getStructuringElement(MORPH_RECT, Size(15, 15));
   Mat dstImage;
   erode(srcImage, dstImage, element);
   //显示效果图 
   imshow("【效果图】腐蚀操作", dstImage);
   waitKey(0);

   return 0;
}
```
其中， getStructuringElement()函数值为指定形状和尺寸的结构元素(内核矩阵)。

## 1.3 图像模糊

主要使用进行均值滤波操作的blur函数。

```c

#include "opencv2/highgui/highgui.hpp" 
#include "opencv2/imgproc/imgproc.hpp" 
using namespace cv;

int main()
{
   //【1】载入原始图
   Mat srcImage = imread("C:/Users/Administrator/Desktop/pic.jpg");

   //【2】显示原始图
   imshow("均值滤波【原图】", srcImage);

   //【3】进行均值滤波操作
   Mat dstImage;
   blur(srcImage, dstImage, Size(7, 7));

   //【4】显示效果图
   imshow("均值滤波【效果图】", dstImage);

   waitKey(0);
}
```

## 1.4 canny边缘检测


```c

#include <opencv2/opencv.hpp>
#include<opencv2/imgproc/imgproc.hpp>
using namespace cv;

int main()
{
   //【0】载入原始图  
   Mat srcImage = imread("C:/Users/Administrator/Desktop/pic.jpg");  
   imshow("【原始图】Canny边缘检测", srcImage); 	//显示原始图 
   Mat dstImage, edge, grayImage;	//参数定义

   //【1】创建与src同类型和大小的矩阵(dst)
   dstImage.create(srcImage.size(), srcImage.type());

   //【2】将原图像转换为灰度图像
   //此句代码的OpenCV2版为：
   //cvtColor( srcImage, grayImage, CV_BGR2GRAY );
   //此句代码的OpenCV3版为：
   cvtColor(srcImage, grayImage, COLOR_BGR2GRAY);

   //【3】先用使用 3x3内核来降噪
   blur(grayImage, edge, Size(3, 3));

   //【4】运行Canny算子
   Canny(edge, edge, 3, 9, 3);

   //【5】显示效果图 
   imshow("【效果图】Canny边缘检测", edge);

   waitKey(0);

   return 0;
}
```
# 二、读取并播放视频

VedioCapture类。

```c
#include <opencv2\opencv.hpp>  
using namespace cv;


int main()
{
   //【1】读入视频
   VideoCapture capture("C:/Users/Administrator/Desktop/1.avi");

   //【2】循环显示每一帧
   while (1)
   {
      Mat frame;//定义一个Mat变量，用于存储每一帧的图像
      capture >> frame;  //读取当前帧
      imshow("读取视频", frame);  //显示当前帧
      waitKey(30);  //延时30ms
   }
   return 0;
}

```
# 三、调用摄像头采集图像
```c
#include <opencv2\opencv.hpp>  
using namespace cv;


int main()
{
   //【1】从摄像头读入视频
   VideoCapture capture(0);

   //【2】循环显示每一帧
   while (1)
   {
      Mat frame;  //定义一个Mat变量，用于存储每一帧的图像
      capture >> frame;  //读取当前帧
      imshow("读取视频", frame);  //显示当前帧
      waitKey(30);  //延时30ms
   }
   return 0;
}
```

# 四、HighGUI图形界面用户


## 4.1 图像的载入、显示与输出

Mat 类：图像类型，是用于保存图像以及其他矩阵数据的数据结构；
imread():读取图像，返回Mat类，第一个参数为图像名，第二个参数为色彩类型（默认为彩色）；
imshow():用于在指定窗口显示一幅图像，第一个参数是窗口名，第二个参数是要显示的图像（类型为Mat )；
namedWindow():给窗口命名；
imwrite():输出（保存）图片，第一个参数为要命名的图片名称，第二个参数为图片名（InputArray类），第三个参数为要保存的格式（一般不写）。

```c
#include <opencv2/core/core.hpp>
#include <opencv2/highgui/highgui.hpp>
using namespace cv;


int main()
{
   Mat girl = imread("C:/Users/Administrator/Desktop/girl.jpg"); //载入图像到Mat
   namedWindow("【1】动漫图"); //创建一个名为 "【1】动漫图"的窗口  
   imshow("【1】动漫图", girl);//显示名为 "【1】动漫图"的窗口  

   //-----------------------------------【二、初级图像混合】--------------------------------------
   //	描述：二、初级图像混合
   //--------------------------------------------------------------------------------------------------
   //载入图片
   Mat image = imread("C:/Users/Administrator/Desktop/dota.jpg", 199);
   Mat logo = imread("C:/Users/Administrator/Desktop/dota_logo.jpg");

   //载入后先显示
   namedWindow("【2】原画图");
   imshow("【2】原画图", image);

   namedWindow("【3】logo图");
   imshow("【3】logo图", logo);

   //// 定义一个Mat类型，用于存放，图像的ROI
   //Mat imageROI;
   ////方法一
   ////imageROI = image(Rect(800, 350, logo.cols, logo.rows));
   ////方法二
   //imageROI= image(Range(350,350+logo.rows),Range(800,800+logo.cols));

   //// 将logo加到原图上
   //addWeighted(imageROI, 0.5, logo, 0.3, 0., imageROI);

   ////显示结果
   //namedWindow("【4】原画+logo图");
   //imshow("【4】原画+logo图", image);

   //-----------------------------------【三、图像的输出】--------------------------------------
   //	描述：将一个Mat图像输出到图像文件
   //-----------------------------------------------------------------------------------------------
   //输出一张jpg图片到工程目录下
   imwrite("C:/Users/Administrator/Desktop/由imwrite生成的图片.jpg", image);

   waitKey();

   return 0;
}

```

## 4.2 滑动条的创建和使用

```c
#include <opencv2/opencv.hpp>
#include <opencv2/highgui/highgui.hpp>
using namespace cv;


#define WINDOW_NAME "【滑动条的创建&线性混合示例】"        //为窗口标题定义的宏 

const int g_nMaxAlphaValue = 100;//Alpha值的最大值
int g_nAlphaValueSlider;//滑动条对应的变量
double g_dAlphaValue;
double g_dBetaValue;

//声明存储图像的变量
Mat g_srcImage1;
Mat g_srcImage2;
Mat g_dstImage;


//-----------------------------------【on_Trackbar( )函数】--------------------------------
//		描述：响应滑动条的回调函数
//------------------------------------------------------------------------------------------
void on_Trackbar(int, void*)
{
   //求出当前alpha值相对于最大值的比例
   g_dAlphaValue = (double)g_nAlphaValueSlider / g_nMaxAlphaValue;
   //则beta值为1减去alpha值
   g_dBetaValue = (1.0 - g_dAlphaValue);

   //根据alpha和beta值进行线性混合
   addWeighted(g_srcImage1, g_dAlphaValue, g_srcImage2, g_dBetaValue, 0.0, g_dstImage);

   //显示效果图
   imshow(WINDOW_NAME, g_dstImage);
}


//-----------------------------【ShowHelpText( )函数】--------------------------------------
//		描述：输出帮助信息
//-------------------------------------------------------------------------------------------------
//-----------------------------------【ShowHelpText( )函数】----------------------------------
//		描述：输出一些帮助信息
//----------------------------------------------------------------------------------------------
void ShowHelpText()
{
   //输出欢迎信息和OpenCV版本
   printf("\n\n\t\t\t非常感谢购买《OpenCV3编程入门》一书！\n");
   printf("\n\n\t\t\t此为本书OpenCV3版的第17个配套示例程序\n");
   printf("\n\n\t\t\t   当前使用的OpenCV版本为：" CV_VERSION);
   printf("\n\n  ----------------------------------------------------------------------------\n");
}


//--------------------------------------【main( )函数】-----------------------------------------
//		描述：控制台应用程序的入口函数，我们的程序从这里开始执行
//-----------------------------------------------------------------------------------------------
int main(int argc, char** argv)
{

   //显示帮助信息
   ShowHelpText();

   //加载图像 (两图像的尺寸需相同)
   g_srcImage1 = imread("C:/Users/Administrator/Desktop/1.jpg");
   g_srcImage2 = imread("C:/Users/Administrator/Desktop/2.jpg");
   if (!g_srcImage1.data) { printf("读取第一幅图片错误，请确定目录下是否有imread函数指定图片存在~！ \n"); return -1; }
   if (!g_srcImage2.data) { printf("读取第二幅图片错误，请确定目录下是否有imread函数指定图片存在~！\n"); return -1; }

   //设置滑动条初值为70
   g_nAlphaValueSlider = 70;

   //创建窗体
   namedWindow(WINDOW_NAME, 1);

   //在创建的窗体中创建一个滑动条控件
   char TrackbarName[50];
   printf(TrackbarName, "透明值 %d", g_nMaxAlphaValue);

   createTrackbar(TrackbarName, WINDOW_NAME, &g_nAlphaValueSlider, g_nMaxAlphaValue, on_Trackbar);

   //结果在回调函数中显示
   on_Trackbar(g_nAlphaValueSlider, 0);

   //按任意键退出
   waitKey(0);

   return 0;
}

```

createTrackbar这个函数我们以后会经常用到，它创建一个可以调整数值的轨迹条，并将轨迹条附加到指定的窗口上，使用起来很方便。首先大家要记住，它往往会和一个回调函数配合起来使用。先看下他的函数原型：
```c
C++: int createTrackbar(conststring& trackbarname, conststring& winname, int* value, int count, TrackbarCallback onChange=0,void* userdata=0);
```

>第一个参数，const string&类型的trackbarname，表示轨迹条的名字，用来代表我们创建的轨迹条。
第二个参数，const string&类型的winname，填窗口的名字，表示这个轨迹条会依附到哪个窗口上，即对应namedWindow（）创建窗口时填的某一个窗口名。
第三个参数，int* 类型的value，一个指向整型的指针，表示滑块的位置。并且在创建时，滑块的初始位置就是该变量当前的值。
第四个参数，int类型的count，表示滑块可以达到的最大位置的值。PS:滑块最小的位置的值始终为0。
第五个参数，TrackbarCallback类型的onChange，首先注意他有默认值0。这是一个指向回调函数的指针，每次滑块位置改变时，这个函数都会进行回调。并且这个函数的原型必须为void XXXX(int,void*);其中第一个参数是轨迹条的位置，第二个参数是用户数据（看下面的第六个参数）。如果回调是NULL指针，表示没有回调函数的调用，仅第三个参数value有变化。
>第六个参数，void*类型的userdata，他也有默认值0。这个参数是用户传给回调函数的数据，用来处理轨迹条事件。如果使用的第三个参数value实参是全局变量的话，完全可以不去管这个userdata参数。

getTrackbarPos函数,
用于获取当前轨迹条的位置并返回。

```c
C++: int getTrackbarPos(conststring& trackbarname, conststring& winname);
```

## 4.3 鼠标操作

```c

#include <opencv2/opencv.hpp>
using namespace cv;

#define WINDOW_NAME "【程序窗口】"        //为窗口标题定义的宏 


void on_MouseHandle(int event, int x, int y, int flags, void* param);
void DrawRectangle(cv::Mat& img, cv::Rect box);
void ShowHelpText();

//-----------------------------------【全局变量声明部分】-----------------------------------
//		描述：全局变量的声明
//-----------------------------------------------------------------------------------------------
Rect g_rectangle;
bool g_bDrawingBox = false;//是否进行绘制
RNG g_rng(12345);



int main(int argc, char** argv)
{
   //【0】改变console字体颜色
   system("color 9F");

   //【0】显示欢迎和帮助文字
   ShowHelpText();

   //【1】准备参数
   g_rectangle = Rect(-1, -1, 0, 0);
   Mat srcImage(600, 800, CV_8UC3), tempImage;
   srcImage.copyTo(tempImage);
   g_rectangle = Rect(-1, -1, 0, 0);
   srcImage = Scalar::all(0);

   //【2】设置鼠标操作回调函数
   namedWindow(WINDOW_NAME);
   setMouseCallback(WINDOW_NAME, on_MouseHandle, (void*)&srcImage);

   //【3】程序主循环，当进行绘制的标识符为真时，进行绘制
   while (1)
   {
      srcImage.copyTo(tempImage);//拷贝源图到临时变量
      if (g_bDrawingBox) DrawRectangle(tempImage, g_rectangle);//当进行绘制的标识符为真，则进行绘制
      imshow(WINDOW_NAME, tempImage);
      if (waitKey(10) == 27) break;//按下ESC键，程序退出
   }
   return 0;
}



//--------------------------------【on_MouseHandle( )函数】-----------------------------
//		描述：鼠标回调函数，根据不同的鼠标事件进行不同的操作
//-----------------------------------------------------------------------------------------------
void on_MouseHandle(int event, int x, int y, int flags, void* param)
{

   Mat& image = *(cv::Mat*) param;
   switch (event)
   {
      //鼠标移动消息
   case EVENT_MOUSEMOVE:
   {
      if (g_bDrawingBox)//如果是否进行绘制的标识符为真，则记录下长和宽到RECT型变量中
      {
         g_rectangle.width = x - g_rectangle.x;
         g_rectangle.height = y - g_rectangle.y;
      }
   }
   break;

   //左键按下消息
   case EVENT_LBUTTONDOWN:
   {
      g_bDrawingBox = true;
      g_rectangle = Rect(x, y, 0, 0);//记录起始点
   }
   break;

   //左键抬起消息
   case EVENT_LBUTTONUP:
   {
      g_bDrawingBox = false;//置标识符为false
      //对宽和高小于0的处理
      if (g_rectangle.width < 0)
      {
         g_rectangle.x += g_rectangle.width;
         g_rectangle.width *= -1;
      }

      if (g_rectangle.height < 0)
      {
         g_rectangle.y += g_rectangle.height;
         g_rectangle.height *= -1;
      }
      //调用函数进行绘制
      DrawRectangle(image, g_rectangle);
   }
   break;

   }
}



//-----------------------------------【DrawRectangle( )函数】------------------------------
//		描述：自定义的矩形绘制函数
//-----------------------------------------------------------------------------------------------
void DrawRectangle(cv::Mat& img, cv::Rect box)
{
   cv::rectangle(img, box.tl(), box.br(), cv::Scalar(g_rng.uniform(0, 255), g_rng.uniform(0, 255), g_rng.uniform(0, 255)));//随机颜色
}


//-----------------------------------【ShowHelpText( )函数】-----------------------------
//          描述：输出一些帮助信息
//----------------------------------------------------------------------------------------------
void ShowHelpText()
{
   //输出欢迎信息和OpenCV版本
   printf("\n\n\t\t\t非常感谢购买《OpenCV3编程入门》一书！\n");
   printf("\n\n\t\t\t此为本书OpenCV3版的第18个配套示例程序\n");
   printf("\n\n\t\t\t   当前使用的OpenCV版本为：" CV_VERSION);
   printf("\n\n  ----------------------------------------------------------------------------\n");
   //输出一些帮助信息
   printf("\n\n\n\t欢迎来到【鼠标交互演示】示例程序\n");
   printf("\n\n\t请在窗口中点击鼠标左键并拖动以绘制矩形\n");

}

```




参考链接：
https://blog.csdn.net/zhmxy555/article/category/9262318