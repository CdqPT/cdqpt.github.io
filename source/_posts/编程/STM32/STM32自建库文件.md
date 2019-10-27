---
title: STM32自建库文件
author: 陈德强
date: 2019-04-08 18:17:00
categories: 笔记
tags: mcu
toc: true
top: false
img: /medias/paperimg/mcu.jpg
summary: STM32自建库文件
---

# 一、imut.c文件
```c
# include "imut.h"

/***********************************************************************
//函数名称：STM32常用外设函数
//作用：包括LED初始化、按键初始化、串口初始化等。LED分0,1两个；按键分0,1,2三个；串口分1,2两个。
//示例：SysInit();
//作者：陈德强
//时间：2019年4月11日08:06:28
//修改者：
//修改时间：
//修改说明：
************************************************************************/
void LEDInit(u8 LED)
{
	switch(LED)
	{
		case 0:GPIOInit(GPIOB,5,0);break;
		case 1:GPIOInit(GPIOE,5,0);break;
	}
}
void KeyInit(u8 KEY)
{
	switch(KEY)
	{
		case 0:GPIOInit(GPIOE,4,1);break;
		case 1:GPIOInit(GPIOE,3,1);break;	
		case 2:GPIOInit(GPIOA,0,-1);break;
	}
}
void USARTInit(u8 usart,int paudrate)
{
	switch(usart)
	{
		case 1:uart_init(paudrate);
		case 2:USART2_Init(paudrate);
	}
}

/*************************************************************************
//函数名称：IO口初始化函数封装
//作用：第一个参数是GPIO口，第二个参数是引脚号，第三个参数是输出输入模式选择，0是输出，1是上拉输入，-1是下拉输入。
//说明：输出是推挽输出50HZ，输入默认下拉输入。模拟输入和复用功能需单独设置。
//示例：GPIOInit（GPIOB,5,1）
//作者：陈德强
//时间：2019年4月8日17:29:05
//修改者：
//修改时间：
//修改说明：
***************************************************************************/
void GPIOInit(GPIO_TypeDef * GPIOx,u8 Pin,int OutOrIN)
{
	if(GPIOx==GPIOA)RCC->APB2ENR|=1<<2;
	if(GPIOx==GPIOB)RCC->APB2ENR|=1<<3;
	if(GPIOx==GPIOC)RCC->APB2ENR|=1<<4;
	if(GPIOx==GPIOD)RCC->APB2ENR|=1<<5;
	if(GPIOx==GPIOE)RCC->APB2ENR|=1<<6;
	if(GPIOx==GPIOF)RCC->APB2ENR|=1<<7;
	if(GPIOx==GPIOG)RCC->APB2ENR|=1<<8;
	
	if(OutOrIN==0)
	{
		switch (Pin)
		{
			case 0:
				GPIOx->CRL&=0XFFFFFFF0;GPIOx->CRL|=0X00000003;break;
			case 1:
				GPIOx->CRL&=0XFFFFFF0F;GPIOx->CRL|=0X00000030;break;
			case 2:
				GPIOx->CRL&=0XFFFFF0FF;GPIOx->CRL|=0X00000300;break;
			case 3:
				GPIOx->CRL&=0XFFFF0FFF;GPIOx->CRL|=0X00003000;break;
			case 4:
				GPIOx->CRL&=0XFFF0FFFF;GPIOx->CRL|=0X00030000;break;
			case 5:
				GPIOx->CRL&=0XFF0FFFFF;GPIOx->CRL|=0X00300000;break;
			case 6:
				GPIOx->CRL&=0XF0FFFFFF;GPIOx->CRL|=0X03000000;break;
			case 7:
				GPIOx->CRL&=0X0FFFFFFF;GPIOx->CRL|=0X30000000;break;
			case 8:
				GPIOx->CRH&=0XFFFFFFF0;GPIOx->CRH|=0X00000003;break;
			case 9:
				GPIOx->CRH&=0XFFFFFF0F;GPIOx->CRH|=0X00000030;break;			
			case 10:
				GPIOx->CRH&=0XFFFFF0FF;GPIOx->CRH|=0X00000300;break;	
			case 11:
				GPIOx->CRH&=0XFFFF0FFF;GPIOx->CRH|=0X00003000;break;
			case 12:
				GPIOx->CRH&=0XFFF0FFFF;GPIOx->CRH|=0X00030000;break;
			case 13:
				GPIOx->CRH&=0XFF0FFFFF;GPIOx->CRH|=0X00300000;break;
			case 14:
				GPIOx->CRH&=0XF0FFFFFF;GPIOx->CRH|=0X03000000;break;
			case 15:
				GPIOx->CRH&=0X0FFFFFFF;GPIOx->CRH|=0X30000000;break;	
			default:break; 
		}
	}
	
	if(OutOrIN==1)
	{
		switch (Pin)
		{
			case 0:
				GPIOx->CRL&=0XFFFFFFF0;GPIOx->CRL|=0X00000008;GPIOx->ODR|=1<<0;break;
			case 1:
				GPIOx->CRL&=0XFFFFFF0F;GPIOx->CRL|=0X00000080;GPIOx->ODR|=1<<1;break;		
			case 2:
				GPIOx->CRL&=0XFFFFF0FF;GPIOx->CRL|=0X00000800;GPIOx->ODR|=1<<2;break;	
			case 3:
				GPIOx->CRL&=0XFFFF0FFF;GPIOx->CRL|=0X00008000;GPIOx->ODR|=1<<3;break;
			case 4:
				GPIOx->CRL&=0XFFF0FFFF;GPIOx->CRL|=0X00080000;GPIOx->ODR|=1<<4;break;
			case 5:
				GPIOx->CRL&=0XFF0FFFFF;GPIOx->CRL|=0X00800000;GPIOx->ODR|=1<<5;break;
			case 6:
				GPIOx->CRL&=0XF0FFFFFF;GPIOx->CRL|=0X08000000;GPIOx->ODR|=1<<6;break;
			case 7:
				GPIOx->CRL&=0X0FFFFFFF;GPIOx->CRL|=0X80000000;GPIOx->ODR|=1<<7;break;
			case 8:
				GPIOx->CRH&=0XFFFFFFF0;GPIOx->CRH|=0X00000008;GPIOx->ODR|=1<<0;break;
			case 9:
				GPIOx->CRH&=0XFFFFFF0F;GPIOx->CRH|=0X00000080;GPIOx->ODR|=1<<1;break;			
			case 10:
				GPIOx->CRH&=0XFFFFF0FF;GPIOx->CRH|=0X00000800;GPIOx->ODR|=1<<2;break;	
			case 11:
				GPIOx->CRH&=0XFFFF0FFF;GPIOx->CRH|=0X00008000;GPIOx->ODR|=1<<3;break;
			case 12:
				GPIOx->CRH&=0XFFF0FFFF;GPIOx->CRH|=0X00080000;GPIOx->ODR|=1<<4;break;
			case 13:
				GPIOx->CRH&=0XFF0FFFFF;GPIOx->CRH|=0X00800000;GPIOx->ODR|=1<<5;break;
			case 14:
				GPIOx->CRH&=0XF0FFFFFF;GPIOx->CRH|=0X08000000;GPIOx->ODR|=1<<6;break;;
			case 15:
				GPIOx->CRH&=0X0FFFFFFF;GPIOx->CRH|=0X80000000;GPIOx->ODR|=1<<7;break;
			default:break; 			
		}		
	}
	
	if(OutOrIN==-1)
	{
		switch (Pin)
		{
			case 0:
				GPIOx->CRL&=0XFFFFFFF0;GPIOx->CRL|=0X00000008;break;
			case 1:
				GPIOx->CRL&=0XFFFFFF0F;GPIOx->CRL|=0X00000080;break;		
			case 2:
				GPIOx->CRL&=0XFFFFF0FF;GPIOx->CRL|=0X00000800;break;	
			case 3:
				GPIOx->CRL&=0XFFFF0FFF;GPIOx->CRL|=0X00008000;break;
			case 4:
				GPIOx->CRL&=0XFFF0FFFF;GPIOx->CRL|=0X00080000;break;
			case 5:
				GPIOx->CRL&=0XFF0FFFFF;GPIOx->CRL|=0X00800000;break;
			case 6:
				GPIOx->CRL&=0XF0FFFFFF;GPIOx->CRL|=0X08000000;break;
			case 7:
				GPIOx->CRL&=0X0FFFFFFF;GPIOx->CRL|=0X80000000;break;
			case 8:
				GPIOx->CRH&=0XFFFFFFF0;GPIOx->CRH|=0X00000008;break;
			case 9:
				GPIOx->CRH&=0XFFFFFF0F;GPIOx->CRH|=0X00000080;break;			
			case 10:
				GPIOx->CRH&=0XFFFFF0FF;GPIOx->CRH|=0X00000800;break;	
			case 11:
				GPIOx->CRH&=0XFFFF0FFF;GPIOx->CRH|=0X00008000;break;
			case 12:
				GPIOx->CRH&=0XFFF0FFFF;GPIOx->CRH|=0X00080000;break;
			case 13:
				GPIOx->CRH&=0XFF0FFFFF;GPIOx->CRH|=0X00800000;break;
			case 14:
				GPIOx->CRH&=0XF0FFFFFF;GPIOx->CRH|=0X08000000;break;;
			case 15:
				GPIOx->CRH&=0X0FFFFFFF;GPIOx->CRH|=0X80000000;break;
			default:break; 			
		}		
	}
}

/**************************************************************************
//函数名称：单个按键扫描程序
//作用：直连按键扫描程序，包括KE0(PE4)。//mode=1，支持连按。
//示例：KEY_Scan(0)
//作者：陈德强
//时间：2019年4月8日17:29:05
//修改者：
//修改时间：
//修改说明：
***************************************************************************/

u8 KEY_Scan(u8 mode)
{	 
	static u8 key_up=1;//按键按松开标志
	if(mode)key_up=1;  //支持连按		  
	if(key_up&&(KEY0==0||KEY1==0||WakeUP==1))
	{
		delay_ms(10);//去抖动 
		key_up=0;
		if(KEY0==0)return 1;
		else if(KEY1==0)return 2;
		else if(WakeUP==1)return 3;
	}else if(KEY0==1&&KEY1==1&&WakeUP==0)key_up=1; 	    
 	return 0;// 无按键按下
}

/******************************************************************************
//函数名称：UART2初始化函数及中断函数
//作用：UART2初始化函数。注：使用时用杜邦线将PA2和P9下的RXD连接，将PA3和TXD连接。
//示例：USART2_Init(115200);
//作者：陈德强
//时间：2019年4月10日17:16:36
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/
void USART2_Init(int BaudRate)
{
	GPIO_InitTypeDef GPIO_InitStrue;
	USART_InitTypeDef USART_InitStrue;
	
	//使能引脚A
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA,ENABLE);
	//使能串口复用功能
	RCC_APB1PeriphClockCmd(RCC_APB1Periph_USART2,ENABLE);
	
	/*----------------------配置引脚PA2和PA3为复用功能----------------*/
	GPIO_InitStrue.GPIO_Mode=GPIO_Mode_AF_PP;
	GPIO_InitStrue.GPIO_Pin=GPIO_Pin_2;
	GPIO_InitStrue.GPIO_Speed=GPIO_Speed_10MHz;
  GPIO_Init(GPIOA,&GPIO_InitStrue);
	
	GPIO_InitStrue.GPIO_Mode=GPIO_Mode_IN_FLOATING;
	GPIO_InitStrue.GPIO_Pin=GPIO_Pin_3;
	GPIO_InitStrue.GPIO_Speed=GPIO_Speed_10MHz;
  GPIO_Init(GPIOA,&GPIO_InitStrue);
	/*----------------------------------------------------------------*/
	
	/*-------------------------配置USART2-----------------------------*/
	USART_InitStrue.USART_BaudRate=BaudRate;
	USART_InitStrue.USART_HardwareFlowControl=USART_HardwareFlowControl_None;
	USART_InitStrue.USART_Mode=USART_Mode_Tx|USART_Mode_Rx;
	USART_InitStrue.USART_Parity=USART_Parity_No;
	USART_InitStrue.USART_StopBits=USART_StopBits_1;
	USART_InitStrue.USART_WordLength=USART_WordLength_8b;
	USART_Init(USART2,&USART_InitStrue);//③
	/*----------------------------------------------------------------*/
	
	//使能串口1
	USART_Cmd(USART2,ENABLE);
	//开启接收中断
	USART_ITConfig(USART2,USART_IT_RXNE,ENABLE);
		
}
/*----------------------------串口2中断服务函数--------------------------------*/	

void USART2_IRQHandler(void)
{
	u8 res;
	static int num;
	 if(USART_GetITStatus(USART2,USART_IT_RXNE)) //当接收寄存器接收到数据时
 {
	 res= USART_ReceiveData(USART2); //将数据放在res变量中
	 USART_SendData(USART2,res); //将数据发送到串口，这时电脑串口接收到数据，并在串口调试助手显示出来。
	 num++;
	 if(num==5)
	 {
	   num=0;
		 LED0 =1;
	 }		
  }
}
/*----------------------------------------------------------------------------*/	

/******************************************************************************
//函数名称：外部中断初始化函数及中断函数
//作用：外部中断初始化函数。带注释的地方需要改。1上升沿，0上升沿。
//示例：EXTIX_Init(GPIOE,3,0);
//说明：PE3引脚，下降沿
//作者：陈德强
//时间：2019年4月11日00:56:11
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/

void ExtiInit(GPIO_TypeDef * GPIOx,u8 BITx,u8 UpOrDown)
{
	u8 EXTADDR;
	u8 EXTOFFSET;
	u8 gpiox;
	if(GPIOx==GPIOA)gpiox=0;
  if(GPIOx==GPIOB)gpiox=1;	
	if(GPIOx==GPIOC)gpiox=2;
	if(GPIOx==GPIOD)gpiox=3;
	if(GPIOx==GPIOE)gpiox=4;
	if(GPIOx==GPIOF)gpiox=5;
	if(GPIOx==GPIOG)gpiox=6;
	EXTADDR=BITx/4;
	EXTOFFSET=(BITx%4)*4; 
	RCC->APB2ENR|=0x01;		 
	AFIO->EXTICR[EXTADDR]&=~(0x000F<<EXTOFFSET);
	AFIO->EXTICR[EXTADDR]|=gpiox<<EXTOFFSET;
	EXTI->IMR|=1<<BITx;
 	if(UpOrDown==0)EXTI->FTSR|=1<<BITx;
	if(UpOrDown==1)EXTI->RTSR|=1<<BITx;	  	
}

/*----------------------外部中断3服务程序中断服务函数--------------------------*/
void EXTI3_IRQHandler(void)
{
	delay_ms(10);//消抖
	if(KEY1==0)	 //按键KEY1
	{		
		LED0=!LED0;
		LED1=!LED1;
	}		 
	EXTI_ClearITPendingBit(EXTI_Line3);  //清除LINE3上的中断标志位  
}

/******************************************************************************
//函数名称：NVIC设置函数
//作用：设置NVIC。数值越小，优先级越高。
//示例：NVICInit(2,1,EXTI3_IRQn,2);
//说明：设置抢占优先级为2，子优先级为1，中断类型为EXTI3_IRQn，中断分组方式为组2.
//作者：陈德强
//时间：2019年4月11日00:52:32
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/   

void NVICPriorityGroupConfig(u8 NVIC_Group)	 
{ 
	u32 temp,temp1;	  
	temp1=(~NVIC_Group)&0x07;
	temp1<<=8;
	temp=SCB->AIRCR;  
	temp&=0X0000F8FF; 
	temp|=0X05FA0000; 
	temp|=temp1;	   
	SCB->AIRCR=temp;      	  				   
}
void NVICInit(u8 NVIC_PreemptionPriority,u8 NVIC_SubPriority,u8 NVIC_Channel,u8 NVIC_Group)	 
{ 
	u32 temp;	
	NVICPriorityGroupConfig(NVIC_Group);
	temp=NVIC_PreemptionPriority<<(4-NVIC_Group);	  
	temp|=NVIC_SubPriority&(0x0f>>NVIC_Group);
	temp&=0xf;								  
	NVIC->ISER[NVIC_Channel/32]|=(1<<NVIC_Channel%32);
	NVIC->IP[NVIC_Channel]|=temp<<4;		  	    	  				   
} 

/******************************************************************************
//函数名称：独立看门狗初始化
//作用：程序卡死时自动复位。
//示例：IWDG_Init(4,625);//约为1s
//说明：时间计算(大概):Tout=((4*2^prer)*rlr)/40 (ms).
//作者：陈德强
//时间：2019年4月11日08:20:43
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/   
void IWDG_Init(u8 prer,u16 rlr) 
{	
 	IWDG_WriteAccessCmd(IWDG_WriteAccess_Enable);  	
	IWDG_SetPrescaler(prer);  
	IWDG_SetReload(rlr);  
	IWDG_ReloadCounter();  	
	IWDG_Enable();  
}

/******************************************************************************
//函数名称：通用定时器初始化函数
//作用：初始化通用定时器，即定时器2、3、4、5。
//示例：TIMInit(TIM3,500);
//说明：500ms更新一次，arr和psc随便设置，不超过2^16-1就行。.
//作者：陈德强
//时间：2019年4月11日09:12:40
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/ 
void TIMInit(TIM_TypeDef* TIMx,u16 uptime)
{
	u8 addr;
	if(TIMx==TIM2)addr=1;
	if(TIMx==TIM3)addr=2;	
	if(TIMx==TIM4)addr=3;	
	if(TIMx==TIM5)addr=4;	
	RCC->APB1ENR|=1<<addr;	 
 	TIMx->ARR=uptime*10-1;  
	TIMx->PSC=7200-1;		  
	TIMx->DIER|=1<<0; 
	TIMx->CR1|=0x01;							 
}
/*---------------------------定时器3中断服务程序------------------------------*/
void TIM3_IRQHandler(void)   
{
	if (TIM_GetITStatus(TIM3, TIM_IT_Update) != RESET)  
		{
		TIM_ClearITPendingBit(TIM3, TIM_IT_Update  );   
		LED1=!LED1;
		}
}

/******************************************************************************
//函数名称：PWM初始化函数
//作用：初始化PWM。TIM2-TIM5,每个定时器4个通道。每个通道有固定引脚。
//      目前仅写了PWMInit(TIM2,2,900);和PWMInit(TIM3,1,900);和PWMInit(TIM3,2,900);
//示例：PWMInit(TIM2,1,900);//启用定时器2的CH1通道，自动装载值为0-899。
//说明：PWM频率=72000000/900=80Khz。
//PWM的调用函数：TIM_SetCompare2(TIM2,100);	//第二个参数要在自动装载值的范围内，即0-899
//作者：陈德强
//时间：2019年4月11日12:23:19
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/ 
void PWMInit(TIM_TypeDef *TIMx,u8 CHx,u16 arr)
{		 					 
	u8 addr;
	if(TIMx==TIM2)addr=1;
	if(TIMx==TIM3)addr=2;	
	if(TIMx==TIM4)addr=3;	
	if(TIMx==TIM5)addr=4;	
	RCC->APB1ENR|=1<<addr;	   

	if((TIMx==TIM2)&&(CHx==2))//TIM2的CH2通道PA1
	{
		RCC->APB1ENR|=1<<0; 		//TIM2时钟使能    
		RCC->APB2ENR|=1<<1;    	//使能PORTA时钟	
		GPIOA->CRL&=0XFFFFFF0F;	//PA1输出
		GPIOA->CRL|=0X000000B0;	//复用功能输出 	  	 	 
		RCC->APB2ENR|=0x01;     	   
		TIMx->ARR=arr-1;			
		TIMx->PSC=0;			
		TIMx->CCMR1|=7<<12;  	 
		TIMx->CCMR1|=1<<11; 	   
		TIMx->CCER|=1<<4;   	   
		TIMx->CR1=0x0080;   	
		TIMx->CR1|=0x01;    	 		
	}	
	if((TIMx==TIM3)&&(CHx==1))//TIM3的CH1通道PB5
	{
		RCC->APB1ENR|=1<<1; 	    
		RCC->APB2ENR|=1<<3;    	
		GPIOB->CRL&=0XFF0FFFFF;	
		GPIOB->CRL|=0X00B00000;		  	 		 
		RCC->APB2ENR|=1<<0;       
		AFIO->MAPR&=0XFFFFF3FF; 
		AFIO->MAPR|=1<<11;      
		TIM3->ARR=arr;			
		TIM3->PSC=0;			
		TIM3->CCMR1|=7<<12;  		 
		TIM3->CCMR1|=1<<11; 		   
		TIM3->CCER|=1<<4;   	   
		TIM3->CR1=0x0080;   	
		TIM3->CR1|=0x01;    	
	}
		if((TIMx==TIM3)&&(CHx==2))//TIM3的CH2通道PA7
	{
		RCC->APB1ENR|=1<<1; 		  
		RCC->APB2ENR|=1<<1;    		
		GPIOA->CRL&=0X0FFFFFFF;	
		GPIOA->CRL|=0XB0000000;		  	 	 
		RCC->APB2ENR|=0x01;     	   
		TIMx->ARR=arr-1;			
		TIMx->PSC=0;			
		TIMx->CCMR1|=7<<12;  	 
		TIMx->CCMR1|=1<<11; 	   
		TIMx->CCER|=1<<4;   	   
		TIMx->CR1=0x0080;   	
		TIMx->CR1|=0x01;    	 		
	}	
}  

/******************************************************************************
//函数名称：输入捕获初始化函数
//作用：捕获PA0的上升沿，并输出结果。
//示例：CapInit(0XFFFF,72-1);
//说明：
//PWM的调用函数：由定时器中断处理。
//作者：陈德强
//时间：2019年4月11日16:56:00
//修改者：
//修改时间：
//修改说明：
*******************************************************************************/ 
void CapInit(u16 arr,u16 psc)
{		 
	RCC->APB1ENR|=1<<3;   	//TIM5 时钟使能 
	RCC->APB2ENR|=1<<2;    	//使能PORTA时钟  
	 
	GPIOA->CRL&=0XFFFFFFF0;	//PA0 清除之前设置  
	GPIOA->CRL|=0X00000008;	//PA0 输入   
	GPIOA->ODR|=0<<0;		//PA0 下拉
	  
 	TIM5->ARR=arr;  		//设定计数器自动重装值   
	TIM5->PSC=psc;  		//预分频器 

	TIM5->CCMR1|=1<<0;		//CC1S=01 	选择输入端 IC1映射到TI1上
 	TIM5->CCMR1|=0<<4; 		//IC1F=0000 配置输入滤波器 不滤波
 	TIM5->CCMR1|=0<<10; 	//IC2PS=00 	配置输入分频,不分频 

	TIM5->CCER|=0<<1; 		//CC1P=0	上升沿捕获
	TIM5->CCER|=1<<0; 		//CC1E=1 	允许捕获计数器的值到捕获寄存器中

	TIM5->DIER|=1<<1;   	//允许捕获中断				
	TIM5->DIER|=1<<0;   	//允许更新中断	
	TIM5->CR1|=0x01;    	//使能定时器2
}

void TIM5_IRQHandler(void)
{ 		    
	if (TIM_GetITStatus(TIM5, TIM_IT_Update) != RESET)
	{	    
		if (TIM_GetITStatus(TIM5, TIM_IT_CC1) != RESET)
		{	
			printf("捕获到一次上升沿输入！\r\n");
			LED0=!LED0;
		}			     	    					   
	TIM_ClearITPendingBit(TIM5, TIM_IT_CC1|TIM_IT_Update); //清除中断标志位	    
	}
}

```

