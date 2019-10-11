---
title: 'Linux centos上Java环境安装'
date: 2019-10-11 12:37:27
categories:
- 编程吧
- Java
tags:
- java
---




Java(JDK8)环境的安装.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-java-install-home.jpg)
<center><font color=#c3c3c3>简单的几个水果和一块芯片板连接Iphone后就是一个电子鼓</font></center>
<!-- more -->
>本节主要告诉你CentOS 7 下安装 JAVA环境（JDK 1.8）


# 打开url选择jdk1.8下载，下载会要求登录Oracle网站，免费注册个账号即可
下载地址见：
https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
可选择linux X64版本
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-java-install-download.png)

# 下载
可以复制浏览器中待下载文件的路径，然后使用wget命令进行下载：
```bash
wget http://download.oracle.com/otn-pub/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.tar.gz?AuthParam=15322212121121_4tewrwr22232322sww
```
# 安装
##  创建安装目录
```bash
mkdir /usr/local/java/
```
##  解压至安装目录
```bash
tar -zxvf jdk-8u171-linux-x64.tar.gz -C /usr/local/java/
```
# 设置环境变量

打开文件
```bash
vi /etc/profile
```
在末尾添加
```bash
export JAVA_HOME=/usr/local/java/jdk1.8.0_171
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
使环境变量生效
```bash
source /etc/profile
```
检查是否生效
```bash
[root@ssk]# java -version
java version "1.8.0_221"
Java(TM) SE Runtime Environment (build 1.8.0_221-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.221-b11, mixed mode)
```

</br>
作者 [@碧海饮冰人]    
2019 年 10月 11日    
  



