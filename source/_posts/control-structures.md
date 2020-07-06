---
title: 【PHP面试题】请列出3种PHP数组循环操作的语法，并注明各种循环的区别。
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-05 22:27:27
password:
summary:
tags:
    - PHP基础
    - PHP面试
    - 流程控制
categories: PHP
---
## 一. 流程控制考点

### 1. PHP的遍历数组的三种方式及各自区别

#### 1） 遍历数组的方式

- 使用 for循环；
- 使用 foreach循环；
- 使用 while、list()、each()组合循环；

#### 2） 区别

- for循环：只能遍历索引数组；
- foreach循环：可以遍历索引和关联数组；
- 联合使用list()、each() 和 while循环：可以遍历索引和关联数组；
- while、list()、each()组合不会 reset()操作【即不会将数组的指针重置】；
- foreach遍历会对数组进行reset()操作。

### 2. 延伸：分支结构

#### 1） if …elseif：

- 在 elseif 语句中 只能有一个表达式为 true，即 在 elseif 语句中 只能有一个语句块被执行，多个 elseif从句是排斥关系；
- 使用 elseif 语句 有一个基本原则： 总是把优先范围小的条件放在前面处理。【即把可能性大的放到前面】

#### 2） switch…case…：

- 和 if 不同的是，switch 后面的控制表达式的数据类型只能是整型、浮点型或者字符串；
- continue 语句 作用到 switch 的作用类似于 break ；
- 跳出 switch 外的循环( for(){switch(){ case ...: ... continue2; }})，可以使用 continue2；
- 底层原理：switch … case 会生成跳转表，直接跳转到对应 case (不会一层一层去判断)；
- 效率：如果条件比一个简单的比较要复杂得多，或者在一个很多次的循环中，那么用 switch 语句可能会快一些

代码执行：

```PHP
for() {
    switch($var) {
        case...:
        	break; // 等价于 continue;【如果需要其作用于 for循环，此处应为 continue2（跳出两层循环）】
        case...:
        	break;
        case...:
        	break;
        default: ...:
        	break;
	}
}
```

## 二. 解题方法
> 注：理解循环内部机制，更易于记忆 **foreach的reset** 特性，分支结构中理解了 **switch…case** 的执行步骤也就不难理解为什么效率高了。

## 三. 真题：
### PHP中如何优化多个 if...else 语句的情况？
- 表达式的可能性越大，越往前放；
- 如果判断的是一个比较复杂的结构，且判断的结构是整型、浮点型或字符串类型，则可以使用 switch…case 来进行替换，此时效率会提升。