---
title: 'node.js的server可以用localhost访问，却为什么IP访问不了'
date: 2017-11-12 16:33:27
categories:
- 编程吧
- NodeJS
tags:
- nodejs
- 配置
- 编程
---

# 问题概述
node.js的server启动后，hellow world可以用localhost访问到，换了IP为什么访问不了?

<!-- more --> 

#  问题背景及描述
------
使用了NodeJS上的Demo代码，启动了一个webserver，用localhost或127.0.0.1能够访问到，但是换成ip地址就一直访问不到，代码如下：
```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 4000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

server.listen(port, hostname, () => {
  console.log(`server running at http://${hostname}:${port}/`);
});

```

------

#  问题分析及解决过程
> * 怀疑端口问题，将阿里云里的安全组端口4000开放，仍然无法解决
> * 怀疑启动的hostname应该是用ip，然后直接将ip地址赋值给hostname，再运行，结果webserver启动失败，直接报错


#  成功解决方案
查看了下NodeJS的http.createServer文档，发现可以不指定hostname，然后去除hostname的声明，可以成功用localhost及ip地址访问
```javascript
const http = require('http');

//const hostname = '127.0.0.1';
const port = 4000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World!\n');
});

server.listen(port, () => {
  console.log(`server running at ${port}/`);
});
```

    声明---原创技术文章，欢迎转载并请带出原文链接以尊重个人劳动及知识产权，谢谢

作者 [@碧海饮冰人]    
2017 年 11 月 12日    


