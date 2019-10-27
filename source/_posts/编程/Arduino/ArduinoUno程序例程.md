---
title: ArduinoUno程序例程
date: 2019-05-26  17:58:00
categories:
- 笔记
tags:
- mcu
toc: true
summary: arduino例程。
img: /medias/paperimg/arduino.jpg
---
# 一、双色LED实验
```c
/***********************
R     11
GND   GND
G     10
***********************/
int redPin=11;
int greenPin=10;
int val=0;

void setup()
{
	pinMode(redPin,OUTPUT);
	pinMode(greenPin,OUTPUT);
}

void loop()
{
	digitalWrite(redPin, LOW);  //设置管脚1为LOW
	digitalWrite(greenPin, LOW);  //设置管脚3为LOW
	delay(500); //延时500毫秒

	digitalWrite(redPin, HIGH);  
	digitalWrite(greenPin, HIGH);  
	delay(500); 
}
```

# 二、继电器实验
```c

const int relayPin=7;

void setup()
{
	pinMode(relayPin,OUTPUT);
}
void loop()
{
	digitalWrite(relayPin, HIGH);  
	delay(500); 

	digitalWrite(relayPin, LOW);  
	delay(500); 
}
```

# 三、激光传感器实验
```c
const int relayPin=7;

void setup()
{
	pinMode(relayPin,OUTPUT);
}
void loop()
{
	digitalWrite(relayPin, HIGH);  //发射激光
	delay(500); 

	digitalWrite(relayPin, LOW);  //关闭激光
	delay(500); 
}
```
# 四、轻触开关实验
```c
const int keyPin=7;

void setup()
{
	pinMode(keyPin,OUTPUT);
}
void loop()
{
	digitalWrite(keyPin, HIGH);  //闭合开关
	delay(500); 

	digitalWrite(keyPin, LOW);  //断开开关
	delay(500); 
}
```
# 五、倾斜开关
当倾斜一定角度时，开关通电。
```c
const int sigPin=7;
const int ledPin=13;//led引脚
boolean sigState=0;

void setup()
{
	pinMode(sigPin,INPUT);
	pinMode(ledPin,OUTPUT);
	Serial.begin(9600);
}
void loop()
{
	sigState=digitalRead(sigPin);
	Serial.println(sigState);
	if(sigState==HIGH)
	{
		digitalWrite(ledPin, LOW); 
	}
	else
	{
		digitalWrite(ledPin, HIGH); 
	}
}
```
# 六、震动开关传感器
晃动传感器led灯会亮。
```c
const int vibswPin = 8; //the Vibration Switch attach to 
const int ledPin = 13; //the led attach to
int val = 0; //initialize the variable val as 0
/******************************************/
void setup()
{
  pinMode(vibswPin,INPUT); //initialize vibration switch as an input
  pinMode(ledPin,OUTPUT); //initialize ledPin switch as an output
  Serial.begin(9600);
}
/*******************************************/
void loop()
{
  val = digitalRead(vibswPin); //read the value from vibration switch
  Serial.println(val);
  if(val == LOW)  //without vibration signal
  {
    digitalWrite(ledPin,HIGH); //turn on the led
    delay(500);//delay 500ms,The LED will be on for 500ms
  }
  else
  {
    digitalWrite(ledPin,LOW); //turn off the led
  }
}
```
# 七、红外遥控实验
使用红外遥控器发射信号，红外接收端接收信号。
```c
/*
 * IRrecvDemo
 * 红外控制，接收红外命令控制板载LED灯亮灭
 */

#include <IRremote.h>

int RECV_PIN = 11;
int LED_PIN = 13;

IRrecv irrecv(RECV_PIN);

decode_results results;

void setup()
{
  Serial.begin(9600);
  irrecv.enableIRIn(); // Start the receiver
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, HIGH);
}

void loop() {
  if (irrecv.decode(&results)) {
    Serial.println(results.value, HEX);
    if (results.value == 0xFFA25D) //开灯的值
    {
      digitalWrite(LED_PIN, LOW);
    } else if (results.value == 0xFF629D) //关灯的值
    {
      digitalWrite(LED_PIN, HIGH);
    }
    irrecv.resume(); // Receive the next value
  }
  delay(100);
}
```
# 八、干簧管传感器
干簧管传感器也是一种检测磁场的传感器。霍尔传感器通常用于测量智能车辆的速度并计算装配线，而干簧管传感器通常用于检测磁场的存在。
```c
const int digitalInPin = 8; //the Vibration Switch attach to 
const int ledPin = 13; //the led attach to
int val = 0; //initialize the variable val as 0
/******************************************/
void setup()
{
  pinMode(digitalInPin,INPUT); //initialize vibration switch as an input
  pinMode(ledPin,OUTPUT); //initialize ledPin switch as an output
  Serial.begin(9600);
}
/*******************************************/
void loop()
{
  val = digitalRead(digitalInPin); //read the value from vibration switch
  Serial.println(val);
  if(val == LOW)  //without vibration signal
  {
    digitalWrite(ledPin,HIGH); //turn on the led
    delay(500);//delay 500ms,The LED will be on for 500ms
  }
  else
  {
    digitalWrite(ledPin,LOW); //turn off the led
  }
}
```
# 九、光电传感器
光电传感器由发射器和接受器组成，当有物体通过时会被触发。
```c
const int photoPin = 8; //the Vibration Switch attach to 
const int ledPin = 13; //the led attach to
int val = 0; //initialize the variable val as 0
/******************************************/
void setup()
{
  pinMode(photoPin,INPUT); //initialize vibration switch as an input
  pinMode(ledPin,OUTPUT); //initialize ledPin switch as an output
  Serial.begin(9600);
}
/*******************************************/
void loop()
{
  val = digitalRead(photoPin); //read the value from vibration switch
  Serial.println(val);
  if(val == LOW)  //without vibration signal
  {
    digitalWrite(ledPin,HIGH); //turn on the led
    delay(500);//delay 500ms,The LED will be on for 500ms
  }
  else
  {
    digitalWrite(ledPin,LOW); //turn off the led
  }
}
```
