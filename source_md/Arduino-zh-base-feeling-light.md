---
title: 'Arduino入门项目1-点亮感应灯'
date: 2017-11-01 15:33:27
categories:
- 创客吧
- 入门必查
- 入门项目
tags:
- Arduino
- 基础
- 入门项目
---

# 关于该项目
感应灯，当有人经过的时候，LED灯就会自动亮起，人一旦走了，LED又自动关闭了。

<!-- more --> 

# 元件准备
| 图片参考        | 项目   |  数量  |  购买渠道  |
| --------   | -----  | ----  | ----  |
|![](/images/elec-comp/led-fish.jpg)| 数字食人鱼红色LED发光模块|   1    | [Dfrobot](http://www.dfrobot.com.cn/goods-82.html)  |
|![](/images/elec-comp/body-feeling.jpg)| 人体红外热释电运动传感器 |   1   | [Dfrobot](http://www.dfrobot.com.cn/goods-286.html)   |
|![](/images/elec-comp/ArdGen_Mega.jpg)|    ARDUINO MEGA 2560 REV3    |  1  | Dfrobot   |
|![](/images/elec-comp/extend-ardunio.jpg)|  IO 传感器扩展板 V7.1    |  1  | [Dfrobot](http://www.dfrobot.com.cn/goods-791.html)   |

#  项目原理
------
##  人体红外热释电运动传感器
红外热释电运动传感器能检测运动的人或动物身上发出的红外线，输出开关信号，可以应用于各种需要检测运动人体的场合.

##  Arduino起什么作用?
整个装置分为三个部分，输入，控制与输出。人体红外热释电运动传感器为输入设备， Arduino就是控制设备，LED发光模块就是输出设备。
又由于人体红外热释电运动传感器为数字量的传感器，所以接数字口。LED输出信号也是数字量，同样接数字口。
------

#  电路图
> * 人体红外热释电运动传感器 → 数字引脚2
> * 数字食人鱼红色LED发光模块 → 数字引脚13
![感应灯连接电路图](/images/elec-map/zh-base-feeling-light.jpg "感应灯连接电路图，来自DFRobot")

# 代码

**digitalRead(pin)**
> 这个函数是用来读取数字引脚状态，HIGH还是LOW。人体红外热释电传感器有人或者动物走动时，读到HIGH，否则读到LOW。代码的后半段就是对判断出来的值来执行相应动作。（HIGH代表1，LOW代表0）

```python
   //项目名—— 感应灯
int sensorPin =2;             //传感器连接到数字2
int ledPin =  13;              //LED连接到数字13
int sensorState =0;           //变量sensorState用于存储传感器状态
void setup() {
  pinMode(ledPin, OUTPUT);         //LED为输出设备
  pinMode(sensorPin, INPUT);      //传感器为输入设备
}
void loop(){
  sensorState = digitalRead(sensorPin);    //读取传感器的值
  
  if (sensorState == HIGH) {       //如果为高，LED亮
    digitalWrite(ledPin, HIGH);  
  }
  else {                               //否则，LED灭
    digitalWrite(ledPin, LOW);
  }
}
```



# 效果演示
下载完成后，可以试着人走开，等待一段时间，看看LED是否会关掉。随后再试着靠近，LED是不是会自动亮起。

>**声明**
> * 文章中的购买链接推荐仅基于完成项目效率提供的参考，与相关卖家并无任何商业利益牵涉，本人不对其商品品质进行担保，请意图购买的朋友们自行判断决定其渠道.

作者 [@碧海饮冰人]    
2017 年 11 月 1日    


