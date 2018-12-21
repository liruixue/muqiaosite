---
title: 'Arduino制作的纸钢琴'
date: 2017-10-25 18:33:27
categories:
- 创客吧
- 创客项目
- 他山之石
tags:
- Arduino
- 外译
---

# 关于该项目
这是一个简单的项目使用Arduino画键盘使用铅笔、纸、声器为元件，可组合成供演奏的乐器.

<!-- more --> 

# 元件准备
| 图片参考        | 项目   |  数量  |  购买渠道  |
| --------   | -----  | ----  | ----  |
|![](/images/elec-comp/jump-wires.jpg)| 公母跳线 |   1    | [Dfrobot][jump-wires]  |
|![](/images/elec-comp/breaboard-generic.jpg)|   面包板(通用型)  |   1   | Dfrobot   |
|![](/images/elec-comp/ArdGen_Mega.jpg)|    ARDUINO MEGA 2560 REV3    |  1  | Dfrobot   |
|![](/images/elec-comp/MFR-25FRF52-1M_sml.jpg)|    电阻器 1M ohm    |  1  | Dfrobot   |
|![](/images/elec-comp/speaker.jpg)|    扬声器: 0.25W, 8 ohms   |  1  | Dfrobot   |
|其他|    A4纸、铅笔、纸夹（曲别针）  |  若干  | --   |

#  项目原理
------
##  电容式感应
电容式触摸感应是一种人类接触传感激活的方式,需要很少的力气就能被感应到。它可应用于超过四分之一英寸的塑料、木材、陶瓷或其他绝缘材料(不是任何一种金属)内,并使传感器完全隐藏掉。
##  为什么用电容触控?
> * 每个触控传感器只需要一个导线连接到它
> * 可以在任何非金属材质隐藏
> * 可以很容易地使用一个按钮
> * 如果需要的话，可以检测到从几英寸的手动作
> * 非常便宜

##  电容式传感器是如何工作的呢?
传感器板和你的身体形成一个电容器。我们知道,电容器储存电荷。电容越能储存电荷，这个电容式触控传感器的电容量取决于你的手如何去触摸板。

##  Arduino起什么作用?

基本来说，Arduino测量电容器(触感器)需要充多长时间电,然后给电容一个估计值。电容可能很小,然而却Arduino可以精准地测量它。

在项目中使用电容式触摸的方法之一是使用CapSense库，对于CapSense库，arduino使用一个发送针和任意所需数量的接收针。接收针通过一个中到高值的电阻器来连接到发送针。

为达到实验所需的响应，下面是一些对于电阻使用的指引
> * 使用1兆欧电阻器(或者更少)使得触碰就能激活
> * 带有10兆欧电阻传感器将对4-6英寸距离有反应
------

#  电路图
![](/images/elec-map/paper_piano_2_PkxEgIChqo.jpg)

# 代码

```python
// Import the CapacitiveSensor Library.
#include <CapacitiveSensor.h>


 
#define speaker 11


// Set the Send Pin & Receive Pin.
CapacitiveSensor   cs_2_3 = CapacitiveSensor(2,3);        
CapacitiveSensor   cs_2_4 = CapacitiveSensor(2,4);         
CapacitiveSensor   cs_2_5 = CapacitiveSensor(2,5);     
CapacitiveSensor   cs_2_6 = CapacitiveSensor(2,6);     
CapacitiveSensor   cs_2_7 = CapacitiveSensor(2,7);      
CapacitiveSensor   cs_2_8 = CapacitiveSensor(2,8);         
CapacitiveSensor   cs_2_9 = CapacitiveSensor(2,9);  
CapacitiveSensor   cs_2_10 = CapacitiveSensor(2,10);     


void setup()                    
{
  cs_2_6.set_CS_AutocaL_Millis(0xFFFFFFFF);     // turn off autocalibrate on channel 1 - just as an example
  
  // Arduino start communicate with computer.
  Serial.begin(9600);
}

void loop()                    
{
  // Set a timer.
  long start = millis();
  
  // Set the sensitivity of the sensors.
  long total1 =  cs_2_3.capacitiveSensor(3000);
  long total2 =  cs_2_4.capacitiveSensor(3000);
  long total3 =  cs_2_5.capacitiveSensor(3000);
  long total4 =  cs_2_6.capacitiveSensor(3000);
  long total5 =  cs_2_7.capacitiveSensor(3000);
  long total6 =  cs_2_8.capacitiveSensor(3000);
  long total7 =  cs_2_9.capacitiveSensor(3000);
  long total8 =  cs_2_10.capacitiveSensor(3000);
  


  Serial.print(millis() - start);        // check on performance in milliseconds
  Serial.print("\t");                    // tab character for debug windown spacing

  Serial.print(total1);                  // print sensor output 1
  Serial.print("\t");                    // Leave some space before print the next output
  Serial.print(total2);                  // print sensor output 2
  Serial.print("\t");                    // Leave some space before print the next output
  Serial.print(total3);                  // print sensor output 3
  Serial.print("\t");                    // Leave some space before print the next output
  Serial.print(total4);                  // print sensor output 4
  Serial.print("\t");                    // Leave some space before print the next output
  Serial.print(total5);                  // print sensor output 5
  Serial.print("\t");                    // Leave some space before print the next output
  Serial.print(total6);                  // print sensor output 6
  Serial.print("\t");                    // Leave some space before print the next output
  Serial.print(total7);                   // print sensor output 7
                                          // Leave some space before print the next output
  Serial.print("\t");
  Serial.println(total8);                 // print sensor output 8
                                         // "println" - "ln" represent as "line", system will jump to next line after print the output.
  
  
  
  
  // When hand is touched the sensor, the speaker will produce a tone.
  // I set a threshold for it, so that the sensor won't be too sensitive.
  if (total1 > 500) tone(speaker,131);   // frequency
  if (total2 > 500) tone(speaker,147);   // you can see https://www.arduino.cc/en/Tutorial/toneMelody if you want to change frequency
  if (total3 > 500) tone(speaker,165);
  if (total4 > 500) tone(speaker,175);
  if (total5 > 500) tone(speaker,196);
  if (total6 > 500) tone(speaker,220);
  if (total7 > 500) tone(speaker,247);
  if (total8 > 500) tone(speaker,262);
  
  // When hand didn't touch on it, no tone is produced.
  if (total1<=500  &  total2<=500  &  total3<=500 & total4<=500  &  total5<=500  &  total6<=500 &  total7<=500 &  total8<=500)
    noTone(speaker);

  delay(10);                             // arbitrary delay to limit data to serial port 
}
```



# 纸钢琴的视频演示
<iframe height=380 width=510 src='http://player.youku.com/embed/XMzg4NzUxODE0MA==' frameborder=0 'allowfullscreen'></iframe>

>**声明**
> * 本文是个人搜集Arduino典型有趣的一些项目进行翻译，其项目的整体步骤本人尚无完整的验证，如照做可能会产生一定的误差，如果需要原文链接可以留言把邮箱给我，我会私发链接给你供研习.
> * 文章中的购买链接推荐仅基于完成项目效率提供的参考，与相关卖家并无任何商业利益牵涉，本人不对其商品品质进行担保，请意图购买的朋友们自行判断决定其渠道.

作者 [@碧海饮冰人]    
2017 年 10月 25日    

