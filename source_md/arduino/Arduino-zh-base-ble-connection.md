---
title: 'Arduino入门项目-ble蓝牙4.0的连接'
date: 2019-05-29 10:44:27
categories:
- 创客吧
- 入门必查
- 入门项目
tags:
- Arduino
- 基础
- 入门项目
- 蓝牙4.0
- ble
---

# 关于该项目
Arduino的低成本低功率蓝牙通讯方案测试:手机与蓝牙芯片连接，手机发出信息，并能接收到蓝牙模块的返回信息。
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/ble-connection/ble-connction-home.jpg)
<center><font color=#c3c3c3>华侨城创意文化园内的入口处，一辆攀满彩带和led的车，晚间的灯光轮廓会更清晰</font></center>


<!-- more --> 

# 元件准备
| 图片参考        | 项目   |  数量  |  购买渠道  |
| --------   | -----  | ----  | ----  |
|![]()| IOS设备(Iphone手机)|   1    | [Dfrobot](http://www.dfrobot.com.cn/goods-82.html)  |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/Arduino-nano-v3.jpg)| Arduino or Genuino Nano V3.0 ATmega328|   1   | [Dfrobot](http://www.dfrobot.com.cn/goods-286.html)   |
|![]()|    ble4.0模块    |  1  | 汇承HC-08蓝牙4.0模块   |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/jump-wires-ff.jpg)| 母对母杜邦线   |  6  | [Dfrobot](http://www.dfrobot.com.cn/goods-366.html)   |

#  项目原理
------
##  BLE 4.0
BLE 全称是 Bluetooth Low Energy 低功耗蓝牙的缩写，是蓝牙 4.0 的一个分支。BLE 支持 iPhone4s 以上的 iOS 设备，并且无需做 MFI 认证，因此对于个人开发者及一些中小型开发团队快速开发硬件原型以及智能家居等产品设备有着极大的便利。
本次使用的是蓝牙芯片为汇承HC-08蓝牙4.0模块。

##  Arduino起什么作用?
Arduino就是控制设备，负责接收蓝牙请求并转给串口，再将串口的信息通过蓝牙模块发送给IOS设备。
------

#  电路图
> * Arduino 5V  → BLE4.0  VCC
> * Arduino GND → BLE4.0  GND
> * Arduino TX  → BLE4.0  RXD
> * Arduino RX  → BLE4.0  TXD
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/ble-connection/arduino-connection.jpg)

# 代码

```python
   //项目名—— 蓝牙4.0的连接测试
void setup() {
   Serial.begin(9600);

}

void loop() {
    while(Serial.available()){
        char c = Serial.read();
        if (c == 'A') {
            Serial.println("BLE get char A.");
        }
    }
}
```
# IOS的LightBlue应用来连接Arduino与BLE设备
App Store中安装LightBlue Explorer应用，打开 LightBlue App，打开蓝牙，我们可以看到周围的蓝牙设备，我们这里的 BLE4.0 设备名称是 HC-08
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/ble-connection/ble-list.jpg)
点击 HC-08 ，可以看到左上方显示 Connected 字样，代表连接成功。接下来，我们下拉到最下面，点击 TX&RX 进入 TX&RX 子页。
注意页面的最右上方，这里现在是显示 Hex，我们需要的是使用 UTF-8 编码方式进行通讯，因此，我们点击 Hex，进入设置页面，选择 UTF-8 String 设置编码方式为 UTF-8 编码。
设置完成后，我们就可以通过 WRITTEN VALUES 来输入发送数据，然后可以在手机屏幕的 READ/NOTIFIED VALUES 列表中看到 与BLE板连接的Arduino代码 返回来的数据。
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/ble-connection/test-result.jpg)


# 效果演示
IOS设备通过上面步骤的WRITTEN VALUES 发送大写字母A，则Arduino设备接收蓝牙数据，如果截获数据为A，那么就返回 BLE get char A，。

>**声明**
> * 文章中的购买链接推荐仅基于完成项目效率提供的参考，与相关卖家并无任何商业利益牵涉，本人不对其商品品质进行担保，请意图购买的朋友们自行判断决定其渠道.

作者 [@碧海饮冰人]    
2019 年 5 月 29日    


