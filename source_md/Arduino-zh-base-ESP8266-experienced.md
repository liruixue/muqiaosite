---
title: '一文填千坑--Arduino Nano(atmega328p)对ESP8266的烧写'
date: 2018-12-16 21:49:43
categories:
- 创客吧
- 入门必查
- 入门项目
tags:
- Arduino
- 基础
- 入门项目
- ESP8266模块
---

现在物联网平台很多，类似Yeelink，乐为物联，Bylnk（为microduino量身打造，更易上手），借助它们提供的APP和接口可以快速地实现在手机端接收远程硬件信息。但别人的框架总是固定的，接口也是有限的，在样式和功能上有一定局限性，没法做到完全满足需求的定制.
想搭建自己的独立平台，实现远程控制硬件，类似智能家居的设计，来，看这篇基于Arduino的ESP8266模块的玩法：

![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/connection-result.jpg)
<!-- more --> 

# 关于该项目
ESP8266无线收发模块，可串口远距离传输，可用于扩展Arduino的无线连接能力，本文主要是展示如何使得Arduino 的Nano-atmega32p芯片与Esp8266进行烧写、通讯、调试的过程，以及中间过程所遇到的各种问题。
ESP8266 WIFI模块除了具备以下能力外，一个很重要的原因是稳定且价格低廉，价格低廉，价格低廉，重要的事情说三遍!
> * ESP8266 拥有强大的片上处理和存储能力，内置32位处理器，内置Lwip协议栈。支持AP+STA模式共存，可通过AT指令配置各种参数
> * Wi-Fi Direct (P2P)、soft-AP, 支持 WPA WPA2/WPA2–PSK加密, 无线标准:IEEE802.11b/g/n, 频率: 2.4 GHz


# 元件准备
| 图片参考        | 项目   |  数量  |  购买渠道  |
| --------   | -----  | ----  | ----  |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/esp8266-1-wifi.jpg)| ESP8266 WiFi模块|   1   | 国内某宝网站|
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/Arduino-nano-atmega328p.jpg)|    Arduino nano atmega328p    |  1  | 国内某宝网站  |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/NANO-UNO-extend-board.jpg)|  Arduino NANO UNO的扩展板   |  1  |国内某宝网站 |
|![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/elec-comp/jump-wires.jpg)| 杜邦线（母） |   若干  | 国内某宝网站  |

#  项目步骤
------
## 配置ESP8266在Arduino的运行环境
使用1.6.4及以上版本的Arduino来安装安装ESP8266开发板软件包， 打开Arduino IDE，打开'文件->首选项'在 附加开发板管理器网址一栏填入：
http://arduino.esp8266.com/stable/package_esp8266com_index.json
添加完以后点击‘好’。

做完这步以后重启Arduino IDE，然后依次点击'工具->开发板->开发板管理器'打开后在搜索框输入esp，然后能找到类似'esp8266 by ESP8266 Community'，选择较近的一个稳定版本（我用的是2.4.2版本），点击并安装。
**注意**
> * 搜索到的结果，在安装时，拜我们伟大的墙所赐，也有可能会下载失败，可以[参考这里的方法](https://blog.csdn.net/chenchen2360060/article/details/84838788)去手动安装，如果手动从github下载仍然受阻，你可以尝试[从这里下载这个文件](https://download.csdn.net/download/mini_snow/10852214)
安装完后重启Arduino IDE，然后依次点击 工具->开发板->Generic ESP8266 Module，接着按照下面的信息在菜单栏找到对应项进行配置
Flash Mode: QIO
Flash Frequency: 40 MHz
CPU Frequency: 80 MHz
Flash Size: 1M (512K SPIFFS)
Upload Speed: 115200
Port: 对应的USB 端口 (当你一将Arduino连接电脑时，在设备管理器中会冒出端口号）
Programmer: AVRISP mkll
其他的设置就按照默认的选择不变即可
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/esp-pakage-param-ref.jpg)

## 实物连线
ESP8266各引脚的标注内容如下：
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/esp8266-wire-api-desc.jpg)
ESP8266与Arduino Nano 扩展板的引脚对应关系如下：
ESP8266:_______________________ Arduino:
GND -------------------------- GND
GP2 -------------------------- Not connected (open)
GP0 -------------------------- GND
RXD -------------------------- RX
TXD -------------------------- TX
CHPD ------------------------ 3.3V
RST -------------------------- Not connected (open)
VCC -------------------------- 3.3V

**关于ESP8266引脚相连的一些说明**
> * GP0引脚与GND相连，在代码烧写的时候是必须处于连接状态的，烧写完成之后，代码运行时，可以撤掉这根线
> * RST引脚可以不连任何线，但在烧写开始前，请记得备上一根母的杜邦线将RST引脚与Arduino Nano扩展板上的GND通电再撤掉，目的是为了重置低电平（重置过程中可以看到ESP8266板上自带的蓝灯闪烁一下）
> * 有的文章写需要在GPO或CHPD引脚加装1千欧的电阻云云，我没去尝试，个人认为是锦上添花的事儿，与烧写能否成功并无关系，不用照做，最后也能烧写完成
> * ESP8266模块是由低电压（3.3V DC）供电的，其VCC和CH_PD要连接到Arduino的3.3V引脚上


