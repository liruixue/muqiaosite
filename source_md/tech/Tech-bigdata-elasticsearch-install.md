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
<center><font color=#c3c3c3>故乡城中心公园里的八卦亭</font></center>
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
默认情况下，Elasticsearch 只允许本机访问，如果需要远程访问，可以修改 Elasticsearch 安装目录中的config/elasticsearch.yml文件，去掉network.host的注释，将它的值改成0.0.0.0，让任何人都可以访问，然后重新启动 Elasticsearch 

```bash
network.host: 0.0.0.0
```
上面代码中，"network.host:"和"0.0.0.0"中间有个空格，不能忽略，不然启动会报错。线上服务不要这样设置，要设成具体的 IP进行限制.


</br>
作者 [@碧海饮冰人]    
2019 年 10月 8日    
  



