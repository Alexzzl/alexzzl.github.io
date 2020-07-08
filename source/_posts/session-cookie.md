---
title: 【PHP面试题】简述 cookie 和 session的区别及各自的工作机制，存储位置等，简述 cookie 的优缺点。
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-08 11:29:32
password:
summary:
tags: 
    - PHP基础
    - PHP面试
    - Session
    - Cookie
    - 会话控制
categories: PHP
---
## 一、考点
### 1、PHP的会话控制技术
#### 1） 为什么要使用会话控制技术？
> - 本身我们的Web是通过 HTTP协议来实现的，而HTTP协议是无状态协议（即HTTP协议没有一个内件机制来维护两个事物之间的状态），所以同一个用户在请求相同的页面两次的时候，HTTP协议不会认为这两次请求都来自于同一个用户，会把它们当做是两次请求的独立（即会将两次请求隔离开），会认为是两个不同的用户请求的。如果用户执行了登录操作，再次请求页面，HTTP协议不会认为该用户之前登录过，因为它无法保持该用户之前的登录状态，所以无法在不同页面之间进行用户的跟踪和状态的保持。
> - 而对于我们的会话控制技术来说，就是为了解决这样的问题。所以会话控制技术的思想是：允许服务器跟踪同一个客户端做出的连续请求。（即之前做过登录，下一次再进行请求的时候，就会知道之前已经登录过，不需要再登录，这一可以保持用户的状态，从而完成登录状态的保持。）

#### 2） 会话控制技术的实现方式
① 通过 GET 参数传递
> 如：在 页面一 中做了一个登录，用户的登录信息已经存在，跳到 页面二 中的时候，继续把刚才的登录信息传到 页面二 中，这个时候服务端在进行处理的时候，就会发现该用户刚刚在 页面一 中已经登录过了。这种方式可以通过GET参数进行传递，但是这样会有一些问题。

通过GET参数传递可能存在的问题：
> - 信息不安全，都暴露在了URL地址栏中； 
> - 在传递的过程中，可能会出现参数丢失的现象，这样登录信息也会被丢失。

因此，通过GET方式传递这种方法不推荐使用

② Cookie
**工作原理**
> cookie的工作原理：是一种由服务器发送给客户端的片段信息，存储在客户端浏览器的内存或硬盘当中。【可以把它当做是存储在浏览器中的一个文件，这个文件中包含我们客户端的一些片段的信息（如：登录或存储的一些其他信息等），它就是一个文件，这个文件是**存储在客户端的！！！**】

**举例说明**
> 用户去超市购买东西，购买完之后办理了会员卡，这个过程中：
>   - 超市 可以理解成是 服务器，
>   - 用户 可以理解成是 客户端，
>   - 会员卡 可以理解成时 Cookie。

即存储在客户端中的文件，就是会员卡，这张会员卡中存储着用户的基本信息，这些基本信息就是用户的状态信息。（如：我们做登录，就可以把登录的状态保存在 cookie的文件里）

**Cookie 的操作**
```PHP
/* 
 * 写操作
 * $name 	键名
 * $value	值
 * $expire	过期时间
 * $path    有效的服务器路径
 * $domain  有效的域名/子域名
 * $secure  设置该cookie是否仅通过安全的HTTPS连接传给客户端
 */
setcookie($name, $value, $expire, $path, $domian, $secure);

// 读取 cookie
$_COOKIE;

// 设置 cookie
setcookie('a[b]', 'val');

// 删除 cookie
setcookie($name, '', time()-1000); // 此处减去多少都可以，只要保证cookie过期，就表示已经删除
```

- Cookie 的优点和缺点
    - **cookie的优点：**因为cookie是将信息存储在客户端，因此不会占用服务器的资源（即不会浪费服务器的资源），效率会高一些。
    - **cookie的缺点：**也是因为其存储在客户端，由于我们的信息全部保存在客户端计算机中，所以不建议将一些敏感重要的数据保存到cookie当中，而且用户有权限禁止cookie的使用。如果我们在浏览器中，将cookie禁止掉，一旦用户禁止cookie，我们没有办法去保存用户的信息。

③ Session
> session的工作原理：将使用者相同的资料存储在服务器中，这样用户无法禁用session的使用。

注：session 并不是完全脱离 cookie的，而是基于 cookie的。

**举例说明**

> 用户去超市购买东西，购买之后超市给用户办了会员卡，会员卡是一个电子卡，只要输入用户的身份证号就可以使用该会员卡进行打折优惠。在此过程中：
> - 超市 -> 服务器
> - 用户 -> 客户端
> - 会员卡 -> 由于没有实质的会员卡，我们拿到的是一个电子卡，会员卡信息是用户状态的信息，该信息是 存储在服务器的

> 与cookie不同的是，电子卡是由服务器端进行保存的，是由超市进行保存的。用户每次来购物，只需要报自己的身份证号就可以了，该身份证号，叫 sessionId，而这个 sessionId 是存储在 cookie 中的。如果 cookie 被禁用，可以通过 URL 来传递 sessionId 的形式保存 session的状态。

> 因此 session 是基于 cookie 的，session 的信息都存储在服务器中的一个文件中，不存储于客户端文件中。每当用户想去读取 session 的内容时，首先会去拿到浏览器中携带 sessionId 的 cookie， 我们根据这个 sessionId 找到对应的 session 文件，将里面的内容读取出来。

- **Session 的操作**

