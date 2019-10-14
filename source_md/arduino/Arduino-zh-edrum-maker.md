---
title: 'Arduino入门项目-水果电子鼓'
date: 2019-10-11 22:44:27
categories:
- 创客吧
- 入门必查
- 入门项目
tags:
- Arduino
- 基础
- 入门项目
- MIDI BLE
---

# 关于该项目
和小朋友一起，为参加科技节而制作的一个简易水果电子鼓.
![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/Arduino/edrum/Arduino-zh-edrum-maker-home.jpg)
<center><font color=#c3c3c3>简单的几个水果和一块芯片板连接Iphone后就是一个电子鼓</font></center>
<!-- more --> 


# 元件准备
| 图片参考        | 项目   |  数量  | 
| --------   | -----  | ----  | 
|![]()| IOS设备(Iphone手机)|   1    | 
|![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/elec-comp/Arduino-nano-v3.jpg)| Arduino Genuino 101|   1   |
|![]()|    12孔电容触摸板    |  1  | 
|![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/elec-comp/clip.jpg)| 鳄鱼夹线   |  4根  | 
|![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/elec-comp/fruit.jpg)| 柠檬苹果若干  |  1  | 

#  项目原理
------
##  MIDI BLE
通过带有蓝牙特性的Arduino的开源芯片，将BLE-MIDI数据数据传输到ios设备上，通过ios设备内置乐器软件（比如：库乐队）接收MIDI数据，并最终模拟出电子鼓的声音。
![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/Arduino/edrum/Arduino-zh-edrum-maker-chip.jpg)


##  接线原理

Arduino控制芯片与电容触摸板相连接，电容触摸板的引线与水果相连，水果本来就是电流的良导体，人体触碰到水果，就相当于接触到了电容板，接触再放手就会形成电信号，电信号通过Arduino板的控制器传输给到相连接的IOS设备，IOS设备中乐器软件识别了MIDI序列的信号，然后即可依据这个MIDI数据发出乐器的声响。

#  电路图
触摸板与Arduino控制板孔位相连
![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/Arduino/edrum/Arduino-zh-edrum-maker-touchboard.jpg)

然后再用几条鳄鱼夹线引出触摸板中，想要导出的接线柱（这里导出0,1,3,4孔位）：
![](https://gitee.com/ruixue_li/muqiaosite/raw/master/images/Arduino/edrum/Arduino-connection.jpg)


# 效果演示
敲击鳄鱼夹上连接的水果，不同的水果即可发出不同的鼓声
<iframe height=498 width=510 src='http://player.youku.com/embed/XNDM5NDU2MzU0OA==' frameborder=0 'allowfullscreen'></iframe>



作者 [@碧海饮冰人]    
2019 年 10 月 11日    


