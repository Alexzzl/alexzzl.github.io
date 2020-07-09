---
title: JavaScript 基本知识
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-09 16:55:05
password:
summary:
tags:
    - 前端
    - JavaScript
    - JQuery
categories:
---
## JavaScript 基本语法
### 1） 变量的定义
> - 变量必须以字母开头；
> - 变量也能以 `$` 和 _ 符号开头；
> - 变量名称对大小写敏感；
> - 使用 var 关键字来声明变量；

**注意事项：**
> 可以在一条语句中声明很多变量（如： var a=1, b=2, c=3…）；
> 未使用值来声明的变量，值是 `undefined`(如：var a; 代表先声明一个变量，但是没有给它赋值，那该变量的值就是 `undefined`）；
> 如果重新声明 JavaScript变量，该变量的值不会丢失（如：var a=1；var a; 这样 a 不会被丢失，还是1）。

### 2） 数据类型 
> 字符串、数字、布尔、数组、对象、Null、Undefined

注：JavaScript变量 均为对象。当您声明一个变量时，就创建了一个新的对象。无论是哪种数据类型，都是以对象的形式存在的，在JavaScript中，一切皆对象。

### 3） 创建对象
> - new Object()；
> - 使用对象构造器；
> - 使用 JSON对象。

### 4） 函数
> - 定义方法；
> - 无默认值；
> - 函数内部声明的变量（使用 var）是局部变量；
> - 在函数外声明的变量是全局变量，所有脚本和函数都能访问它。

### 5） 运算符
> + 号 可以用来 字符串的拼接

### 6） 流程控制
> else if 必须分开写

### 7）JavaScript内置对象
① Number
```JavaScript
// 定义方法1：【使用最多】
var pi = 3.14;
// 定义方法2
var myNum = new Number(value);
// 定义方法3
var myNum = Number(value);
```
② String
```JavaScript
// 定义方法1
var str = 'This is String'; // 可以使用单引号或者双引号来定义【在JS中，单双引号基本没有区别】
// 定义方法2
var str = new String(s);
// 定义方法3
var str = String(s);
```
String 拥有一些 [方法和属性](https://www.w3school.com.cn/jsref/jsref_obj_string.asp)。


③ Boolean
```JavaScript
// 定义方法1
var bol = true;
// 定义方法2
var bol = new Boolean(value);
// 定义方法3
var bol = Boolean(value);
```
Boolean 拥有一些 [方法和属性](https://www.w3school.com.cn/jsref/jsref_obj_boolean.asp)。

④ Array
```JavaScript
// 定义方法1
var arr = new Array();
// 定义方法2
var arr = new Array(size); // 将数组的长度放进去
// 定义方法3
var arr = new Array(e1, e2, e3, ..., en); // 将数组的元素放进去
```
注：Array 拥有一些 [方法和属性](https://www.w3school.com.cn/jsref/jsref_obj_array.asp)。与PHP不同的是 JavaScript中的数组没有关联数组，即通常情况下，下标不允许为字符串。如果要定义像PHP那样的关联数组，要使用 JSON对象。

⑤ Date

```JavaScript
var date = new Date();
```
Date 也拥有一些 [方法和属性](https://www.w3school.com.cn/jsref/jsref_obj_date.asp)。


常考：如何使用 JS 来获取当前客户端的时间？
```JavaScript
var myDate = new Date();
myDate.getYear();        //获取当前年份(2位)
myDate.getFullYear();    //获取完整的年份(4位,1970-????)
myDate.getMonth();       //获取当前月份(0-11,0代表1月)
myDate.getDate();        //获取当前日(1-31)
myDate.getDay();         //获取当前星期X(0-6,0代表星期天)
myDate.getTime();        //获取当前时间(从1970.1.1开始的毫秒数)
myDate.getHours();       //获取当前小时数(0-23)
myDate.getMinutes();     //获取当前分钟数(0-59)
myDate.getSeconds();     //获取当前秒数(0-59)
myDate.getMilliseconds();    //获取当前毫秒数(0-999)
myDate.toLocaleDateString();     //获取当前日期
var mytime=myDate.toLocaleTimeString();     //获取当前时间
myDate.toLocaleString( );        //获取日期与时间
```

⑥ Math
```JavaScript
var pi_value = Math.PI; // 不需要 new ，可以直接使用
var sqrt_value = Math.sqrt(15); // 求平方根
```
Math 也拥有一些 [方法和属性](https://www.w3school.com.cn/jsref/jsref_obj_math.asp)。

⑦ RegExp正则表达式【非常重要】
```JavaScript
// 使用方法1
/pattern/attributes; // 正则表达式，不需要加引号
// 使用方法2
new RegExp(pattern, attributes); 
```
RegExp 也拥有一些 [方法和属性](https://www.w3school.com.cn/js/js_obj_regexp.asp)，其中包含正则表达式的分割、查找、匹配、替换。

### 8） Window 对象

- [Window](https://www.w3school.com.cn/js/js_window.asp)【常考】
- [Navigator](https://www.w3school.com.cn/js/js_window_navigator.asp)
- [Screen](https://www.w3school.com.cn/js/js_window_screen.asp)
- [History](https://www.w3school.com.cn/js/js_window_history.asp)【常考】
- [Location](https://www.w3school.com.cn/js/js_window_location.asp)【常考】

### 9） DOM 对象
- [Document 对象](https://www.w3school.com.cn/js/js_htmldom_document.asp)
- [Element  元素](https://www.w3school.com.cn/js/js_htmldom_elements.asp)
- [Attr     属性](https://www.w3school.com.cn/js/js_htmldom_elements.asp)
- [Even     事件](https://www.w3school.com.cn/js/js_htmldom_events.asp)

### 10) jQuery基础知识
① jQuery 选择器
- 基本选择器
- 层次选择器
- 过滤选择器
- 可见性过滤选择器
- 属性过滤选择器
- 子元素过滤选择器
- 表单对象属性过滤选择器

② jQuery 事件
```JavaSript
$("p").show();
```

④ jQuery DOM 操作
```JavaSript
操作属性、值、节点、CSS、尺寸
```

牢记 以上基础知识点，比较常考察的是 JavaScript的HTML样式操作以及 jQuery的选择器 和 事件、样式操作。

## 真题
### 1、JavaScript中为Id是test的元素，设置样式为good。
```JavaScript
document.getElementById('test').className = 'good';
```

### 2、要求使用jQuery事件写在页面元素加载完成之后，动态绑定 click 事件到 btnOK 元素。
```JavaScript
$(function (){
    $(".btnOK").click(function (){
        // .....
    });
});
```