```PHP
// 开启 session
session_start();

// 使用 session
$_SESSION;

// 销毁 session【将session中的内容情况】
$_SESSION = []; // 或者 $_SESSION = null;
session_start();
if(isset($_SESSION['views']))
{
    unset($_SESSION['views']);
}

// 删除 session 文件, 彻底销毁 session
session_destroy(); // 删除 session文件，且将 sessionId 对应的 cookie文件一并删除掉
```

- **Session 的配置**
```
在 php.ini 中有一些关于 session 的配置：
1) session.auto_start	  // 自动开启 session
2) session.cookie_domian  // 存储 sessionId 的那个 cookie 的有效域名
3) session.cookie_lifetime
4) session.cookie_path
5) session.name  // 默认PHPSESSID
6) session.save_path 	  // session 文件在服务器中的存储路径
7) session.use_cookies    // 是否使用 cookie 来传递 sessionId
8) session.use_trans_sid  // 是否可以使用传递的方式来传递 sessionId
9) session.save_handler   // session 存储的句柄是什么【可以将 session存储到 memcache/redis/mysql，可以在此处进行配置】
```

在 php.ini 中，还有关于 session 的三项配置，是用来配合使用，来进行垃圾回收的。
```
session.gc_probability; 
session.gc_divisor;
session.gc_maxlifetime;
```
> 如果用户直接关闭了浏览器，而不退出，会永久的保持在服务器中，这个时候，就需要去配置垃圾回收，会把一些 session 文件给清除掉，定时去清除。去配置我们的配置：
```
session.gc_probability = 1
session.gc_divisor = 100
session.gc_maxlifetime = 1440
```
**表示如果超过 过期时间（maxlifetime 最大生命周期）1440 秒，则 每 100 次 清理 1次。**

> 原理：
每 100次去调用 session_start()的时候，会有一次去清理文件，清理的是 当前时间的时间戳 - 文件最后的修改时间 > 1440s 的，说明该文件就已经过期，我们就将其清除掉。（如果将 session.gc_divisor = 1，每次去调用 session_start() 时，都会去清理一遍文件，清理一遍所有过期的 session文件）【此处不推荐将 session.gc_divisor 的值配置的特别小，如果配置的特别小，当去执行 session_start() 的时候，就会去消耗我们服务器的资源，会降低我们的效率】

- Session 优点和缺点
> - session的优点：信息非常的安全，都是存储在服务器端的，客户端不可能拿到 session 的数据。
> - session的缺点：会占用服务器的资源（session文件越来越多，可能某一天会占满磁盘），并且它的分布式也是一个问题（如：将来我们可能会有多台 web服务器，但是 session 可能是存储在其中一台，而另外一台是没有办法去使用的。这种情况下，我们可以使用 redis，不管在哪台服务器，都可以去调用 redis 的服务器，就可以达到信息共享)。

- 传递 SessionID 的问题【重点】
> 由于 session 是基于 cookie的，cookie 中存储了 sessionId，如果我们把 cookie 禁用掉，也就意外着我们的 sessionId 无法进行传递，即没有办法找到保持 session文件 的一个状态。
```HTML
&#60a href="1.php"&#62下个页面&#60/a&#62
```
> 让该标签跳转到 1.php 页面，默认情况下，跳转到下一个页面，sessionId 存储在 cookie中，到下一个页面的时候，可以从 cookie 中拿到 sessionId，然后去找 session 文件，将我们的登录信息读取出来。但如果把 cookie 禁用掉，此时 sessionId 无法取出，也即 session的信息也无法取出。

解决方法：
```HTML
// 只要用该方法进行传递，哪怕没有 sessionId，到下一个页面的时候，也会去服务器中找以 sessionId的值 为文件名 的文件，并将里面的信息读取出来

// 此处不可以用 PHPSESSION=sessionId的值这样的形式来写【错误写法】
&#60a href="1.php?PHPSESSION=sessionId的值"&#62下个页面&#60/a&#62

// 正确写法1
&#60a href="1.php?&#60?php echo session_name().'='.session_id(); ?&#62  "&#62 下个页面&#60/a&#62   

// 正确写法2
&#60a href="1.php?&#60?php echo SID; ?&#62"&#62下个页面&#60/a&#62 
// 常量 SID 就是 session_name() 和 session_id 的拼接。
```
SID 的特性：
> - 如果开启了 cookie，SID为空；
> - 如果未开启 cookie（cookie被禁用掉了），此时 SID才是 session_name() 和 session_id()字符串的拼接。

> 注：如果大家进行分析过，就不容易发现，sessionId的值 就是 session文件的名称，文件的默认名称为：sess_sessionid的值。

- Session存储

出现的问题

> 假设有5台 web服务器，有其中一台存储了 session，在登录的是否访问了5台中的一台，即登录的信息存储在了5台中的第一台服务器中，但是当跳转到下一个页面时，有可能就被轮询到了第二台服务器中，在第二台服务器上，我们去寻找 sessionId对于的那个 session文件的时候，无法找到。因为 session文件是在第一台服务器中存储着。

解决方法：
> 建议不要将 session以文件的形式进行存储，可以存储到我们的内存服务器中（如：memcache/redis/mysql）。
通过session_set_save_handler()存入 MySQL、Memcache、Redis等。

## 二、解题方法

充分理解 Session 和 Cookie 的工作原理

## 三、真题
### Session信息的存储方式，如何进行遍历？
存储方式：默认情况下存储到服务器的文件中，还可以通过 session_set_save_handler()存储到Mysql/Memcache/redis
遍历方式:
```PHP
$_SESSION;
```
