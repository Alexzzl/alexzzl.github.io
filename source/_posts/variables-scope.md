---
title: PHP 自定义函数及内部函数考察点
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
## 四、延伸考点
### 1） 函数的参数及参数的引用传递
默认情况下，函数参数通过值传递。如果希望允许函数修改它的值，必须通过引用传递参数(`&`)

```PHP
$a = 1;
function myFun(&$a) 
{
    global $a = 2;
}
myFun($a); // 只能传递变量
echo $a; // 2
```

### 2）函数的返回值
- 值通过使用可选的返回语句( return )返回
- 可以返回包括数组和对象的任意类型
- 返回语句会终止函数执行，将控制权交回函数调用处
- 省略return,返回值为NULL，不可有多个返回值(如果想返回多个值，可以返回一个数组)


### 3）函数的引用返回
从函数返回一个引用，必须在函数声明和指派返回值给一个变量时都使用引用运算符 &

```PHP
function &myFunc()
{
    static $b = 10;
    return $b;
}
$a = myFunc();//10
$a = &myFunc();//$a 指向了 $b
$a = 100;//$b 也变成了 100
echo myFunc();//100
```

### 4）外部文件的导入
- include/require语句包含并运行指定文件
- 如果给出路径名则按照路径来找，否则从include_path中查找
- 如果include_path中也没有，则从调用脚本文件所在的目录和当前工作目录下寻找
- 当一个文件被包含时，其中所包含的代码继承了include所在行的变量范围。
- 加载过程中未找到文件则include 结构会发出一条警告(E_WARNING)，脚本会继续运行；这一点和require不同，require会发出一个致命错误(E_COMPILE_ERROR)导致脚本中止
- require(include)/require_one(inclue_once) 唯一区别是 PHP 会检查该文件是否已经被包含过，如果是则不会再次包含。

#### 5）系统内置函数
1. 时间日期函数
    - data()   格式化本地日期和时间，并返回格式化的日期字符串
    - strtotimme()  获取指定日期的unix时间戳
    - mktime()  返回一个日期的 UNIX 时间戳，然后使用它来查找该日期的天
    - time()  返回当前时间的 Unix 时间戳，并格式化为日期
    - microtime()   函数返回当前 Unix 时间戳的微秒数
    - date_default_timezone_set()  设置默认时区

2. ip处理函数
    - ip2long()  将四个字段以点分开的IP网络址协议地址转换成整数
    - long2ip()  用于将一个数字格式的IPv4地址转换成字符串格式

3. 打印处理函数
- echo()： 
> 可以一次输出多个值，多个值之间用逗号分隔。echo是语言结构(language construct)，而并不是真正的函数，因此不能作为表达式的一部分使用。
- print()： 
> 简单类型变量的值(如int,string) 函数print()打印一个值（它的参数），如果字符串成功显示则返回true，否则返回false。
- print_r()
> 可以打印出复杂类型变量的值(如数组,对象) ,可以把字符串和数字简单地打印出来，而数组则以括起来的键和值得列表形式显示，并以Array开头。但print_r()输出布尔值和NULL的结果没有意义，因为都是打印"\n"。因此用var_dump()函数更适合调试。打印关于变量的易于理解的信息,如果给出的是 string、integer 或 float，将打印变量值本身。如果给出的是 array，将会按照一定格式显示键和元素。object 与数组类似。 记住，print_r() 将把数组的指针移到最后边。使用 reset() 可让指针回到开始处。
- var_dump()
> 此函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。
判断一个变量的类型与长度,并输出变量的数值,如果变量有值输的是变量的值并回返数据类型。此函数显示关于一个或多个表达式的结构信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。
- var_export()
> 输出或返回一个变量的字符串表示,此函数返回关于传递给该函数的变量的结构信息

您可以通过将函数的第二个参数设置为 TRUE，从而返回变量的表示。是其返回的表示是合法的 PHP 代码。
```
var_dump和print_r的区别：
var_dump返回表达式的类型与值而print_r仅返回结果，相比调试代码使用var_dump更便于阅读。
 
var_dump和var_export的区别：
var_export() 函数返回关于传递给该函数的变量的结构信息，是合法的 PHP 代码，可以通过将函数的第二个参数设置为 TRUE，从而返回变量的表示
var_dump() 打印变量的相关信息
 
printf():根据格式进行输出
sprintf():根据格式转换字符串，并返回
```
    
#### 6）序列化和反序列化函数
- [serialize](https://www.php.net/manual/zh/function.serialize.php) ( mixed $value ) : string  产生一个可存储的值的表示

返回字符串，此字符串包含了表示 value 的字节流，可以存储于任何地方。 

- [unserialize](https://www.php.net/manual/zh/function.unserialize.php) ( string $str ) : mixed  从已存储的表示中创建 PHP 的值

对单一的已序列化的变量进行操作，将其转换回 PHP 的值。


```PHP
$a = array('a' => 'Apple' ,'b' => 'banana' , 'c' => 'Coconut');   
 
//序列化数组   
$s = serialize($a);  
echo $s;  
//输出结果：a:3:{s:1:"a";s:5:"Apple";s:1:"b";s:6:"banana";s:1:"c";s:7:"Coconut";}   
echo '<br /><br />';  
  
//反序列化  
$o = unserialize($s);  
print_r($o);  
//输出结果 Array ( [a] => Apple [b] => banana [c] => Coconut ) 

```

json_encode 和 json_decode
```PHP
$a = array('a' => 'Apple' ,'b' => 'banana' , 'c' => 'Coconut');
//序列化数组
$s = json_encode($a);
echo $s;
//输出结果：{"a":"Apple","b":"banana","c":"Coconut"}
echo "\n";
 
//反序列化
$o = json_decode($s);
//输出结果 Array ( [a] => Apple [b] => banana [c] => Coconut ) 
```

#### 7）字符串处理函数
- implode（） 把数组元素组合为一个字符串
- explode（） 把字符串打散为数组
- join（） 把数组元素组合为一个字符串
- strrev（） 反转字符串
- trim（）移除字符串两侧的字符
- ltrim（）移除字符串左侧的字符
- rtrim（）移除字符串右侧的字符
- strstr（）函数搜索字符串在另一字符串中是否存在，如果是，返回该字符串及剩余部分，否则返回 FALSE
- number_format（）通过千位分组来格式化数字

#### 8）数组处理函数
- array_keys（）返回包含数组中所有键名的一个新数组
- array_value（）返回数组中所有的值（不保留键名）
- array_diff（）比较两个数组的键值，并返回差集
- array_intersect（）比较两个数组的键值，并返回交集
- array_merge（） 把两个数组合并为一个数组
- array_shift（）删除数组中的第一个元素，并返回被删除元素的值
- array_unshift（）函数用于向数组插入新元素。新数组的值将被插入到数组的开头
- array_pop（）函数删除数组中的最后一个元素
- aray_push（）函数向数组尾部插入一个或多个元素
- sort（） 函数对索引数组进行升序排序
- rsort() 函数对索引数组进行降序排序