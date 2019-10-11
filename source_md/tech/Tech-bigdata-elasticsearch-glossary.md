---
title: '大数据-elasticsearch-术语表'
date: 2019-10-09 17:55:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- elasticsearch
- 术语
---




个人认为，所有的学习，都是概念的掌握。Elasticsearch的学习过程中，本篇将记录过程中关联的所有抽象概念.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-glossary-home.jpg)
<center><font color=#c3c3c3>一尊闲适自在的菩萨像</font></center>
<!-- more -->
>本节将记录Elasticsearch学习过程中关联的所有抽象概念.

# 基本理解
在Elasticsearch中，文档归属于一种类型(type),而这些类型存在于索引(index)中，我们可以画一些简单的对比图来类比传统关系型数据库：
```bash
Relational DB -> Databases -> Tables -> Rows -> Columns
Elasticsearch -> Indices   -> Types  -> Documents -> Fields
```
Elasticsearch集群可以包含多个索引(indices)（数据库），每一个索引可以包含多个类型(types)（表），每一个类型包含多个文档(documents)（行），然后每个文档包含多个字段(Fields)（列）。
# 术语表
| 术语名称       | 全称  |  解释 |  备注  |
| --------   | -----  | ----  | ----  |
|JSON| JavaScript Object Notation| Javascript对象符号,作为文档序列化格式 | |
|索引| indexing| 在Elasticsearch中存储数据的行为 | ‘索引一个文档’表示把一个文档存储到索引（名词）里，以便它可以被检索或者查询|
|倒排索引| JavaScript Object Notation|ES/Lucene区别于传统数据库B-Tree索引的一种数据结构来加速检索 | |
|DSL查询| Domain Specific Language|通过请求体方式替换了查询字符串，使用JSON表示法可包含match语句| |
|分片| shard|分片是Elasticsearch在集群中分发数据的关键，把分片想象成数据的容器。文档存储在分片中，然后分片分配到你集群中的节点上。当你的集群扩容或缩小，Elasticsearch将会自动在你的节点间迁移分片，以使集群保持平衡|分片可以是主分片(primary shard)或者是复制分片(replica shard) |
|主节点| Master 节点是和集群操作相关的内容，如创建或删除索引，跟踪哪些节点是群集的一部分，并决定哪些分片分配给相关的节点| |
|Data 节点| 数据节点|存储索引数据的节点，主要对文档进行增删改查操作，聚合操作等。数据节点对 CPU、内存、IO 要求较高，在优化的时候需要监控数据节点的状态，当资源不够的时候，需要在集群中添加新的节点| |
| Client 节点| 负载均衡节点|只能处理路由请求，处理搜索，分发索引操作等，从本质上来说该客户节点表现为智能负载平衡器，独立的客户端节点在一个比较大的集群中是非常有用的| |
| Ingest 节点| 预处理节点|在索引数据之前可以先对数据做预处理操作，所有节点其实默认都是支持 Ingest 操作的，也可以专门将某个节点配置为 Ingest 节点| |


</br>
作者 [@碧海饮冰人]    
2019 年 10月 9日    
  



