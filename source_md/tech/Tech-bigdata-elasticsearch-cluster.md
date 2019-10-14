---
title: '大数据-elasticsearch-集群搭建'
date: 2019-10-12 19:12:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- elasticsearch
---




Elasticsearch运行需要高可用性，想要高可用性就要搭建集群
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-cluster-home.jpg)
<center><font color=#c3c3c3>印度史诗《罗摩衍那》神猴哈奴曼的皮影形象</font></center>
<!-- more -->
>本节主要讲述如何搭建Elasticsearch的集群


# 为何要搭建 Elasticsearch 集群
##  一是获取高可用性
什么是高可用性呢？它通常是指，通过设计减少系统不能提供服务的时间。假设系统一直能够提供服务，我们说系统的可用性是 100%。如果系统在某个时刻宕掉了，比如某个网站在某个时间挂掉了，那么就可以它临时是不可用的。所以，为了保证 Elasticsearch 的高可用性，我们就应该尽量减少 Elasticsearch 的不可用时间。

那么怎样提高 Elasticsearch 的高可用性呢？这时集群的作用就体现出来了。假如 Elasticsearch 只放在一台服务器上，即单机运行，假如这台主机突然断网了或者被攻击了，那么整个 Elasticsearch 的服务就不可用了。但如果改成 Elasticsearch 集群的话，有一台主机宕机了，还有其他的主机可以支撑，这样就仍然可以保证服务是可用的。

##  二是保持索引的健康状况
针对一个索引，Elasticsearch 中其实有专门的衡量索引健康状况的标志，分为三个等级：
* green，绿色。这代表所有的主分片和副本分片都已分配。你的集群是 100% 可用的。
* yellow，黄色。所有的主分片已经分片了，但至少还有一个副本是缺失的。不会有数据丢失，所以搜索结果依然是完整的。不过，你的高可用性在某种程度上被弱化。如果更多的分片消失，你就会丢数据了。所以可把 yellow 想象成一个需要及时调查的警告
* red，红色。至少一个主分片以及它的全部副本都在缺失中。这意味着你在缺少数据：搜索只能返回部分数据，而分配到这个分片上的写入请求会返回一个异常

如果你只有一台主机的话，其实索引的健康状况也是 yellow，因为一台主机，集群没有其他的主机可以防止副本，所以说，这就是一个不健康的状态，因此集群也是十分有必要的

##  三是获取更大的存储空间
另外，既然是群集，那么存储空间肯定也是联合起来的，假如一台主机的存储空间是固定的，那么集群它相对于单个主机也有更多的存储空间，可存储的数据量也更大


