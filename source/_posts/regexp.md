---
title: 【PHP面试题】正则表达式及手机号码的正则表达式。
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-06 16:26:45
password:
summary:
tags:
    - PHP面试
    - 正则表达式
categories:
---
## 一、考点：
### 1. 手机号码的正则表达式编写
### 2. 延伸：正则表达式组成及编写方法
####  1） 正则表达式的作用：分割、查找、匹配、替换字符串
####  2）正则表达式的组成部分：

① 分隔符：

    1) 正斜线（/）【推荐使用】；
    2) hash符号（#）；
    3) 取反符号（~）；

② 通用原子：

    1) \d: 0-9
    2) \D: 除了0-9
    3) \w: 数字、字母、下划线
    4) \W: 除了数字、字母、下划线
    5) \s: 空白符
    6) \S: 除了空白符

③ 元字符：

    1) . : 除了 换行符 之外的任意符
    2) * : 出现0次、1次或者多次
    3）? : 出现0或1次
    4) ^ : 必须以其开头
    5) $ : 必须以其结尾
    6) + : 出现1次或多次
    7) {n} : 恰巧出现n次
    8) {n,} : >= n次
    9) {n, m} : n <= 出现次数 <= m
    10) [] : 集合【如：[abc] 表示匹配 a或b或c】
    11) ()：互相引用，即匹配一个整体【如：(abc) 表示同时匹配abc】
    12) [^] : 取反【如：[^abc] 表示除了a/b/c】
    13) | : 或者
    14) [-] : 匹配一个范围【如：[0-9] 表示匹配0-9】

④ 模式修正符：

    1) i : 不区分大小写
    2) m : 将字符串通过分隔符进行分割【即将字符串中的每一行分别进行匹配】
    3) e : 在进行 preg_replace时，可以将匹配的内容进行PHP语法的处理【PHP7.0起废除】
    4) s : 修正圆点元字符(.)和换行
    5) U : 取消贪婪模式
    6) x : 忽略模式中的空白符
    7) A : 必须以该模式开头
    8) D : 修正 $对\n的忽略
    9) u : 当进行uft-8中文匹配的使用，可以使用

####  3） 后向引用：将前面匹配到的放到后面

```php
$str = '&#60b&#62abc&#60/b&#62';
$pattern = '/&#60b&#62(.*)&#60\/b&#62/'; // 正则表达式
echo preg_replace($pattern, '\\1', $str); 
 // 两个反斜线是为了防止将 \1 转义掉；
 // \1表示匹配第一个括号中的内容；
```

####  4） 贪婪模式
```php
$str = '&#60b&#62abc&#60/b&#62&#60b&#62bcd&#60/b&#62'; 
$pattern = '/&#60b&#62(.*)&#60\/b&#62/'; // 贪婪模式
echo preg_replace($pattern, '\\1', $str);  // abc&#60/b&#62&#60b&#62bcd
```
取消 贪婪模式 的方法：
① 使用 . * ? 取消贪婪模式
```php
$str = '&#60b&#62abc&#60/b&#62&#60b&#62bcd&#60/b&#62'; 
// 匹配每一个&#60b&#62标签中的内容
$pattern = '/&#60b&#62(.*?)&#60\/b&#62/'; // 匹配到了abc和bcd（从[b]开始，匹配到了abc，遇到[/b]结束；再次遇到[b]开始，匹配到了bcd，遇到[/b]结束）
echo preg_replace($pattern, '\\1'."\n", $str); 
// abc
// bcd
```
② 使用 . * 后面加 U 取消贪婪模式

```php
$str = '&#60b&#62abc&#60/b&#62&#60b&#62bcd&#60/b&#62';
$pattern = '/&#60b&#62(.*)&#60\/b&#62/U';
echo preg_replace($pattern, '\\1'."\n", $str); 
// abc
// bcd

```

两种方法不能同时使用

#### 5） 正在表达式PCRE函数
<table><tbody><tr><th>函数</th><th>描述</th></tr>
<tr><td>
<a href="https://www.runoob.com/php/php-preg_filter.html" target="_blank">preg_filter</a> </td><td> 执行一个正则表达式搜索和替换</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_grep.html" target="_blank">preg_grep</a> </td><td> 返回匹配模式的数组条目</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_last_error.html" target="_blank">preg_last_error</a> </td><td> 返回最后一个PCRE正则执行产生的错误代码</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_match_all.html" target="_blank">preg_match_all</a> </td><td> 执行一个全局正则表达式匹配</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_match.html" target="_blank">preg_match</a> </td><td> 执行一个正则表达式匹配</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_quote.html" target="_blank">preg_quote</a> </td><td> 转义正则表达式字符</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_replace_callback_array.html" target="_blank">preg_replace_callback_array</a> </td><td> 执行一个正则表达式搜索并且使用一个回调进行替换</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_replace_callback.html" target="_blank">preg_replace_callback</a> </td><td> 执行一个正则表达式搜索并且使用一个回调进行替换</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_replace.html" target="_blank">preg_replace</a> </td><td> 执行一个正则表达式的搜索和替换</td></tr><tr><td>
<a href="https://www.runoob.com/php/php-preg_split.html" target="_blank">preg_split</a> </td><td> 通过一个正则表达式分隔字符串</td></tr></tbody></table>

#### 6） 中文匹配

① UTF-8汉字编码范围是：0x4e00-0x9fa5； UTF-8要使用 u模式修正符 使模式字符串被当成 UTF-8。

```php
// 该中文为UTF-8下的中文
$str = '中文'; 
// UTF-8进行匹配
$pattern = '/[\x{4e00}-\x{9fa5}]+/u'; // 匹配一次或多次，不区分大小写
preg_match($pattern, $str, $match);
var_dump($match);
/* array(1) {
  [0]=>
  string(6) "中文"
}
*/
```
② 在ANSI(gb2312)环境下， 0xb0-0xf7, 0xa1-0xfe；在ANSI(gb2312)环境下，要使用chr将ASCII码转换为字符

```php
$str = '中文';
$pattern = '/['.chr(0xb0).'-'.chr(0xf7).']['.chr(0xa1).'-'.chr(0xfe).']/';
preg_match($pattern, $str, $match);
var_dump($match);
```
## 二、解题方法

> 1） 先写出一个要匹配的字符串；
2） 自左向右的顺序使用正则表达式的原子核元字符进行拼接；
3） 最终加入模式修正符；
4） 不可死记硬背模式；
5） 练习常见正则表达式（URL、Email、IP地址、手机号码等）。

## 三、真题
### 1. 至少写出一种验证 139手机号码的正则表达式。
```php
$str = '13988888888';
$pattern = '/^139\d{8}$/';
preg_match($pattern, $str, $match);
var_dump($match);
/*
array(1) {
  [0]=>
  string(11) "13988888888"
}
*/
```

### 2. 请写出一个正则表达式，取出页面中所有 img标签 中的 src值。

```php
$str = '&#60img alt="狗狗" id="dog" src="dog.jpg" /&#62';
$pattern = '/&#60img.*?src="(.*?)".*?\/?&#62/i';
preg_match($pattern, $str, $match);
var_dump($match);
/*
array(2) {
  [0]=>
  string(43) "&#60img alt="狗狗" id="dog" src="dog.jpg" /&#62"
  [1]=>
  string(7) "dog.jpg"
}
*/
```