---
title: 【PHP 运算符】foo()和 @foo()之间的区别
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-05 20:39:27
password:
summary:
tags: 
    - PHP面试
    - PHP基础
    - 运算符
categories: PHP
---
## 一. 运算符考点
### 1. PHP的运算符的错误控制符@的使用及其作用
> PHP支持一个错误控制符：@。当将其放置在一个PHP表达式之前，该表达式可能产生的任何错误信息都被忽略掉。
### 2. 延伸：PHP所有运算符考点
#### 1） PHP运算符优先级（从高到低）
- 递增 / 递减
- !
- 算术运算符(+-*/)
- 大小比较
- （不）相等比较
- 引用
- 位运算符(^)
- 位运算符(|)
- 逻辑与(&&)
- 逻辑或(||)
- 三目(?:)
- 赋值($a = false)
- and
- xor
- or

**注：括号的使用可以增加代码可读性，推荐使用**

#### 2） 比较运算符：== 和 ===的区别

- == 比较值是否相等； === 比较值是否相等，类型是否相同。
- 等值判断（FALSE的七种情况）

```php
var_dump(''== false);// bool(true)
var_dump('0' == 0);// bool(true)
var_dump(0.0 == 0);// bool(true)
```

FALSE的七种情况都满足:
- 整型 0
- 浮点型 0.0
- 零字符串 '0'
- 空字符串 '' "
- 空数组 array()
- null
- 布尔型 false

#### 3） 递增/递减运算符

- 递增/递减运算符不影响布尔值；
    - true++; // true
    - true–; // true
    - false++ ; // false
    - false–; // false
- 递减NULL值没有效果；递增NULL值为1；
    - NULL–; // NULL
    - NULL++; // 1
- 递增和递减在前，就先运算，后返回；反之就先返回，后运算

#### 4） 逻辑运算符
1. 短路作用

```php
var_dump($a = true || $b == 3); // bool(true) 前面是 true，后面不会执行【|| : 一真为真】
var_dump($b = false && $a == 1);// bool(false) 前面是 false，后面不会执行【&&：一假为假】
```

2. `||` 和 `&&` 与 `or` 和 `and` 的优先级不同

```php
$a = false || true; // 先执行 false || true，得到 true,再赋值给 $a
var_dump($a); // bool(true)
$b = false or true;// 先执行 $b = false，整体为 true，则 $b的值为 false
var_dump($b);// bool(false)
```

## 二. 解题方法
> 重点记忆：**递增/递减**运算符的**运算规则**，逻辑运算符的 **短路效果**，在看到逻辑运算符要多考虑 **优先级** 问题。

## 三. 真题
**下列程序中，请写出打印输出的结果：**
```php
$a = 0;
$b = 0;

if ($a = 3 > 0 || $b = 3 > 0) {
    var_dump($a); // bool(true)
    var_dump($b); // int(0)
    $a++;
    $b++;
    echo $a."\n"; // 1
    var_dump($a); // bool(true)
    echo $b."\n"; // 1
    var_dump($b); // int(1)
}
```
**分析：**
```
1）优先级问题：(从大到小)
	>
	||
	=
2）执行顺序
	3 > 0; 					// true
	(3>0) || $b = 3 > 0；	// true ($b = 3 > 0 不执行)
	$a = true;
	$b = 0;
3） 递增运算
	$a++; // $a = true; true++; => true  => 1
	$b++; // $b = 0; 0++; => 1
```
```php
$a = 0;
$b = 0;

if ($a = 3 > 0 || $b = 3 > 0) {
    var_dump($a); // bool(true)
    var_dump($b); // int(0)
    $a++;
    $b++;
    echo $a."\n"; // 1
    var_dump($a); // bool(true)
    echo $b."\n"; // 1
    var_dump($b); // int(1)
}
```

**运算结果：**
```php
$a = true;
$b = 1;
```