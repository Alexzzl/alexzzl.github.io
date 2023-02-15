---
title: GitHub访问慢解决方法
top: false
cover: false
toc: true
mathjax: true
date: 2023-02-15 23:02:14
password:
summary:
tags:
categories:
---
GitHub访问慢或者无法访问一般是由以下问题引起的：

- 本地网络访问慢，科学上网速度很快
- 本地网络无法访问（响应时间过长导致无法访问）
- 由于github的加速分发CDN域名assets-cdn.github.com遭到DNS污染，无法访问

# 1. 修改本地hosts映射
> 域名解析直接指向GitHub的IP地址，以此来绕过DNS解析
## 1.1 查看最新GitHub 的最新ip地址
在 ip地址查询 网站中查询GitHub相关的网站对应的最新IP地址

- github.com
- github.global.ssl.fastly.net
- assets-cdn.github.com
- codeload.github.com

在当前网站中查询指定网站ip地址还可以使用另外方法:
①直接将网站作为参数进行请求，省略点击查询的步骤：

- websites.ipaddress.com/github.glob…
- websites.ipaddress.com/github.com
- websites.ipaddress.com/assets-cdn.…
- websites.ipaddress.com/codeload.gi…

②将ip查询网站拼接在之后进行查询

- github.global.ssl.fastly.net.ipaddress.com/
- github.com.ipaddress.com/
- assets-cdn.github.com.ipaddress.com/
- codeload.github.com.ipaddress.com/

## 1.2 本地hosts文件映射ip地址
找到对应的IP地址后，将IP地址与网站地址进行对应，并将对应关系写入本地hosts文件中。
在Windows系统中的 `C:\Windows\System32\drivers\etc` 下找到hosts文件，编辑打开，将四个网站的IP地址和网站地址对应写入进入，作为DNS的映射。
hosts文件直接编辑修改时可能没有权限，可以通过以下方法完成修改：

1. 修改当前文件权限，右键hosts文件 -> 属性 -> 安全 -> 编辑 -> Users -> Users的权限后加入写权限
1. 将当前文件复制到别的盘中，修改文件后复制回来覆盖原来文件

```
#github dns映射 格式如：  [ip]: [domainName]
199.232.69.194 github.global.ssl.Fastly.net
140.82.114.4 GitHub.com
185.199.108.153 assets-cdn.Github.com
140.82.114.9 codeload.Github.com
```

## 1.3 刷新DNS缓存来访问新的映射
hosts文件内容更新成功后，还需要刷新windows系统的DNS才可以生效。
使用 win+R ，打开cmd命令行，输入 `ipconfig/flushdns` 刷新DNS缓存即可。

刷新完成后，再次打开github网站时速度会明显提升，需要注意的是以上github网站的ip经常发生变化，如果访问再次变慢可以重新更新映射信息。
# 2. 一键更新
手动更新本地hosts文件的方式比较繁琐，我们可以编写程序来代替手动操作，实现需要时hosts文件内容的一键更新。
推荐一个github开源项目：[更新hosts](https://github.com/isevenluo/github-hosts) ，[国内git地址](https://gitee.com/isevenluo/github-hosts)，作者会每日提供最新的相关ip地址映射信息，我们可以直接复制使用或者使用其中的程序进行一键更新操作。
作为一个coder，我们也可以自己去实现一个脚本程序。

作者：东方甲乙木土
链接：https://juejin.cn/post/7019683061977579557
来源：稀土掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。