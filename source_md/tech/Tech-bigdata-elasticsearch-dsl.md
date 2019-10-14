---
title: '大数据-elasticsearch-结构化搜索'
date: 2019-10-14 18:12:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- elasticsearch
---




结构化搜索是指查询包含内部结构的数据. 日期，时间，和数字都是结构化的：它们有明确的格式给你执行逻辑操作.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-dsl-home.jpg)
<center><font color=#c3c3c3>印度史诗《罗摩衍那》神猴哈奴曼的皮影形象</font></center>
<!-- more -->
>本节主要讲述通过结构化搜索，你的查询结果始终为 是或非；是否应该属于集合。


结构化搜索不关心文档的相关性或分数，它只是简单的包含或排除文档.
# 查找准确值
对于准确值，你需要使用过滤器。过滤器的重要性在于它们非常的快。它们不计算相关性（避过所有计分阶段）而且很容易被缓存，请先记住尽可能多的使用过滤器.
## 用于数字的 term 过滤器
这个过滤器旨在处理数字，布尔值，日期，和文本。
我们来看一下例子，一些产品最初用数字来索引，包含两个字段`price`和`productID`：
```bash
POST /my_store/products/_bulk
{ "index": { "_id": 1 }}
{ "price" : 10, "productID" : "XHDK-A-1293-#fJ3" }
{ "index": { "_id": 2 }}
{ "price" : 20, "productID" : "KDKE-B-9947-#kL5" }
{ "index": { "_id": 3 }}
{ "price" : 30, "productID" : "JODL-X-1937-#pV7" }
{ "index": { "_id": 4 }}
{ "price" : 30, "productID" : "QQPX-R-3956-#aD8" }
```
我们的目标是找出特定价格的产品，在 Elasticsearch DSL 中，我们使用 term 过滤器来查找我们设定的准确值. term 过滤器本身很简单，它接受一个字段名和我们希望查找的值：
```bash
{
    "term" : {
        "price" : 20
    }
}
```
搜索 API 需要得到一个查询语句，而不是一个 过滤器。为了使用 term 过滤器，我们需要将它包含在一个过滤查询语句中：
```bash
GET /my_store/products/_search
{
    "query" : {
        "filtered" : { <1>
            "query" : {
                "match_all" : {} <2>
            },
            "filter" : {
                "term" : { <3>
                    "price" : 20
                }
            }
        }
    }
}
```
<1> `filtered`查询同时接受`query`与`filter`
<2> `match_all`用来匹配所有文档，这是默认行为，所以在以后的例子中我们将省略掉`query`部分
<3> 这是我们上面见过的`term`过滤器。注意它在`filter`分句中的位置
执行之后，你将得到预期的搜索结果：只能文档 2 被返回了:
```bash
"hits" : [
    {
        "_index" : "my_store",
        "_type" :  "products",
        "_id" :    "2",
        "_score" : 1.0, <1>
        "_source" : {
          "price" :     20,
          "productID" : "KDKE-B-9947-#kL5"
        }
    }
]
```
<1> 过滤器不会执行计分和计算相关性
## 用于文本的 term 过滤器
term 过滤器可以像匹配数字一样轻松的匹配字符串。那让我们不是通过价格，而通过特定 UPC(universal product code)标识码来找出产品，看看是怎样的.
我们用 term 过滤器来构造一个类似的DSL查询：
```bash
GET /my_store/products/_search
{
    "query" : {
        "filtered" : {
            "filter" : {
                "term" : {
                    "productID" : "XHDK-A-1293-#fJ3"
                }
            }
        }
    }
}
```
这次有点出乎意料，我们没有得到任何结果值！为什么呢？问题不在于`term`查询，而在于数据被索引的方式. 如果我们使用`analyze API`，我们可以看到 UPC 被分解成短小的表征：
```bash
GET /my_store/_analyze?field=productID
XHDK-A-1293-#fJ3
```
返回结果
```bash
{
  "tokens" : [ {
    "token" :        "xhdk",
    "start_offset" : 0,
    "end_offset" :   4,
    "type" :         "<ALPHANUM>",
    "position" :     1
  }, {
    "token" :        "a",
    "start_offset" : 5,
    "end_offset" :   6,
    "type" :         "<ALPHANUM>",
    "position" :     2
  }, {
    "token" :        "1293",
    "start_offset" : 7,
    "end_offset" :   11,
    "type" :         "<NUM>",
    "position" :     3
  }, {
    "token" :        "fj3",
    "start_offset" : 13,
    "end_offset" :   16,
    "type" :         "<ALPHANUM>",
    "position" :     4
  } ]
}
```
这里看到的问题：
* 我们得到了四个分开的标记，而不是一个完整的标记来表示 UPC。
* 所有的字符都被转为了小写。
* 我们失去了连字符和 # 符号。

所以当我们用 XHDK-A-1293-#fJ3 来查找时，得不到任何结果，因为这个标记不在我们的倒排索引中，显然，在处理唯一标识码，或其他枚举值时，这不是我们想要的结果.

为了避免这种情况发生，我们需要通过设置这个字段为`not_analyzed`来告诉 Elasticsearch 它包含一个准确值. 我们曾在【自定义字段映射】中见过它。为了实现目标，我们要先删除旧索引（因为它包含了错误的映射），并创建一个正确映射的索引：
```bash
DELETE /my_store <1>

PUT /my_store <2>
{
    "mappings" : {
        "products" : {
            "properties" : {
                "productID" : {
                    "type" : "string",
                    "index" : "not_analyzed" <3>
                }
            }
        }
    }

}
```
<1> 必须首先删除索引，因为我们不能修改已经存在的映射。
<2> 删除后，我们可以用自定义的映射来创建它。
<3> 这里我们明确表示不希望`productID`被分析
现在我们可以继续重新索引文档：
```bash
POST /my_store/products/_bulk
{ "index": { "_id": 1 }}
{ "price" : 10, "productID" : "XHDK-A-1293-#fJ3" }
{ "index": { "_id": 2 }}
{ "price" : 20, "productID" : "KDKE-B-9947-#kL5" }
{ "index": { "_id": 3 }}
{ "price" : 30, "productID" : "JODL-X-1937-#pV7" }
{ "index": { "_id": 4 }}
{ "price" : 30, "productID" : "QQPX-R-3956-#aD8" }
```
现在我们的 term 过滤器将按预期工作. 让我们在新索引的数据上再试一次（注意，查询和过滤都没有修改，只是数据被重新映射了）:
```bash
GET /my_store/products/_search
{
    "query" : {
        "filtered" : {
            "filter" : {
                "term" : {
                    "productID" : "XHDK-A-1293-#fJ3"
                }
            }
        }
    }
}
```
`productID`字段没有经过分析，`term`过滤器也没有执行分析，所以这条查询找到了准确匹配的值，如期返回了文档 1.

</br>
作者 [@碧海饮冰人]    
2019 年 10月 14日    
  



