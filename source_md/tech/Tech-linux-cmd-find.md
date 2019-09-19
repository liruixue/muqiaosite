---
title: 'linux系列命令-find查找命令+模糊匹配'
date: 2019-09-19 17:12:27
categories:
- 编程吧
- linux
tags:
- linux
- centos
---




linux或centos系统中常用到文件查询及搜索命令，find是比较常用的一个.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-linux-cmd-find-home.jpg)
<center><font color=#c3c3c3>现在的盲盒好火爆，一只几十元的都能炒到上千，不知道咋火起来的</font></center>
<!-- more -->
>本节主要演示find的各种用法


# 在当前目录下搜索指定文件：
```bash
find . -name test.txt
```

# 在当前目录下模糊搜索文件：
```bash
find . -name '*.txt'
```

# 在当前目录下搜索特定属性的文件：
```bash
find . -amin -10 # 查找在系统中最后10分钟访问的文件
find . -atime -2 # 查找在系统中最后48小时访问的文件
find . -mmin -5 # 查找在系统中最后5分钟里修改过的文件
find . -mtime -1 #查找在系统中最后24小时里修改过的文件
find . -user fred #查找在系统中属于FRED这个用户的文件
```
# 在当前目录搜索文件内容含有某字符串（大小写敏感）的文件：
```bash
find . -type f | xargs grep 'your_string'
```

# 在当前目录搜索文件内容含有某字符串（大小写敏感）的特定文件：
```bash
find . -type f -name '*.sh' | xargs grep 'your_string'
```

# 在当前目录搜索文件内容含有某字符串（忽略大小写）的特定文件：
```bash
find . -type f -name '*.sh' | xargs grep -i 'your_string'
```


</br>
作者 [@碧海饮冰人]    
2019 年 9月 19日    
  



