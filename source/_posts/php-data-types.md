---
title: PHP 常量及数据类型
top: false
cover: false
toc: true
mathjax: false
date: 2020-07-05 17:59:59
password:
summary:
tags: 
    - PHP面试
    - PHP基础
    - 数据类型
    - 常量
categories: PHP
---
## 数据类型
三大数据类型（标量，复合，特殊）
- 标量：
    - 字符串
    - 整型
    - 浮点型
    - 布尔型
    - NULL
- 复合：
    - 数组
    - 对象
- 特殊：
    - 资源

### 浮点类型

不能用于精确的相等的判断

```php
$a = 0.1;
$b = 0.7;

if ($a+$b == 0.8) { // fasle, 值为0.7999999
    echo "yes";
}

echo 'no'; // 答案为 no
```

### 布尔类型

```php
// false 的7种情况
0, 0.0, '', '0', false, [], null
```

### 数组类型

```php
// 超全局数据
$GLOBALS, $_GET, $_POST, $_REQUEST, $_SESSION, $_COOKIE, $_SERVER, $_FILES, $_ENV
// 注意点：$GLOBALS 包含了后面的所有，$_REQUEST包含了$_GET, $_POST和$_COOKIE,尽量少用$_REQUEST,安全性低.
$_SERVER['SERVER_ADDR'] // 服务器ip
$_SERVER['SERVER_NAME'] // 服务器名
$_SERVER['REQUEST_TIME'] // 请求时间
$_SERVER['QUERY_STRING'] // 路由？后的一串
$_SERVER['HTTP_REFERER'] // 上级请求页面，页面从那里过来的,如果通过网址直接访问就是空
$_SERVER['HTTP_USER_AGENT'] // 头信息中的USER_AGENT
$_SERVER['REMOTE_ADDR'] // 客户端的IP地址
$_SERVER['REQUEST_URI'] // ／index.php
echo $_SERVER['PATH_INFO']; // ...index.php/use/reg?status=1   use/reg
```

### NULL
直接赋值为NULL， 未定义的变量，unset的变量

### 常量
定义const和define， const更快是语言结构，define是函数,define不能定义类的常量，const可以。常量一经定义就不能修改和删除

### 预定义常量
```php
__FILE__ // 文件的路径名和文件的名称
__LINE__ // 所在行行号
__DIR__ // 所在目录
__FUNCTION__ //所在的函数体的函数名称
__CLASS__ // 类名
__TRAIT__ // trait名
__METHOD__ // 类名+方法名
__NAMESPACE__ // 命名空间
```