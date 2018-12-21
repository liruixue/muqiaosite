---
title: 'Arduino练习教程-入门版OTTO机器人自己做'
date: 2018-11-04 14:33:27
categories:
- 创客吧
- 创客项目
- OTTO DIY
tags:
- Arduino
- 进阶
- 入门项目
- 机器人
- OTTO
- 开源
- 教程
---

# OTTO机器人是什么
OTTO是完全开源的,任何人都可以做的交互式机器人. 她与Arduino控制系统兼容,其主要外观材料可以直接3d打印而来, 甚至可以说她是为培养孩子们学习机器人的热情而建造的。 OTTO的灵感来源于另一个称为[Zowi](https://github.com/bqlabs/zowi)的两足机器人。

<!-- more --> 

# 元件准备
| 图片参考        | 项目   |  数量  |  购买渠道  |
| --------   | -----  | ----  | ----  |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/Arduino-nano-v3.jpg)| Arduino or Genuino Nano V3.0 ATmega328|   1    | [Dfrobot](http://www.dfrobot.com.cn/goods-754.html)  |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/Arduino-nano-shield.jpg)| Arduino Nano扩展板 |   1   | [Dfrobot](http://www.dfrobot.com.cn/goods-64.html)   |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/Ultrasound-sensor.jpg)|    HC-SR04超声波传感器    |  1  | Dfrobot   |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/icon-components-placeholder.jpg)|  5v无源蜂鸣器    |  4  | [Dfrobot] |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/jump-wires-ff.jpg)| 母对母杜邦线   |  6  | [Dfrobot](http://www.dfrobot.com.cn/goods-366.html)   |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/icon-components-placeholder.jpg)|  5号AA电池  |  4  | [Dfrobot](http://www.dfrobot.com.cn/goods-791.html)   |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/icon-components-placeholder.jpg)|  两枚装5号电池的电池盒  |  2  | [Dfrobot](http://www.dfrobot.com.cn/goods-1618.html)   |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/3d-print.jpg)|  OTTO的外观3D打印一套   |  1 | [Dfrobot](http://www.dfrobot.com.cn/goods-791.html)   |

*OTTO外观打印机的说明:你可以从[这里](https://pan.baidu.com/s/1Ei_NzV5RR84f2g58HHAnkA)下载3d打印机的源文件自行打印，也可以找3d打印商进行打印机购买.如果自行打印，其打印设置要求：精度-0.15mm， 填充密度-20%*

#  组装
------
##  躯体部位鸵机的安装
拿来两个鸵机，按照图中所示，把他们安装到指定的位置，用螺丝刀固定好
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_body.jpg)


##  双腿安装到躯体上
先把双腿部件连接到身体上微型鸵机处，要确保腿部相对于身体能够向**左右两边各自转动90度**。在对齐腿部之后用螺丝刀通过腿内部的小孔固定好.此处需注意**将腿部顶端上的穿线孔与躯体下方的穿线孔靠近，使得稍后可以流畅穿线**
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_leg.jpg)

##  腿部鸵机的安装
把微型鸵机放到腿部，可以把鸵机线绕几圈，然后如图示把它推进内部,并将鸵机的引线从躯体穿线孔穿出 如果困难的话，也许需要用刀清洁一下相关的区域。像检查腿部的活动方向一样，你也看下鸵机至少可以**朝左右两边各旋转90度**。检查后要使用小螺丝来固定它，同样地,另一只脚也是同样的步骤
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_leg.jpg)
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_leg_wire.jpg)


##  双脚安装到腿部
按照图示的方式来安装，但要注意把电线放置到躯体内部的槽位并通过腿部的小孔穿出来。一旦确定好他们的位置，就可以用螺丝刀从背后固定好她们。
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_foot_leg.jpg)
##  头部的组装
将超声波传感器推到眼睛的极限处，将Arduino nano安装到扩展板之后，你也可以将电池底座的线缆正极焊接到板子的Vin接线柱上，负极接到GND上。将连接好的板子，对准3d打印的头部上的USB连接口，再用至少两颗螺丝将他们固定到头部上.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_head.jpg)
##  线路连接
准备好杜邦线及及蜂鸣器，根据图形所示的连接方法将各处引针连接完毕
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/otto_asssembly_elec_map.jpg)
>**注意**
在Nano板上传代码时，碰到以下avrdude: stk500_getsync() attempt 8 of 10: not in sync: resp=0x36错误，可以尝试在Arduino IDE上尝试以下操作以下菜单：
'工具- 处理器 - ATmega328P(Old Bootloader)'应该可以解决
##  合上头部
头部与身体有卡扣，小心地放置好线缆并合起来。

------
#  代码上传
其操作步骤如下:
1.下载并安装[Arduino IDE软件](https://www.arduino.cc/en/Main/Software)
2.下载这里的[lib](https://pan.baidu.com/s/1gj7cE0Vl0LKXRym5V2FmuA)库解压文件并复制到C:\Users\user\Documents\Arduino\libraries (或者是你安装的库文件夹所在之处):
3.通过USB线连接你的OTTO
4.打开并上传[OttoDIY_smooth_criminal.ino](https://pan.baidu.com/s/1Zgks7mkI_Nic-tRBU9D83w)代码到你的Arduino Nano中，之后就可以看到Otto跳舞了:)

# 效果演示
机器人的运动：
<iframe height=380 width=510 src='http://player.youku.com/embed/XMzkxMjAxNjg3Ng==' frameborder=0 'allowfullscreen'></iframe>
另外有一些关于Otto机器人的演示代码，可以[下载](https://pan.baidu.com/s/1vvB2-3j7fZcXSjvQpQmtMA)并体验机器人的避障、演奏及跳舞功能。

>**后续扩展**
OTTO DIY还有其他各项包括蓝牙控制及其他传感器的交互(可见Ottodiy.com)，后续我会加以拓展并推出其他功能供分享，敬请关注.

    声明-文章部分内容出自英文直译，并由作者本人的亲历及经验总结，欢迎转载并带出原文链接

作者 [@碧海饮冰人]    
2018 年 11 月 4日    


