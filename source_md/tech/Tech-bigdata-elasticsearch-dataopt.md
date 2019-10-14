---
title: '大数据-elasticsearch-数据操作'
date: 2019-10-14 12:12:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- elasticsearch
---




Elasticsearch是一个分布式的文档(document)存储引擎，它可以实时存储并检索复杂数据结构——序列化的JSON文档.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-dataopt-home.jpg)
<center><font color=#c3c3c3>印度史诗《罗摩衍那》神猴哈奴曼的皮影形象</font></center>
<!-- more -->
>本节主要讲述如何使用API来创建、检索、更新和删除文档，以及文档如何安全在Elasticsearch中存储，以及如何让它们返回.


# 索引-自增ID
如果我们的数据没有自然ID，我们可以让Elasticsearch自动为我们生成。请求结构发生了变化：
PUT方法——“在这个URL中存储文档”要变成POST方法——"在这个类型下存储文档"
```bash
POST /website/blog/
{
  "title": "My second blog entry",
  "text":  "Still trying this out...",
  "date":  "2014/01/01"
}
```
返回的结果中包含一个自增的字段：
```bash
{
   "_index":    "website",
   "_type":     "blog",
   "_id":       "wM0OSFhDQXGZAWDf0-drSA",
   "_version":  1,
   "created":   true
}
```
# 检索获取
## 检索文档
想要从Elasticsearch中获取文档，我们使用同样的_index、_type、_id，但是HTTP方法改为GET：
```bash
GET /website/blog/123?pretty
```
响应包含了现在熟悉的元数据节点，增加了_source字段，它包含了在创建索引时我们发送给Elasticsearch的原始文档:
```bash
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 1,
  "found" :    true,
  "_source" :  {
      "title": "My first blog entry",
      "text":  "Just trying this out...",
      "date":  "2014/01/01"
  }
}
```
> pretty:在任意的查询字符串中增加pretty参数，会让Elasticsearch美化输出(pretty-print)JSON响应以便更加容易阅读
> _source字段不会被美化，它的样子与我们输入的一致
GET请求返回的响应内容包括`{"found": true}`，这意味着文档已经找到;如果我们请求一个不存在的文档，依旧会得到一个JSON，不过found值变成了false.
## 检索文档的一部分
通常，GET请求将返回文档的全部，存储在`_source`参数中. 但是可能你感兴趣的字段只是`title`，请求个别字段可以使用`_source`参数。多个字段可以使用逗号分隔：
```bash
GET /website/blog/123?_source=title,text
```
_source字段现在只包含我们请求的字段，而且过滤了date字段：
```bash
{
  "_index" :   "website",
  "_type" :    "blog",
  "_id" :      "123",
  "_version" : 1,
  "exists" :   true,
  "_source" : {
      "title": "My first blog entry" ,
      "text":  "Just trying this out..."
  }
}
```
或者你只想得到`_source`字段而不要其他的元数据，你可以这样请求：
```bash
GET /website/blog/123/_source
```
它仅仅返回:
```bash
{
   "title": "My first blog entry",
   "text":  "Just trying this out...",
   "date":  "2014/01/01"
}
```
还有更多的 Elasticsearch 相关的内容可以参考官方文档：https://es.xiaoleilu.com/030_Data/15_Get.html
# 理解查询语句
如果是合法语句的话，使用`explain`参数可以返回一个带有查询语句的可阅读描述， 可以帮助了解查询语句在ES中是如何执行的：
```bash
GET /_validate/query?explain
{
   "query": {
      "match" : {
         "tweet" : "really powerful"
      }
   }
}
```
`explanation`会为每一个索引返回一段描述，因为每个索引会有不同的映射关系和分析器：
```bash
{
  "valid" :         true,
  "_shards" :       { ... },
  "explanations" : [ {
    "index" :       "us",
    "valid" :       true,
    "explanation" : "tweet:really tweet:powerful"
  }, {
    "index" :       "gb",
    "valid" :       true,
    "explanation" : "tweet:really tweet:power"
  } ]
}
```
从返回的`explanation`你会看到 match 是如何为查询字符串 `"really powerful"`进行查询的， 首先，它被拆分成两个独立的词分别在`tweet`字段中进行查询。
而且，在索引us中这两个词为`"really"`和`"powerful"`，在索引gb中被拆分成`"really"`和`"power"`， 这是因为我们在索引gb中使用了english分析器.

</br>
作者 [@碧海饮冰人]    
2019 年 10月 14日    
  



