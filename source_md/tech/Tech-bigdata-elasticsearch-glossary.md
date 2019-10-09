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


</br>
作者 [@碧海饮冰人]    
2019 年 10月 8日    
  



