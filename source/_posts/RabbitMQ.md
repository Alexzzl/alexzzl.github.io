---
title: RabbitMQ
top: false
cover: false
toc: true
mathjax: true
date: 2021-01-08 17:02:06
password:
summary:
tags:
categories:
---
## RabbitMQ 的基本概念

> **RabbitMQ** 是一种消息队列，用于程序间的通信。形象地说 : MQ就像一个邮局，发送者将消息写入 **MQ**，**MQ** 负责把消息发送给接收者。RabbitMQ 可支持 `Java`, `PHP`, `Python`, `Go`, `JavaScript`, `Ruby` 等多种语言。本文主要介绍 RabbitMQ 的基本概念和模型。

## 术语

![file](https://cdn.learnku.com/uploads/images/201608/29/4430/7ZX96izcht.png)

* **生产者( producer )**

`在图中为 P，表示消息的发送者。`

* **交换机( exchanges )**

`在图中为 X, 生产者发过来的消息需要经过交换机，交换机将决定将消息放到哪些队列当中。`

* **队列（queue）**

`队列在图 1 中由红色矩形阵列表示，负责保存消息和发放消息。`

* **消费者（consumer）**

`在图中为 C，代表等待接收消息的程序。`

## 信息流

* **消息是怎么从生产者传递到消费者的呢？**

首先，生产者发送消息到交换机，同时发送一个 `key`，通过这个 `key`，交换机就知道该把消息发到哪个队列。随后交换机把消息发送到相应的队列中。由队列将消息发送给消费者。消费者监听某些队列，当有消息过来时，就立即处理消息。

* **提问**
 * **交换机是如何根据 key 来分配消息到队列？**
 * **队列怎样将消息发送给消费者？**

## **第一个问题**
* **RabbitMQ 的交换机有四种类型：`direct`, `topic`, `headers`, `fanout`**

* **fanout**

**fanout** 交换机就跟广播一样，对消息不作选择地发给所有绑定的队列。以图 1 为例，两个队列都将收到消息。

* **direct**

![file](https://cdn.learnku.com/uploads/images/201608/29/4430/VQHBbb0ekr.png)
`图 2 direct` 

在 `direct` 模式里，交换机和队列之间绑定了一个 `key`，只有消息的 `key` 与绑定了的 `key` 相同时，交换机才会把消息发给该队列。如图 2 所示，消息的 `key` 为 `orange` 时，消息将进入队列 `Q1` ; `key` 为 `black` 或者 `green` 时，消息将进入队列 `Q2`。若消息的 `key` 是其他字符串，被交换机直接遗弃。

![file](https://cdn.learnku.com/uploads/images/201608/29/4430/odw7uDotKl.png)
`图 3 多重绑定`

同时，交换机支持多重绑定，多个队列可以以相同的 `key` 与交换机绑定。如图 3 所示，当消息的 `key` 为 `black` 时，消息将进入 `Q1` 和 `Q2`

* **topic**

`topic` 模式可以理解为主题模式，当 `key` 包涵某个主题时，即可进入该主题的队列。`topic` 模式的 `key` 必须具有固定的格式：以 `.` 作为间隔的一串单词；比如：`quick.orange.rabbit`，`key` 最多不能超过 `255byte`。
交换机和队列的key可以以类似正则表达式的方式存在，有两种语法：

1. ** "\*" 可以替代一个单词  **
2. **"#" 可以替代 0 个或多个单词**

![file](https://cdn.learnku.com/uploads/images/201608/29/4430/xj4u5P913O.png)
`图4 topic`

图 4。图中，`Q1` 与交换机绑定的 `kye` 为：`“*.orange.*”`，故当消息的 `key` 为三个单词，且中间的单词为 `orange` 时，消息将进入 `Q1`。`Q2` 与`exchange` 绑定的 `key` 为 `”rabbit.#”`，当消息的 `key` 以 `rabbit` 开头时，消息将进入 `Q2` 。

* **headers**

官网没介绍这个模式呀，大概不常用吧。

## **第二个问题**

![file](https://cdn.learnku.com/uploads/images/201608/29/4430/zigekpwKxP.png)
`图5 Round-robin Dispatching`

* **循环发放（Round-robin dispatching）**

队列分发消息给消费者的方式采用循环发放。举例来说，若队列里有四个消息 `w, x, y, z`，则 `C1` 将得到消息 `z` 和 `x` , `C2` 将得到消息 `y` 和 `w` 。即每个消费者按顺序每人发一个消息。
注意，在这种分配方式下，消息其实在刚进入队列的时候就已经内定好将要被分发的消费者。即 `z, x` 一定是给 `C1` . `y, w` 一定是给 `C2` 。
这种方式存在一些隐患，如果 `z` 和 `x` 都是耗时的命令、`y` , `z` 都是简单的命令，`C1` 将不停地工作，而 `C2` 就比较空闲，造成资源浪费。

* **公平发放（fair dispatching）**

公平发放解决了上述问题。这种方式下，队列只会把消息给空闲的消费者。如果它看到某个消费者正忙，就查找下一个空闲消费者。

* **消息的确认（Message acknowledgment）**

若没有特别设定，消息一旦被队列分发给消费者，就被 `Rabbitmq` 从内存中删除。
在这种情况下，如果将一个正在处理消息的消费者强行关闭，那么，消息将未被完全处理，且 `RabbitMQ` 完全不知情。
为了解决上述问题，可以配置使得消息处理完后，向 `RabbitMQ` 返回一个 `Acknowledgment`。`RabbitMQ` 直到收到`Acknowledgment` 后，才将消息删除。
当消费者死亡时（its channel is closed, connection is closed, or TCP connection is lost），`RabbitMQ` 会知道这个消费者发生问题了，将重新发送消息给空闲的消费者。
消息没有 `TimeOut`，即使消费者处理很长很长时间，乃至无穷无尽，`RabbmitMQ` 也认为消费者正在处理。

> 其实，消息的确认是默认开启的，不需要特地设置。


