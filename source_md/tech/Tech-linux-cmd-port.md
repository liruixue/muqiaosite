---
title: 'linux系列命令-端口查询'
date: 2019-08-31 17:12:27
categories:
- 编程吧
- linux
tags:
- linux
- centos
---




linux或centos系统中常用到端口查询命令，一般来说是可以组合使用的.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-linux-cmd-port-huanhuan.jpg)
<center><font color=#c3c3c3>一只帅气的虎斑猫</font></center>
<!-- more -->
>本节主要演示centos端口查询的常用命令


# 查看端口的开启情况
```bash
/sbin/iptables -L -n
```
查得结果情况：
```bash
Chain IN_public_allow (1 references)
target     prot opt source               destination         
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:22 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:3306 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8082 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8083 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8084 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:443 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:8085 ctstate NEW
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0            tcp dpt:80 ctstate NEW
```
# 查看端口对应的正在运行的进程
```bash
netstat   -nultp
```
查得结果情况：
```bash
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1132/sshd           
tcp6       0      0 :::9099                 :::*                    LISTEN      53305/java          
tcp6       0      0 127.0.0.1:8016          :::*                    LISTEN      97438/java          
tcp6       0      0 :::80                   :::*                    LISTEN      53305/java          
tcp6       0      0 :::8084                 :::*                    LISTEN      97438/java          
tcp6       0      0 :::22                   :::*                    LISTEN      1132/sshd           
tcp6       0      0 127.0.0.1:6015          :::*                    LISTEN      53305/java          
tcp6       0      0 :::8070                 :::*                    LISTEN      97438/java          
tcp6       0      0 :::3306                 :::*                    LISTEN      61412/mysqld        
udp        0      0 127.0.0.1:323           0.0.0.0:*                           792/chronyd         
udp6       0      0 ::1:323                 :::*                                792/chronyd  
```
# 根据进程名称查询该进程的运行位置
```bash
ps -aux|grep java
```
查得结果情况：
```bash
root      20258  0.0  0.0 112660   972 pts/1    S+   17:05   0:00 grep --color=auto java
root      53305  0.1  3.8 8898784 614476 ?      Sl   Jul25  84:30 /usr/local/java/jdk1.8.0_121/jre/bin/java -Djava.util.logging.config.file=/usr/local/apache-tomcat-8.0.35/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Djdk.tls.ephemeralDHKeySize=2048 -Djava.endorsed.dirs=/usr/local/apache-tomcat-8.0.35/endorsed -classpath /usr/local/apache-tomcat-8.0.35/bin/bootstrap.jar:/usr/local/apache-tomcat-8.0.35/bin/tomcat-juli.jar -Dcatalina.base=/usr/local/apache-tomcat-8.0.35 -Dcatalina.home=/usr/local/apache-tomcat-8.0.35 -Djava.awt.headless=true -Djava.io.tmpdir=/usr/local/apache-tomcat-8.0.35/temp org.apache.catalina.startup.Bootstrap start
root      97438  0.1  6.4 8892384 1035068 ?     Sl   Aug01  60:16 /usr/local/java/jdk1.8.0_121/jre/bin/java -Djava.util.logging.config.file=/usr/local/tomcat8-uic/conf/logging.properties -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager -Duser.timezone=Asia/shanghai -Djdk.tls.ephemeralDHKeySize=2048 -Djava.endorsed.dirs=/usr/local/tomcat8-uic/endorsed -classpath /usr/local/tomcat8-uic/bin/bootstrap.jar:/usr/local/tomcat8-uic/bin/tomcat-juli.jar -Dcatalina.base=/usr/local/tomcat8-uic -Dcatalina.home=/usr/local/tomcat8-uic -Djava.awt.headless=true -Djava.io.tmpdir=/usr/local/tomcat8-uic/temp org.apache.catalina.startup.Bootstrap start
```
</br>
作者 [@碧海饮冰人]    
2019 年 8月 31日    
  