## 编写Arduino的代码
此段代码是我为了调试ESP8266板与远程服务器连通情况而改写，你可以照搬走(但是需要使用Java/NodeJS等后台语言来搭建下例代码url中提供的类似服务)，然后将SSID及PASSWORD等信息改成你自己的，如果你想更简单的例程，你也可以发现Arduino的菜单‘文件-示例-Generic ESP8266 moudle的例子‘里有大把可以学习的例子，调出来一个你想要验证的功能即可：
```C++
/*
    This sketch sends data via HTTP GET requests to data.sparkfun.com service.

    You need to get streamId and privateKey at data.sparkfun.com and paste them
    below. Or just customize this script to talk to other HTTP servers.

*/

#include <ESP8266WiFi.h>

const char* ssid     = "roy-iphone"; //修改成你可访问的wifi名称
const char* password = "kkkk321*";  // 修改成wifi密码

const char* host = "123.252.255.226";
//const char* streamId   = "....................";
//const char* privateKey = "....................";

void setup() {
  Serial.begin(115200);
  delay(10);

  // We start by connecting to a WiFi network

  Serial.println();
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  /* Explicitly set the ESP8266 to be a WiFi-client, otherwise, it by default,
     would try to act as both a client and an access-point and could cause
     network-issues with your other WiFi-devices on your WiFi-network. */
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

int value = 0;

void loop() {
  delay(5000);
  ++value;

  Serial.print("connecting to ");
  Serial.println(host);

  // Use WiFiClient class to create TCP connections, port is customization.
  WiFiClient client;
  const int httpPort = 4000; //可以定制
  if (!client.connect(host, httpPort)) {
    Serial.println("connection failed");
    return;
  }

  // We now create a URI for the request
  String url = "/light-status/";
  //url += streamId;
  //url += "?private_key=";
  //url += privateKey;
  //url += "&value=";
  //url += value;

  Serial.print("Requesting URL: ");
  Serial.println(url);

  // This will send the request to the server
  client.print(String("GET ") + url + " HTTP/1.1\r\n" +
               "Host: " + host + "\r\n" +
               "Connection: close\r\n\r\n");
  unsigned long timeout = millis();
  while (client.available() == 0) {
    if (millis() - timeout > 5000) {
      Serial.println(">>> Client Timeout !");
      client.stop();
      return;
    }
  }

  // Read all the lines of the reply from server and print them to Serial
  while (client.available()) {
    String line = client.readStringUntil('\r');
    Serial.print(line);
  }

  Serial.println();
  Serial.println("closing connection");
}
```

## 烧写代码
激动了，终于来到烧写这一步了，决定你的付出能否被esp8266板子接受的关键一步，Arduino IDE中选好对应的串口，编译无误后，点击上传？额，别着急，先把‘文件-首选项-显示详细输出’的‘编译’‘上传’复选框选中，然后再烧写，如果一次能烧写成功，恭喜你了，如果失败，请对号入座看下面的一些问题及解决方案是不是能够帮到你？
1. espcomm_send_command: can't receive slip payload data
解决：这应该是没有连GP0到GND时的错误
2. GP0到GND连接后还出现下面错误：
warning: espcomm_sync failed
error: espcomm_open failed
error: espcomm_upload_mem failed
解决方案---这个错误成因要逐个排查，请确认你已进行检查：
> * RX与TX连线没接错，RX对RX，TX对TX
> * 尝试esptool.py工具,[点击参考文章这里](https://arduino.stackexchange.com/questions/20219/upload-with-esptool-fails-with-espcomm-send-command-cant-receive-slip-payload)查看如何使用，不过参考文章说的是在linux机器上跑，在windows机器上用，需要安装python，执行setup.py。esptool.py的使用，有两点要注意下：
一是要找到esptool.py.exe这个命令被安装的位置，我的机器是在D:\Program Files\python\Scripts目录之下
二是platform.txt这个文件的位置因系统而异，我的win10系统的位置就是C:\Users\RoyLi\AppData\Local\Arduino15\packages\esp8266\hardware\esp8266\2.4.2这个目录下面，这两点信息能确认了，那个esptool.py的工具也就能顺利运行了
> * 如果接线也无问题，且esptool.py也能运行，但仍然没有排除以上问题，请尝试我前文提到的将RST引脚重置低电平，然后将Arduino板的USB电源线拔出再重新接入，之后重新尝试上传代码

经过这一系列折腾，如果看到下面的这一串提示，那么恭喜你，你的代码已经烧写成功了。
```C++
esptool.py v2.5.0
Serial port COM3
Connecting.....
Detecting chip type... ESP8266
Chip is ESP8266EX
Features: WiFi
MAC: cc:50:e3:46:20:ec
Uploading stub...
Running stub...
Stub running...
Configuring flash size...
Auto-detected Flash size: 1MB
Compressed 268656 bytes to 193713...
Writing at 0x00000000... (8 %)
Writing at 0x00004000... (16 %)
Writing at 0x00008000... (25 %)
Writing at 0x0000c000... (33 %)
Writing at 0x00010000... (41 %)
Writing at 0x00014000... (50 %)
Writing at 0x00018000... (58 %)
Writing at 0x0001c000... (66 %)
Writing at 0x00020000... (75 %)
Writing at 0x00024000... (83 %)
Writing at 0x00028000... (91 %)
Writing at 0x0002c000... (100 %)
Wrote 268656 bytes (193713 compressed) at 0x00000000 in 17.1 seconds (effective 125.7 kbit/s)...
Hash of data verified.

Leaving...
Hard resetting via RTS pin...
```

------

## 运行代码及调试效果
烧写完的代码并不是马上会被ESP8266执行，仍然需要下面这个动作才会生效。
拔除GP0与GND的连线，RST再接一下GND线然后拔除，这时，通过打开IDE中的‘工具-串口监视器’，然后就可以观察串口打印出来的日志，表明你已经连接到wifi网络中，开始在互联网的天空中自由翱翔吧，小飞龙...
![TEL0092_control_LED13](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/Arduino/esp8266-experienced/arduino-debug.jpg)



作者 [@碧海饮冰人]    
2017 年 12 月 16日    


