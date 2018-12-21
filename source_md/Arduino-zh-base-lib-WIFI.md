---
title: 'Arduino库教程-WIfi无线扩展块101'
date: 2017-11-05 16:16:27
categories:
- 创客吧
- 入门必查
- Arduino基础库
tags:
- Arduino
- 基础库
- 入门项目
---

#  关于本项目(WIfi无线扩展块101)
> * 在这个例子中，一个简单的Web服务器可以让你在网页上闪烁一个LED。这个例子将打印WiFi Shield 101 或者 MKR1000 开发板（一旦连上）的IP地址到Arduino软件（IDE）串口监视器。一旦你知道开发板的IP地址，你可以在一个Web浏览器里打开该地址来打开和关闭引脚9上的LED
> * 如果你的扩展块/板的IP地址是你的地址：
http://yourAddress/H 打开LED
http://yourAddress/L 关闭LED
> * 这个例子是为一个使用WPA加密的网络编写的。对于WEP或WPA，相应地改变Wifi.begin()的调用

<!-- more --> 


#  硬件要求
> * Arduino WiFi Shield 101
> * Arduino or Genuino Zero board或者MKR1000(可选择)连接到模拟引脚pin 2-5的6个模拟传感器

#  电路图
> * 数字引脚7被用来作为WiFi Shield 101 和 开发板的握手引脚使用，而不应该被占用。
> * 这个例子里你应该进入一个连接到互联网的802.11b/g无线网络。你需要改变程序里的网络设置来符合您的特定网络SSID。
> * 对于使用WPA/WPA2个人加密的网络，你需要的SSID和密码。扩展块将无法通过WPA2企业加密连接到网络。
> * WEP网络密码进制字符串作为键。一个WEP网络可以有4种不同的钥匙；每个按键都分配了一个“关键指标”的价值。WEP加密的网络，你需要SSID，key，和key number。
![Simple Web Server WiFi](/images/ArduinoWiFi101.png "Simple Web Server WiFi")
    在上图，Arduino 或 Genuino Zero 开发板应该在堆叠在 WiFi shield上面

# 代码

```C++
/*
  WiFi Web Server LED Blink

 A simple web server that lets you blink an LED via the web.
 This sketch will print the IP address of your WiFi Shield (once connected)
 to the Serial monitor. From there, you can open that address in a web browser
 to turn on and off the LED on pin 9.

 If the IP address of your shield is yourAddress:
 http://yourAddress/H turns the LED on
 http://yourAddress/L turns it off

 This example is written for a network using WPA encryption. For
 WEP or WPA, change the Wifi.begin() call accordingly.

 Circuit:
 * WiFi shield attached
 * LED attached to pin 9

 created 25 Nov 2012
 by Tom Igoe
 */
#include <SPI.h>
#include <WiFi101.h>

char ssid[] = "yourNetwork";      //  your network SSID (name)
char pass[] = "secretPassword";   // your network password
int keyIndex = 0;                 // your network key Index number (needed only for WEP)

int status = WL_IDLE_STATUS;
WiFiServer server(80);

void setup() {
  Serial.begin(9600);      // initialize serial communication
  pinMode(9, OUTPUT);      // set the LED pin mode

  // check for the presence of the shield:
  if (WiFi.status() == WL_NO_SHIELD) {
    Serial.println("WiFi shield not present");
    while (true);       // don't continue
  }

  // attempt to connect to Wifi network:
  while ( status != WL_CONNECTED) {
    Serial.print("Attempting to connect to Network named: ");
    Serial.println(ssid);                   // print the network name (SSID);

    // Connect to WPA/WPA2 network. Change this line if using open or WEP network:
    status = WiFi.begin(ssid, pass);
    // wait 10 seconds for connection:
    delay(10000);
  }
  server.begin();                           // start the web server on port 80
  printWifiStatus();                        // you're connected now, so print out the status
}


void loop() {
  WiFiClient client = server.available();   // listen for incoming clients

  if (client) {                             // if you get a client,
    Serial.println("new client");           // print a message out the serial port
    String currentLine = "";                // make a String to hold incoming data from the client
    while (client.connected()) {            // loop while the client's connected
      if (client.available()) {             // if there's bytes to read from the client,
        char c = client.read();             // read a byte, then
        Serial.write(c);                    // print it out the serial monitor
        if (c == '\n') {                    // if the byte is a newline character

          // if the current line is blank, you got two newline characters in a row.
          // that's the end of the client HTTP request, so send a response:
          if (currentLine.length() == 0) {
            // HTTP headers always start with a response code (e.g. HTTP/1.1 200 OK)
            // and a content-type so the client knows what's coming, then a blank line:
            client.println("HTTP/1.1 200 OK");
            client.println("Content-type:text/html");
            client.println();

            // the content of the HTTP response follows the header:
            client.print("Click <a href=\"/H\">here</a> turn the LED on pin 9 on<br>");
            client.print("Click <a href=\"/L\">here</a> turn the LED on pin 9 off<br>");

            // The HTTP response ends with another blank line:
            client.println();
            // break out of the while loop:
            break;
          }
          else {      // if you got a newline, then clear currentLine:
            currentLine = "";
          }
        }
        else if (c != '\r') {    // if you got anything else but a carriage return character,
          currentLine += c;      // add it to the end of the currentLine
        }

        // Check to see if the client request was "GET /H" or "GET /L":
        if (currentLine.endsWith("GET /H")) {
          digitalWrite(9, HIGH);               // GET /H turns the LED on
        }
        if (currentLine.endsWith("GET /L")) {
          digitalWrite(9, LOW);                // GET /L turns the LED off
        }
      }
    }
    // close the connection:
    client.stop();
    Serial.println("client disonnected");
  }
}

void printWifiStatus() {
  // print the SSID of the network you're attached to:
  Serial.print("SSID: ");
  Serial.println(WiFi.SSID());

  // print your WiFi shield's IP address:
  IPAddress ip = WiFi.localIP();
  Serial.print("IP Address: ");
  Serial.println(ip);

  // print the received signal strength:
  long rssi = WiFi.RSSI();
  Serial.print("signal strength (RSSI):");
  Serial.print(rssi);
  Serial.println(" dBm");
  // print where to go in a browser:
  Serial.print("To see this page in action, open a browser to http://");
  Serial.println(ip);
}
```

>**后续扩展**
> * [Connect With WPA](https://www.arduino.cc/en/Tutorial/ConnectWithWPA): 演示如何连接到一个用WPA2个人加密网络
> * [WiFi Web Client Repeating](https://www.arduino.cc/en/Tutorial/ConnectWithWPA): 反复做HTTP请求到服务器。

作者 [@碧海饮冰人]    
2017 年 11 月 5日    