# Elasticsearch 集群的建立
首先我们应该清楚多台主机构成了一个集群，每台主机称作一个节点（Node）.
如图就是一个三节点的集群：
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-cluster-deploy.png)
在图中，每个 Node 都有三个分片，其中 P 开头的代表 Primary 分片，即主分片，R 开头的代表 Replica 分片，即副本分片。所以图中主分片 1、2，副本分片 0 储存在 1 号节点，副本分片 0、1、2 储存在 2 号节点，主分片 0 和副本分片 1、2 储存在 3 号节点，一共是 3 个主分片和 6 个副本分片。同时我们还注意到 1 号节点还有个 MASTER 的标识，这代表它是一个主节点，它相比其他的节点更加特殊，它有权限控制整个集群，比如资源的分配、节点的修改等等.
关于节点的概念请参看我的另一篇[Elasticsearch术语](http://limuqiao.com/2019/10/09/tech/Tech-bigdata-elasticsearch-glossary/)的文章.

好，让我们来动手搭建一个集群吧。
这里我一共拥有2台 Linux 主机，系统是 Centos7，都连接在一个内网中，IP 地址为：
```bash
192.168.0.51
192.168.0.52
```

下面我们来一步步介绍如何用这几台主机搭建一个 Elasticsearch 集群，这里使用的 Elasticsearch 版本是 6.2.4，另外我们还需要安装 Kibana 用来可视化监控和管理 Elasticsearch 的相关配置和数据，使得集群的管理更加方便。
## 配置 Elasticsearch
在每台主机都安装好了 Elasticsearch之后，接下来就是要将它们联系在一起构成一个集群.
Elasticsearch 的配置文件是 /etc/elasticsearch/elasticsearch.yml，接下来让我们编辑一下配置文件：
* 集群的名称
通过 cluster.name 可以配置集群的名称，集群是一个整体，因此名称都要一致，所有主机都配置成相同的名称，配置示例：
```bash
cluster.name: germey-es-clusters
```
* 节点的名称
通过 node.name 可以配置每个节点的名称，每个节点都是集群的一部分，每个节点名称都不要相同，可以按照顺序编号（本机为es-node-1，则另外一台为es-node-2），配置示例：
```bash
node.name: es-node-1
```

* 是否有资格成为主节点
通过 node.master 可以配置该节点是否有资格成为主节点，如果配置为 true，则主机有资格成为主节点，配置为 false 则主机就不会成为主节点，可以去当数据节点或负载均衡节点。注意这里是有资格成为主节点，不是一定会成为主节点，主节点需要集群经过选举产生。这里我配置所有主机都可以成为主节点，因此都配置为 true，配置示例：
```bash
node.master: true
```
* 数据和日志路径
通过 path.data 和 path.logs 可以配置 Elasticsearch 的数据存储路径和日志存储路径，可以指定任意位置，另外注意一下写入权限问题，配置示例：
```bash
# ------ Paths -------------
#
# Path to directory where to store the data (separate multiple locations by comma):
#
path.data: /A_Install/es/elasticsearch-6.2.4/data
#
# Path to log files:
#
path.logs: /A_Install/es/elasticsearch-6.2.4/log
```
* 设置访问的地址和端口
我们需要设定 Elasticsearch 运行绑定的 Host，默认是无法公开访问的，如果设置为主机的公网 IP 或 0.0.0.0 就是可以公开访问的，这里我们可以都设置为公开访问或者部分主机公开访问，如果不想被公开访问就不用配置, 如若要公开则访问配置为：
```bash
network.host: 0.0.0.0   //0.0.0.0 表示公开访问
http.port: 9200    //访问的端口，默认是 9200
```
* 集群地址设置
通过 discovery.zen.ping.unicast.hosts 可以配置集群的主机地址，配置之后集群的主机之间可以自动发现，这里我配置的是内网地址，配置示例：
```bash
discovery.zen.ping.unicast.hosts: ["192.168.0.51", "192.168.0.52"]
```
* 节点数目相关配置
为了防止集群发生“脑裂”，即一个集群分裂成多个，通常需要配置集群最少主节点数目，通常为 (可成为主节点的主机数目 / 2) + 1，注意每台主机都要如此配置，例如我这边可以成为主节点的主机数目为 7，那么结果就是 4，配置示例：
```bash
discovery.zen.minimum_master_nodes: 4
```

## 启动所有Elasticsearch
所有主机都启动之后，我们在任意主机上就可以查看到集群状态了，命令行运行以下命令即可：
```bash
curl -XGET 'http://localhost:9200/_cluster/state?pretty'
```
类似的输出如下：
```bash
{
"cluster_name": "tm-es-clusters",
"compressed_size_in_bytes": 294,
"version": 2,
"state_uuid": "9x5Kqt_3S0aGMs42g85XYQ",
"master_node": "CubB6Cv5RXGq-Mkv6RIPgQ",
"blocks": { },
"nodes": {
	"CubB6Cv5RXGq-Mkv6RIPgQ": {
	"name": "es-node-1",
	"ephemeral_id": "GHI7TbRdRkCWk8wqOKaBhA",
	"transport_address": "192.168.0.52:9300",
	"attributes": { }
	},
	"Vv-5WIqfQLK5Hf6zSZPpbg": {
	"name": "es-node-2",
	"ephemeral_id": "CXBptNnUTc22pkCpG0RaPw",
	"transport_address": "192.168.0.51:9300",
	"attributes": { }
	}
},
...
}
```
可以看到这里输出了集群的相关信息，同时 nodes 字段里面包含了每个节点的详细信息，这样一个基本的集群就构建完成了.

## 配置Kibana
Kibana的安装可以参见我[这里的一篇文章](http://limuqiao.com/2019/10/09/tech/Tech-bigdata-elasticsearch-kibana/).
Kibana的默认配置文件路径位于/etc/kibana/kibana.yml,配置文件里的elasticsearch.url项要改成相对应的ElasticSearch集群地址：
```bash
elasticsearch.url: "http://127.0.0.1:9200"    //这里修改成自己集群的端口号
```
### 查询集群状态
Dev Tools的控制台里可以直接输入下列指令来查询状态：
```bash
GET /_cluster/health
```
节点返回的结果如下图示：
```bash
{
  "cluster_name": "tm-es-clusters",
  "status": "green",
  "timed_out": false,
  "number_of_nodes": 2,
  "number_of_data_nodes": 2,
  "active_primary_shards": 0,
  "active_shards": 0,
  "relocating_shards": 0,
  "initializing_shards": 0,
  "unassigned_shards": 0,
  "delayed_unassigned_shards": 0,
  "number_of_pending_tasks": 0,
  "number_of_in_flight_fetch": 0,
  "task_max_waiting_in_queue_millis": 0,
  "active_shards_percent_as_number": 100
}
```
status字段，通过green,yellow,red三种指示就可以提供一个综合的指标来表示集群的的服务状况.
### 添加索引
为了将数据添加到Elasticsearch，我们需要索引(index)——一个存储关联数据的地方.
让我们在集群中唯一一个空节点上创建一个叫做tm的索引。默认情况下，一个索引被分配5个主分片，但是为了演示的目的，我们只分配3个主分片和2个复制分片（每个主分片都有一个复制分片）：
```bash
PUT /tm
{
   "settings" : {
      "number_of_shards" : 3,
      "number_of_replicas" : 2
   }
}
```
返回的结果：
```bash
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "tm"
}
```
复制分片的数量number_of_replicas可以在运行中的集群中动态地变更，这允许我们可以根据需求扩大或者缩小规模：
```bash
PUT /blogs/_settings
{
   "number_of_replicas" : 2
}
```
Ok，ElasticSearch的集群搭建与数据索引添加到此已结束。

还有更多的 Elasticsearch 相关的内容可以参考官方文档：https://www.elastic.co/guide/index.html
</br>
作者 [@碧海饮冰人]    
2019 年 10月 12日    
  



