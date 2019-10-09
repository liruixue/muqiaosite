---
title: '大数据-elasticsearch-kibana'
date: 2019-10-09 15:19:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- kibana安装
---




Kibana提供搜索、查看和与存储在 Elasticsearch 索引中的数据进行交互的功能，开发人员用以执行高级数据分析，并在各种图表、表格和地图中可视化数据.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-kibana-home.jpg)
<center><font color=#c3c3c3>深圳莲花山下的一尊塑像</font></center>
<!-- more -->
>本节主要引导在centos下如何安装Kibana


# 下载
如果你的centos可以访问外网的话，推荐直接在linux中下载，执行如下命令：
```bash
wget https://artifacts.elastic.co/downloads/kibana/kibana-7.4.0-x86_64.rpm
```
上面的那个kibana版本可以更改成你自己es对应的版本，比如说我的es版本为6.2.4,那么上面的7.4.0就可以直接用6.2.4来替换。
# 安装
```bash
rpm --install kibana-7.4.0-x86_64.rpm
```
安装过程中，如果出现‘warning’信息，可以不用管它，直到kibana安装完成.
# 启动
切换进elasticsearch-6.2.4目录，然后执行启动命令：
```bash
service kibana start
```
如果要关闭kibana则使用以下命令：
```bash
service kibana stop
```
请注意，以上命令的执行需要使用root用户权限，如果不是root用户，命令前面需要加上sudo来执行.
以上启动方式，启动过程中看不到任何输出，如果想观察日志输出，则需要打开var/log/kibana/目录下的日志才可以.
启动后打开浏览器访问 http://localhost:5601 即可浏览 kibana 界面

# Kibana RPM包的配置、日志、数据层级结构
| 类型  | 描述   |  默认位置  |  设置 |
| ----   | ---- | ----  | ----  |
|home| Kibana home directory or KIBANA_HOME|/usr/share/kibana |  |
|bin|Binary scripts including kibana to start the Kibana server and kibana-plugin to install plugins | /usr/share/kibana/bin|   |
|config |Configuration files including kibana.yml |/etc/kibana| |
|data|The location of the data files written to disk by Kibana and its plugins|/var/lib/kibana| path.data|
|logs| Logs files location|/var/log/kibana| path.logs|
|optimize| Transpiled source code. Certain administrative actions (e.g. plugin install) result in the source code being retranspiled on the fly|/usr/share/kibana/optimize|   |
|plugins | Plugin files location. Each plugin will be contained in a subdirectory |/usr/share/kibana/plugins|   | |

# Kibana的配置详情
Kibana的默认配置文件路径位于/etc/kibana/kibana.yml,配置文件的格式内容可以相关的[配置说明](https://www.elastic.co/guide/en/kibana/7.4/settings.html)
将默认配置改成如下：
```bash
server.port: 5601
server.host: "0.0.0.0"	//默认是localhost，0.0.0.0是任何ip都可以访问
elasticsearch.url: "http://127.0.0.1:9200"    //修改成自己集群的端口号
kibana.index: ".kibana"
```

</br>
作者 [@碧海饮冰人]    
2019 年 10月 9日    
  



