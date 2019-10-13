---
title: '大数据-elasticsearch-安装'
date: 2019-10-08 17:12:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- elasticsearch安装
---




Elasticsearch运行需要 Java 8 环境，在安装Elasticsearch之前先安装好JDK.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-install-home.jpg)
<center><font color=#c3c3c3>故乡城中心公园里的八卦亭，二十年了仍安稳如初</font></center>
<!-- more -->
>本节主要引导在centos下如何安装Elasticsearch


# 下载
如果你的linux可以访问外网的话，推荐直接在linux中下载，执行如下命令：
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.4.tar.gz
```

# 解压
```bash
tar -zxvf elasticsearch-6.2.4.tar.gz
```
解压完成后，会出现elasticsearch-6.2.4目录.

# 启动
切换进elasticsearch-6.2.4目录，然后执行启动命令：
```bash
./bin/elasticsearch
```
如果想要以守护进程的方式运行，可以添加-d参数：
```bash
./bin/elasticsearch -d
```
这时如果你是root用户启动的话，会报"can not run elasticsearch as root"的错误。因为安全问题elasticsearch不让用root用户直接运行，所以要创建新用户.


## 创建新用户
第一步：liunx创建新用户："adduser esuser"，然后给创建的用户加密码："passwd esuser"，输入两次密码。

第二步：切换刚才创建的用户："su esuser"，然后启动elasticsearch。如果显示Permission denied权限不足，则继续进行第三步。

第三步：给新用户赋权限，因为这个用户本身就没有权限，肯定自己不能给自己付权限。所以要用root用户登录并赋予权限，chown -R esuser ./你的elasticsearch安装目录。

通过上面三步就可以启动elasticsearch了。

## 验证启动是否成功:
如果一切正常，Elasticsearch就会在默认的9200端口运行。这时，打开另一个命令行窗口，请求该端口：
```bash
curl 'http://localhost:9200/?pretty'
```
如果得到如下的返回，就说明启动成功了：
```bash
{
  "name" : "CubB6Cv",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "QNZmrNI5RqCOdtz1J405YA",
  "version" : {
    "number" : "6.2.4",
    "build_hash" : "ccec39f",
    "build_date" : "2018-04-12T20:37:28.497551Z",
    "build_snapshot" : false,
    "lucene_version" : "7.2.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

# 远程访问elasticsearch服务
## 配置为远程IP访问
默认情况下，Elasticsearch 只允许本机访问，如果需要远程访问，可以修改 Elasticsearch 安装目录中的config/elasticsearch.yml文件，去掉network.host的注释，将它的值改成0.0.0.0，让任何人都可以访问，然后重新启动 Elasticsearch 

```bash
network.host: 0.0.0.0
```
上面代码中，"network.host:"和"0.0.0.0"中间有个空格，不能忽略，不然启动会报错。线上服务不要这样设置，要设成具体的 IP进行限制.
## 切换为远程访问后的若干问题
1. max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
每个进程最大同时打开文件数太小，可通过下面2个命令查看当前数量
```bash
ulimit -Hn
ulimit -Sn
```
修改/etc/security/limits.conf文件，增加配置，用户退出后重新登录生效
```bash
* soft nofile 65536
* hard nofile 65536
```
2. max number of threads [1024] for user [esuser] is too low, increase to at least [4096]
问题同上，最大线程个数太低。修改配置文件/etc/security/limits.conf，继续增加nproc那几行：
```bash
* soft nofile 65536
* hard nofile 65536
* soft nproc 4096
* hard nproc 4096
esuser soft nproc 4096
```
注意：正常来说加入nproc带*那两行就足够，但是我使用esuser重新登录后，发现错误一直无法排除掉，所以后面把esuser soft nproc 4096那一行也加入了，才排除掉错误
是否生效，可以通过以下命令查看：
```bash
ulimit -Hu
ulimit -Su
```
3. max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

修改/etc/sysctl.conf文件，增加配置项：
vm.max_map_count=262144
```bash
vi /etc/sysctl.conf
sysctl -p
```
执行命令sysctl -p生效
4. system call filters failed to install; check the logs and fix your configuration or disable system call filters at your own risk
因为Centos6不支持SecComp，而ES5.4.0默认bootstrap.system_call_filter为true进行检测，所以导致检测失败，失败后直接导致ES不能启动
先将elasticsearch.yml中bootstrap.memory_lock配置项改为false，并在memory_lock配置项下面添加下面一行bootstrap.system_call_filter: false， 也即：
```bash
# ---------------- Memory ---------------
#
# Lock the memory on startup:
#
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
```


</br>
作者 [@碧海饮冰人]    
2019 年 10月 8日    
  



