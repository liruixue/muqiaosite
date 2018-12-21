---
title: 'Arduino库教程-伺服库的使用（Servo Sweep）'
date: 2017-11-05 15:37:27
categories:
- 创客吧
- 入门必查
- Arduino基础库
tags:
- Arduino
- 基础库
- 入门项目
---

#  关于Servo Sweep
> * 将一个RC伺服电机的轴扫过180度
> * 这个例子利用了Arduino伺服库

<!-- more --> 


#  硬件要求
> * Arduino or Genuino Board
> * 伺服电机
> * 连接线

#  电路图
伺服电机有三根线：电源、接地和信号。电源线通常是红色的，应该连接到Arduino或genuino板的5V引脚上。接地线通常为黑色或棕色，应连接到板上的地引脚。该信号引脚通常是黄色或橙色，应连接到主板上的引脚pin 9。
![servo 连接图](/images/sweep_bb.png "servo 连接图")

# 代码

```C++
/* Sweep
 by BARRAGAN <http://barraganstudio.com>
 This example code is in the public domain.

 modified 8 Nov 2013
 by Scott Fitzgerald
 http://www.arduino.cc/en/Tutorial/Sweep
*/

#include <Servo.h>

Servo myservo;  // create servo object to control a servo
// twelve servo objects can be created on most boards

int pos = 0;    // variable to store the servo position

void setup() {
  myservo.attach(9);  // attaches the servo on pin 9 to the servo object
}

void loop() {
  for (pos = 0; pos <= 180; pos += 1) { // goes from 0 degrees to 180 degrees
    // in steps of 1 degree
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
  for (pos = 180; pos >= 0; pos -= 1) { // goes from 180 degrees to 0 degrees
    myservo.write(pos);              // tell servo to go to position in variable 'pos'
    delay(15);                       // waits 15ms for the servo to reach the position
  }
}
```

>**后续扩展**
想了解更多关于Servo library的信息，请点击这里到[官网](https://www.arduino.cc/en/Reference/Servo)

作者 [@碧海饮冰人]    
2017 年 11 月 5日    


