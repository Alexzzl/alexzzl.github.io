---
title: 面试常见问题与相关答案
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-14 21:44:38
password: 19961120
summary:
tags:
    - PHP面试
categories:
---
[PHP面试准备的资料](https://xianyunyh.gitbooks.io/php-interview/content/)


[细说浏览器输入URL后发生了什么](https://segmentfault.com/a/1190000012092552)

[简述 cookie 和 session的区别及各自的工作机制，存储位置等，简述 cookie 的优缺点。](https://alexzzl.github.io/2020/07/08/session-cookie/)

[php session 数据保存在哪里?](https://www.cnblogs.com/eoiioe/archive/2008/11/30/1344283.html)

Session 文件什么样的
```php
session_start();

$_SESSION['view'] = 1;
$_SESSION['name'] = "Alex";
var_dump($_SESSION);
/*
array(2) {
  ["view"]=>
  int(1)
  ["name"]=>
  string(4) "Alex"
}
*/
```
```Bash
➜  ~ sudo ls /var/lib/php/sessions                                 
sess_6bjte47jooqn8aoj3tbpuiufc7  sess_slchsjrrp843johcnenjl69lop
sess_kpievppouhsvh1fos7j66iie0k

➜  ~ sudo cat /var/lib/php/sessions/sess_kpievppouhsvh1fos7j66iie0k
view|i:1;%     

➜  ~ sudo cat /var/lib/php/sessions/sess_6bjte47jooqn8aoj3tbpuiufc7
view|i:1;name|s:4:"Alex";%      

```
[linux shell 替换字符串的几种方法，变量替换${}，sed，awk](https://blog.csdn.net/whatday/article/details/104963945)

```Bash
➜  ~ a='zhaoli_zhang@qq.com'
➜  ~ echo  ${a/qq/baidu}    
zhaoli_zhang@baidu.com
```

[PHP str_replace() 函数](https://www.w3school.com.cn/php/func_string_str_replace.asp)
```php
echo str_replace('world', 'Shangehai', 'Hello world!');
```

[preg_replace() 函数](https://www.runoob.com/php/php-preg_replace.html)

[PHP 字符串替换 substr_replace 与 str_replace 函数](https://www.cnblogs.com/mfryf/p/5013937.html)

[explode — 使用一个字符串分割另一个字符串](https://www.php.net/manual/zh/function.explode.php)
```php
explode ( string $delimiter , string $string , int $limit = ? ) : array


$pizza  = "piece1 piece2 piece3 piece4 piece5 piece6";
$pieces = explode(" ", $pizza);
echo $pieces[0]; // piece1
echo $pieces[1]; // piece2
```
[implode — 将一个一维数组的值转化为字符串](https://www.php.net/manual/zh/function.implode.php)
```php
implode ( string $glue , array $pieces ) : string
implode ( array $pieces ) : string

$array = array('lastname', 'email', 'phone');
$comma_separated = implode(",", $array);

echo $comma_separated; // lastname,email,phone

// Empty string when using an empty array:
var_dump(implode('hello', array())); // string(0) ""

```

[HTTP/1.1中，状态码 200 301 304 403 404 500 的含义。](https://alexzzl.github.io/2020/07/08/http/)

- 200(OK) : 从状态码发出的请求被服务器正常处理。
- 204(No Content) : 服务器接收的请求已成功处理，但在返回的响应报文中不含实体的主体部分【即没有内容】。
- 206(Partial Content) : 部分的内容（如：客户端进行了范围请求，但是服务器成功执行了这部分的干请求）。
- 301(Moved Permanently) : 跳转，代表永久性重定向（请求的资源已被分配了新的URI，以后已使用资源，现在设置了URI）。
- 302(Found) : 临时性重定向（请求的资源已经分配了新的URI，希望用户本次能够使用新的URI来进行访问）。
- 303(See Other) : 由于请求对应的资源存在的另一个URI（因使用get方法，定向获取请求的资源）。
- 304(Not Modified) : 客户端发送附带条件的请求时，服务器端允许请求访问资源，但因发生请求未满足条件的情况后，直接返回了 304。
- 307(Temporary Redirect) : 临时重定向【该状态码与302有着相同的含义】。
- 400(Bad Request) : 请求报文中存在语法错误（当错误方式时，需修改请求的内容后，再次发送请求）。
- 401(Unauthorized) : 发送的请求需要有通过HTTP认证的认证信息。
- 403(Forbidden) : 对请求资源的访问被服务器拒绝了。
- 404(Not Found) : 服务器上无法找到请求的资源。
- 500(Internal Server Error) : 服务器端在执行请求时发生了错误。
- 503(Service Unavailable) : 服务器暂时处于超负载或者是正在进行停机维护，现在无法处理请求。

[Laravel8和之前Laravel版本的区别](https://www.cnblogs.com/heyongzhen/p/13863980.html)

[干货预警，一篇文章带你彻底搞懂 Laravel 框架的运行原理！！！](https://learnku.com/articles/52852)

[PHP 完整实战 23 种设计模式](https://learnku.com/laravel/t/3522/php-complete-combat-23-design-patterns)

[PHP 设计模式系列](https://laravelacademy.org/books/php-design-pattern)

[走过的，路过的，快来看看laravel设计模式好文章的汇总！](https://segmentfault.com/a/1190000014449841)

[laravel框架中的设计模式-IOC模式](https://www.kancloud.cn/jdxia/booknote/581230)

[IoC模式（控制反转、依赖注入）](https://blog.51cto.com/quantum/1179124)

[深入 Composer autoload](https://learnku.com/php/t/1002/deep-composer-autoload)

[book note](https://www.kancloud.cn/jdxia/booknote/527018)

[php-fpm进程模型](https://www.easyswoole.com/Video/Basic/php-fpmProcessModel.html)


[MySQL的事务和锁](https://juejin.cn/post/6917477406483677191)

[内存泄漏和内存溢出有啥区别？](https://www.zhihu.com/question/40560123)

[10.swoole基础-常驻内存以及如何避免内存泄漏](https://www.cnblogs.com/JsonM/articles/7325018.html)

[websocket](https://www.ruanyifeng.com/blog/2017/05/websocket.html)

[Socket网络编程入门](http://blog.leanote.com/post/weibo-007/Socket%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B%E5%85%A5%E9%97%A8)

[MySQL 主从复制](https://github.com/doocs/advanced-java/blob/main/docs/high-concurrency/mysql-read-write-separation.md)

[Redis 和 Memcached 有什么区别？](https://github.com/doocs/advanced-java/blob/main/docs/high-concurrency/redis-single-thread-model.md)

[PHP The Right Way PHP之道](https://phptherightway.com/)
除了编码规范之外的各种 PHP 的最佳实践，还包括一些设计模式，安全问题，以及服务部署，Docker 虚拟化以及各种资源。

[PHP FIG](http://www.php-fig.org/psr/)，PHP 编码规范及标准推荐。
[PHP PSR 标准规范](https://learnku.com/docs/psr)

[Clean Code PHP](https://github.com/jupeter/clean-code-php)，《代码整洁之道》的 PHP 实践。

[TCP的那些事儿](https://coolshell.cn/?s=TCP%E7%9A%84%E9%82%A3%E4%BA%9B%E4%BA%8B%E5%84%BF)