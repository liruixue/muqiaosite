---
title: '大数据-elasticsearch-技巧'
date: 2019-10-10 17:12:27
categories:
- 编程吧
- 大数据
tags:
- 大数据
- elasticsearch
---




Elasticsearch运行后，有多种查询方式，可以有直接搜索及聚合查询等多种方式.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-bigdata-elasticsearch-skill-home.jpg)
<center><font color=#c3c3c3>庆建国70周年期间市民广场地铁站洋溢喜庆的氛围</font></center>
<!-- more -->
>本节主要解决Elasticsearch运行过程中所碰到的各种技术问题.


# Fielddata is disabled on text fields by default
Fielddata:默认情况下，大多数字段都已编入索引，这使它们可搜索.大多数字段都可以使用磁盘上的索引时间doc_values来实现此数据访问模式，但text字段不支持doc_values。
相反，text字段使用一个称为查询时内存数据结构 fielddata。该字段首次用于聚合，排序或在脚本中使用时，将按需构建此数据结构。
比如说employee对象类型里的interests字段尚未开启fielddata方式，那么可以使用以下方式进行开启：
```bash
PUT /megacorp/_mapping/employee
{       
  "properties": {
        "interests": {  
            "type": "text",
            "fielddata": true
        }       
    }         
}
```
以上语法借用了[Put mapping API](https://www.elastic.co/guide/en/elasticsearch/reference/current/indices-put-mapping.html) 添加了一个新的字段到已存在的Index上，或者在已存在的字段上改变搜索设置.

如出现如下提示，说明设置成功：
```bash
{"acknowledged":true}
```
然后即可执行如下的聚合查询方式
```bash
GET /megacorp/employee/_search
{
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests"
      }
    }
  }
}
```


</br>
作者 [@碧海饮冰人]    
2019 年 10月 10日    
  



