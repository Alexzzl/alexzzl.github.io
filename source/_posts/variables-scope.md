---
title: 【PHP面试题】PHP变量的作用域和静态变量
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-06 14:30:16
password:
summary:
tags:
    - PHP基础
    - PHP面试
    - 变量作用域
    - 静态变量
categories: PHP
---
## 一、变量的作用域
> 变量的作用域也称变量的范围，变量的范围即它定义的上下文背景（也是它的生效范围）。大部分的PHP变量只有一个单独的范围。这个单独的范围跨度同样包含了 include 和 require 引入的文件。

任何用于函数内部的变量按缺省情况将被限制在局部函数范围内。

```PHP

$a = 1; /* global scope 全局 */

function Test()
{
    echo $a; /* reference to local scope variable 局部 */
}

Test(); // Notice: Undefined variable: a

```

### 1. global关键字：
PHP 中全局变量在函数中使用时必须声明为 global。

```PHP
// global关键字
$outer = 'str'; // 全局变量
function mgfunc()
{
    global $outer; // 可以把外部的变量拿到内部来使用
    echo $outer; // 局部变量
}// 里面的 $outer 和 外面的 $outer不是一个变量
mgfunc() // str
```
### 2. $GLOBALS 及 其他超全局数组：
```PHP
// $GLOBALS
$outer = 'str';
function mgfunc()
{
    $GLOBALS['outer']; 
}
mgfunc() // str
```

## 二、静态变量
> 静态变量：仅在局部函数域中存在，但当程序执行离开此作用域时，其值并不会消失。

### static 关键字
1. 仅初始化一次；
1. 初始化时需要赋值；
1. 每次执行函数该值会保留；
1. static 修饰的变量是局部的【仅在函数内部有效】；
1. 可以记录函数的调用次数，从而可以在某些条件下终止递归。

代码实现：
```PHP
function mgFunc()
{
    static $a = 1;
    echo $a++;
}
myFunc(); // 1
myFunc(); // 2
myFunc(); // 3
```

## 三、真题
写出如下程序的输出结果：
```PHP
$count = 5;
function  get_count()
{
    static $count; // static 修饰的变量是局部的【仅在函数内部有效】；
    return $count++;
}
echo $count; // 5
++$count; // 6
echo get_count(); // null
echo get_count(); // null++ = 1
```