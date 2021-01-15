---
title: TCP和UDP的区别
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-08 16:35:40
password:
summary:
tags:
    - 网络基础
    - TCP
    - UDP
categories:
---
## TCP与UDP基本区别

TCP 传输控制协议（英语：Transmission Control Protocol，缩写：TCP）是一种面向连接的、可靠的、基于字节流的传输层通信协议，位于OSI模型的传输层。
UDP 用户数据报协议（英语：User Datagram Protocol，缩写：UDP；又称用户数据包协议）是一个简单的面向数据报的通信协议，位于OSI模型的传输层。

<table style="border-right: medium none; border-top: medium none; border-left: medium none; border-bottom: medium none; border-collapse: collapse" cellspacing="0" cellpadding="0" border="1">
<tbody>
<tr>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; border-top: windowtext 1pt solid; padding-left: 5.4pt; padding-bottom: 0cm; border-left: windowtext 1pt solid; width: 72.3pt; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>&nbsp;</div>
<div>&nbsp;</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; border-top: windowtext 1pt solid; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 93.3pt; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="124">
<div>TCP</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; border-top: windowtext 1pt solid; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 72.3pt; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>UDP</div></td></tr>
<tr>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; padding-bottom: 0cm; border-left: windowtext 1pt solid; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>是否连接</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 93.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="124">
<div>面向连接</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>面向非连接</div></td></tr>
<tr>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; padding-bottom: 0cm; border-left: windowtext 1pt solid; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>传输可靠性</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 93.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="124">
<div>可靠的</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>不可靠的</div></td></tr>
<tr>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; padding-bottom: 0cm; border-left: windowtext 1pt solid; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>应用场合</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 93.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="124">
<div>传输大量的数据</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>少量数据</div></td></tr>
<tr>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; padding-bottom: 0cm; border-left: windowtext 1pt solid; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>速度</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 93.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="124">
<div>慢</div></td>
<td style="border-right: windowtext 1pt solid; padding-right: 5.4pt; padding-left: 5.4pt; border-left-color: #ece9d8; padding-bottom: 0cm; width: 72.3pt; border-top-color: #ece9d8; padding-top: 0cm; border-bottom: windowtext 1pt solid; background-color: transparent" valign="top" width="96">
<div>快</div></td></tr></tbody></table>


- TCP与UDP基本区别
  - 基于连接与无连接
  - TCP要求系统资源较多，UDP较少； 
  - UDP程序结构较简单 
  - 流模式（TCP）与数据报模式(UDP); 
  - TCP保证数据正确性，UDP可能丢包 
  - TCP保证数据顺序，UDP不保证 
　　
- UDP应用场景：
  - 面向数据报方式
  - 网络数据大多为短消息 
  - 拥有大量Client
  - 对数据安全性无特殊要求
  - 网络负担非常重，但对响应速度要求高

- TCP协议和UDP协议特性区别总结：
  - TCP协议在传送数据段的时候要给段标号；UDP协议不
  - TCP协议可靠；UDP协议不可靠
  - TCP协议是面向连接；UDP协议采用无连接
  - TCP协议负载较高，采用虚电路；UDP采用无连接
  - TCP协议的发送方要确认接收方是否收到数据段（3次握手协议）
  - TCP协议采用窗口技术和流控制  