# 二、imut.h文件

```c
#ifndef __IMUT_H__
#define __IMUT_H__

#include "stm32f10x.h"
#include "sys.h"
#include "delay.h"
#include "usart.h"	

#define KEY0 		PEin(4)
#define LED0 		PBout(5)
#define LED1 		PEout(5)
#define KEY0 		PEin(4)
#define KEY1 		PEin(3)
#define WakeUP  PAin(0)
#define BEEP    PBout(8)



void SysInit(void);
void GPIOInit(GPIO_TypeDef * GPIOx,u8 Pin,int OutOrIN);
u8 KEY_Scan(u8 mode);
void NVICPriorityGroupConfig(u8 NVIC_Group);
void NVICInit(u8 NVIC_PreemptionPriority,u8 NVIC_SubPriority,u8 NVIC_Channel,u8 NVIC_Group);
void USART2_Init(int BaudRate);
void ExtiInit(GPIO_TypeDef * GPIOx,u8 BITx,u8 UpOrDown);
void LEDInit(u8 LED);
void KeyInit(u8 KEY);
void USARTInit(u8 usart,int paudrate);
void IWDG_Init(u8 prer,u16 rlr);
void TIMInit(TIM_TypeDef* TIMx,u16 uptime);
void PWMInit(TIM_TypeDef* TIMx,u8 CHx,u16 arr);
void CapInit(u16 arr,u16 psc);

#endif

```