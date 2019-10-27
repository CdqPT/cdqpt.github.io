---
title: STM32与Arduino串口通信实验
date: 2019-04-20 10:56:00
categories: 笔记
tags: mcu
toc: true
img: /medias/paperimg/mcu.jpg
summary: STM32与Arduino串口通信实验。
author: 陈德强
top: 
password: 
cover: 
---

首先说明一下，arduino使用的编码方式是utf8，因此stm32的编码方式也要使用utf8才能发送汉字成功。
然后再说明一下，stm32的串口接收协议里需要接收的数据以0x0d和0x0a结尾，即末尾时\r\n,而arduino的串口协议不需要任何结尾。
stm32的编码方式设置方式为：configuration（小扳手）-> editor -> encoding -> encode in utf-8 without signature
因此为了避免格式错乱，推荐使用英文进行发送！
这里直接演示发送字符串的方式，同理发送字符就是一个字母或数字而已。

# 一、arduino发送字符串，stm32接收字符串
实验效果为：arduino发送一次数据，灯闪一次；stm32没收到“你好”时，LED2闪烁，收到“你好”时，LED1闪烁，LED2不再闪烁。

## 1.1 arduino源码
```c
void setup() {
	Serial.begin(9600);
	pinMode(13,OUTPUT);
}

void loop() {
	digitalWrite(13,LOW);
	delay(500);
	Serial.print("你好\r\n");
	digitalWrite(13,HIGH);
	delay(500);
}
```


注意：stm32是使用了我自建的库函数，是德飞莱尼莫stm32的程序。
## 1.2 stm32源码
```c
#include "imut_advance.h"

void SysInit()
{
	delay_init();                       		                        
	LEDInit(0);LEDInit(1);            	                                                          
}



int main(void)
{		
 	char t;  
	u16 len;	
	u8 lalal[]="你好";
	u8 mark=0;

	delay_init();	    	   	 
	LEDInit(2);LEDInit(3); 
	uart_init(9600);

	NVICInit(2,0,USART1_IRQn,2);

 	while(1)
	{
		if(USART_RX_STA&0x8000)
		{					   
			len=USART_RX_STA&0x3fff;
			for(t=0;t<len;t++)
			{
				if(USART_RX_BUF[t]==lalal[t])mark=1;
				else {mark=0;break;}			
				while(USART_GetFlagStatus(USART1,USART_FLAG_TC)!=SET);
			}
			if(mark==1)
			{
				while(1)
				{
					LED3=!LED3;
					delay_ms(500);
				}
			}
			USART_RX_STA=0;
		}	 		
		LED2=!LED2;
		delay_ms(500);	
	}
}
```

# 二、stm32发送字符串，arduino接收字符串
实验效果为：stm32发送完字符串LED2闪烁，arduino接收到字符串时灯闪烁。

## arduino源码
```c
char compare[] = "你好";
char comdata[] = "";//字符串函数
int mark;
void setup()
{
  Serial.begin(9600);
  pinMode(13,OUTPUT);
  }
void loop()
{
  if(Serial.available())
  {
      String comdata = "";//缓存清零
      while (Serial.available() > 0)//循环串口是否有数据
      {
        comdata += char(Serial.read());//叠加数据到comdata
        delay(2);//延时等待响应
      } 
      int i=0;
      int t;
      for (t=0;t<comdata.length() ;t++)
      {
        if(comdata[i]==compare[i])mark=1;
        else{mark=0;break;}
        i++;
      }

   }
   if(mark==1)
  {
      digitalWrite(13,HIGH);
      delay(500);
      digitalWrite(13,LOW);
      delay(500);
   } 
}
```

# 二、STM32程序

```c
#include "imut_advance.h"

int main()
{
	delay_init();                       		                        
	LEDInit(2);LEDInit(3);            		                          
	KeyInit(0);KeyInit(1);KeyInit(2); 		                          
	USARTInit(1,9600);                		                        
    NVICInit(2,0,USART1_IRQn,2); 
	while(1)
	{	
		LED2=!LED2;
		printf("你好");
		delay_ms(500);
	}
}
```

# 三、接线方式

拔掉STM32PA9和PA10的跳冒，

|STM32|Ardino|
|:-------:|:-------:|
|PA9|0|
|PA10|1|

重点来了，STM32和Arduino的电源都不要插在电脑上，否则串口会被占用，嗯，我插在了树莓派上。

此时两个板子的灯都会闪烁。