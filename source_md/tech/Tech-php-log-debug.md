---
title: 'php代码的调试与日志打印技巧'
date: 2019-09-04 19:37:27
categories:
- 编程吧
- php
tags:
- php
---




一门语言的掌握，首先要了解它的调试方法与日志的记录技巧.
![](https://raw.githubusercontent.com/liruixue/muqiaosite/master/images/tech/Tech-php-log-debug-home.jpg)
<center><font color=#c3c3c3>另一个角度观赏深圳第一高楼和深圳蓝</font></center>
<!-- more -->
>本节主要告诉你如何用一些常用的技巧去调试PHP代码并记录运行中的各种变量信息


# php常见"Illegal string offset 'XXXXXXX'"错误如何去解决
php代码运行中（如下），碰到变量的引用时，明明infoArray变量的打印可以看到brokerage_def_id属性，可能会碰到以上Illegal string offset的错误，不善用调试方法，有时就会觉得有些绝望。这个时候，你应该想到var_dump()方法来调试，该方法会将变量infoArray中的所有层级的数据类型都打印出来，你会发现某些层级的数据并不是你想象的类型：比如‘cart_info’属性现在有可能是字符串，而下面代码的使用方式是把它当成数组来引用.
```php
$cart['cart_info']['productInfo']['brokerage_def_id']
```
如果‘cart_info’属性经var_dump()输出，发现其类型为字符串，那么就可以用以下的方式，将字符串的变量转化为数组
```php
$infoArray= json_decode($cart['cart_info'], true);
```
然后就可以使用$infoArray['productInfo']['brokerage_def_id']的方式来读取属性值了.
# echo/var_export/var_dump的区别分析
echo：是语法结构，可以输出一个或多个字符串或字符串/数字变量，如果变量是复杂类型则不适用(这时可考虑var_dump()函数).
var_dump： (PHP 3 >= 3.0.5, PHP 4, PHP 5) 打印变量的相关信息出来，包括变量类型,变量长度和变量值，适合在调试过程中使用，使用方式如下：
```php
var_dump($$cart['cart_info']['productInfo']);
```
var_export: (PHP 4 >= 4.2.0, PHP 5) 输出或返回一个变量的字符串表示，并没有将该信息打印出去；适合在产品上线之后，将变量信息变为字串，从而在日志中打印出来，使用方式如下：
```php
mkLog(var_export($level_values,TRUE),'varsee');
```

</br>
作者 [@碧海饮冰人]    
2019 年 9月 04日    
  



