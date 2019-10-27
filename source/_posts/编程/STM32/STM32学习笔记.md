
---
title: STM32学习笔记
author: 陈德强
date: 2019-04-08 08:28:00
categories: 笔记
tags: mcu
toc: true
top: false
img: /medias/paperimg/mcu.jpg
summary: STM32学习笔记
---

# 一、ARM，ST，Keil的区别
ARM公司是做芯片架构设计的；
ST公司是做芯片的；
Keil是针对ARM架构做的IDE（集成开发环境）
所以，任何一个做 Cortex-M3 芯片，他们的内核结构都是一样的，不同的是他们的存储器容量，片上外设，IO 以及其他模块的区别。
![image](https://wx1.sinaimg.cn/large/006jQClrly1g1v7m6j2aqj30pv0howi5.jpg)


# 二、为什么要用typedef定义结构体呢？
例如结构体：
```c
typedef struct
{
  __IO uint32_t CRL;
  __IO uint32_t CRH;
  __IO uint32_t IDR;
  __IO uint32_t ODR;
  __IO uint32_t BSRR;
  __IO uint32_t BRR;
  __IO uint32_t LCKR;
} GPIO_TypeDef;
```
这个和下边是一样的:
```c
struct GPIO_TypeDef
{
  __IO uint32_t CRL;
  __IO uint32_t CRH;
  __IO uint32_t IDR;
  __IO uint32_t ODR;
  __IO uint32_t BSRR;
  __IO uint32_t BRR;
  __IO uint32_t LCKR;
} ;
```
但为什么要用typedef呢？
就是为了定义变量时少用一个strct。
我举个栗子，
第一种方式定义结构体变量：
```c
GPIO_TypeDef A
```

第二种方式定义结构体变量：
```c
struct GPIO_TypeDef A
```
虽然仅仅少了一个sturct，但是众所周知，代码量决定代码速度，当结构体被大量应用时效果就会体现出来了。

# 三、推挽输出
低电平导通，高电平不导通。

上拉输入和下拉输入：
上拉输入就是没信号输入时，是高电平；下拉输入就是没信号输入时，是低电平，相当于自动复位吧。
浮空输入就是什么都不接，据说容受干扰。


# 四、开启代码补全功能
![image](https://wx3.sinaimg.cn/large/006jQClrly1g1vc6m0oz0j30lq0ie0ui.jpg)

# 五、注意事项
5.1 编写文件时最后一行加上空行，否则有警报；

# 六、添加.c文件和.h文件
## 6.1 添加.c文件
新建并编写完c文件后一定要加到项目管理选项里面才能编译：
![1](https://wx3.sinaimg.cn/large/006jQClrly1g1vg0c4dwpj31hc0u01aa.jpg)
![2](https://wx2.sinaimg.cn/large/006jQClrly1g1vg24vqxsj30lq0gbgmp.jpg)

## 6.2 添加.h文件
如果.h文件不再user文件夹中，而是在自建文件夹中，则需要手动添加头文件路径，否则找不到：
![image](https://wx2.sinaimg.cn/large/006jQClrly1g1vg4pjl8ej30u30i9aex.jpg)

# 七、串口下载

先安装CH340驱动，

下载线一定要用专用的下载线，就是中间带转换芯片，而不是普通的usb线，
![image](https://wx4.sinaimg.cn/large/006jQClrly1g1vjjwqrszj33k01s01l7.jpg)

再然后保证硬件设置正确
![image](https://wx4.sinaimg.cn/large/006jQClrly1g1vjezu3ofj30rr0gvh7o.jpg)

再然后保证下载工具的设置正确
![image](https://ws3.sinaimg.cn/large/006jQClrly1g1vjl2p0y7j30f50bcq5d.jpg)

# 八、或运算
或运算可以设置某一位而不影响其他位。例如：
```
RCC->APB2ENR|=1<<3;
```
上述代码代表将第3位设置为1.
& 0运算清零
| 1运算置1

一个字节4位

定义要放在函数前边。

# 九、串口通信
 STM32写中断处理函数时，必须使用上面固定的函数名；
 
 想判断字符，可以用宏定义。

# 十、定时器
 定时器是内部装置，不占用引脚，但定时器通道可以映射到引脚。PWM则通过定时器通道来配置。

# 十一、PWM
PWM作用是调节占空比，换句话说就是调节功率。通过设置有效时间和无效时间的比例，达到按百分比输出的目的。
定时器中断和PWM可以共用同一个定时器，因为定时器中断不占用引脚，所以和PWM不冲突。但配置后定时器的更新时间就不要再更改了，否则PWM的占空比就变了。
实验中重映射将定时器3的通道2重映射到PB5,只是因为LED0在PB5上，所以PWM输出到PB5上才能看到灯的效果，实际应用中不需要重映射。

# 十二、DMA
DMA，全称为：Direct Memory Access，即直接存储器访问，DMA传输将数据从一个地址空间复制到另外一个地址空间。
简单来说，使用DMA传输速度更快。
使用DMA需要从外设（TIMx、ADC、SPIx、I2Cx 和USARTx）产生DMA请求，这样数据传输就不需要从外设-CPU-内存传输，而是通过DMA通道直接从外设-内存传输。
![DMA通道表](https://ws1.sinaimg.cn/large/006jQClrly1g1zt3ffk49j30s40b4n0b.jpg)

# 十三、通信
通信就是两个芯片之间的信息交互。总体原则是初始化，发送函数以及接收函数三部分。
通信分为有线通信和无线通信，常用的通信方式有：
IIC、SPI、485、CAN、红外、蓝牙等。
## 模拟IIC和硬件IIC区别
其实程序是一模一样的，唯一的区别是模拟IIC需要CPU运算，这样就增加了单片机的运算时间，而带IIC接口的单片机，程序还是需要的，但是IIC的运算通过集成在单片机里面的寄存器硬件电路来运算，就像定时器电路一样自己会运算，这样就不要cpu来运算过程了，从而节省了时间，使cpu运算的更快。当然这样就的多付出经济成本哦。
硬件IIC有专门的寄存器，只要你把相关的控制寄存器设置好，比如你要发送数据，就只要往相关的数据寄存器写一个数就可以了。
使用模拟IIC可以使用任意引脚，使用内置IIC需要固定引脚。
常用IIC接口通用器件的器件地址是由种类型号，及寻址码组成的，共7位。
如格式如下：
D7 D6 D5 D4 D3 D2 D1 D0
1-器件类型由：D7-D4 共4位决定的。这是由半导公司生产时就已固定此类型的了，也就是说这4位已是固定的。
2-用户自定义地址码：D3-D1共3位。这是由用户自己设置的，通常的作法如EEPROM这些器件是由外部IC的3个引脚所组合电平决定的（用常用的名字如A0,A1,A2）。这也就是寻址码。换言之，接法不同，地址不同。
所以为什么同一IIC总线上同一型号的IC只能最多共挂8片同种类芯片的原因了。
3-最低一位就是R/W位。这位不用我多说了。
[详解参考此文](https://blog.csdn.net/ferrycooper/article/details/51701567)

# 十四、外部中断和输入捕获的区别

1，定时器配置比中断复杂。 
2，一个中断占用一个定时器，也是很浪费的。 
3，一个定时器一般只有一个中断服务函数，而定时器有4路输入。 
所以，楼主可以用定时器去实现中断的功能，只是有点大材小用。

嗯嗯，我看懂了，最近一直在思考这个问题，他们有什么区别呢？原来实现中断的方式不一样，输入捕获是利用的在定时器溢出周期内产生中断（定时器性质），比外部中断多了计时和滤波功能，但需要占用一个定时器（这个资源很宝贵，一个通道占用了这个定时器其余通道都不能用这个了）除了服务函数而且要配置定时器，配置比较麻烦。而外部中断虽然只有中断功能，但是思路直接，配置简单，每一个IO都能作为中断源。

参考链接：[http://www.openedv.com/posts/list/45117.htm](http://www.openedv.com/posts/list/45117.htm)





# EDN、杂记
不要在.h文件中定义变量，否则会出现过定义情况。

EEPROM用于存储数据，到点不丢失，相当于电脑硬盘。
FLASH，闪存，也是存储数据的。

\r\n是换行
有时按键扫描函数需要有一个缓存函数来传递值，不能直接放在if里判断。所以，写的时候先写全，后期再简化。

