---
title: AJAX的基本工作原理
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-10 09:49:17
password:
summary:
tags:
categories:
---
## AJAX的基本工作原理
### 1） AJAX基本概念
> Asynchronous JavaScript and XML（异步的 JavaScript和XML）。通过在后台与服务器进行少量数据交换，AJAX可以使网页实现异步更新。

### 2） AJAX 工作原理
> - XMLHttpRequest 是 AJAX的基础，在使用AJAX的时候，一定要去使用XMLHttpRequest这个对象，AJAX是基于该对象的；
> - XMLHttpRequest 用于在后台与服务器交换数据。

① XMLHttpRequest 对象请求
```JavaSript
// 请求方式，请求地址，是否异步传送
open(method, url, async);
// 发送
send(string);
```

② XMLHttpRequest 对象响应
```JavaSript
responseText; // 接收 text内容
responseXML;  // 接收 XML内容
onreadystatechange; // 状态发生改变的时候调用
```

readState：
- 0（请求未初始化）；
- 1（服务器连接已建立）；
- 2（请求已接收）；
- 3（请求处理中）；
- 4（请求已完成，且响应已就绪）【响应完成】

status：200、400

## jQuery的AJAX操作
常用方法
```JavaScript
$(ele).load(); // 给我们一个元素直接去加载内容 	
$.ajax();
$.get();
$.post();
$.getJSON();
$.getScript();
```

## 真题
要求写出jQuery中，可以处理AJAX的几种方法。
```JavaScript
$.ajax();
$.get();
$.post();
$.getJSON();
$.getScript();
```