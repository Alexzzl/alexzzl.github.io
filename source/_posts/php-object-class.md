---
title: 【PHP面试题】PHP的类权限控制修饰符
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-08 16:21:23
password:
summary:
tags:
    - PHP基础
    - PHP面试
    - 面向对象
categories: PHP
---
## 一、考点
### 1、PHP的类权限控制修饰符
<table>
<thead>
<tr>
<th align="left"></th>
<th align="center">类外</th>
<th align="center">子类</th>
<th align="center">类内</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">public（公共的）</td>
<td align="center">√</td>
<td align="center">√</td>
<td align="center">√</td>
</tr>
<tr>
<td align="left">protected（受保护的）</td>
<td align="center">×</td>
<td align="center">√</td>
<td align="center">√</td>
</tr>
<tr>
<td align="left">private（私有的）</td>
<td align="center">×</td>
<td align="center">×</td>
<td align="center">√</td>
</tr>
</tbody>
</table>

### 2、延伸
#### 1） 面向对象的封装、继承和多态
① 封装
> 成员访问权限（即 public/protected/private的访问权限问题）；
② 继承
> - 单一继承的特性（只能同时继承一个父类，如果有接口的话，可以继承一个父类，一个接口）；
> - 方法重写（在父类中定义一个方法，子类去继承父类的时候，如果方法名相同，父类的方法会被子类覆盖掉，如果不想被覆盖，可以进行延伸，加一个 parent::方法名()）；

```php
class Father
{
    public function test()
    {
        
    }
}

class Son extends Father
{
    public function test()
    {
        parent::test();
        // 子类的方法体 ....
    }
}
```
③ 多态
- 抽象类的定义
> 在类的前面加 abstract；

```php
abstract class 类名
{
    
}
```
> 如果类中有抽象方法，该类必须定义为抽象类；如果类中没有抽象方法，也可以定义为抽象类。

- 接口的定义
> - 接口中的方法都为 抽象方法；
> - 用 interface 接口名 来定义；

```php
interface 接口名
{
    public function 方法名([参数]);
}
```
> 没有方法体，直接用 {} 即可（即定义好该方法，等着后代去实现）。

#### 2） 魔术方法
```php
__construct(); // 构造方法：在每次创建对象（即实例化对象的时候自动调用）
__destruct();  // 析构方法：在每次销毁的时候自动调用
__call();	   // 在对象中调用一个不可访问方法时，会被调用
__callStatic();// 在静态上下文中调用一个不可访问方法时，会被调用
__get();       // 读取不可访问属性的值时，会被调用
__set();       // 在给不可访问属性赋值时，会被调用
__isset();     // 当对不可访问属性调用 isset()或empty()时，会被调用
__unset();     // 当对不可访问属性调用 unset()时，会被调用
__sleep();     // 常用于未提交的数据，或类似的清理操作。同时如果有一些很大的对象，但不需要全部保存，这个功能就很好用。
__wakeup();	   // 经常用在反序列化操作中，例如重新建立数据库连接，或执行其它初始化操作。
__toString();  // 用于一个类被当成字符串时应怎样回应。（此方法必须返回一个字符串）
__clone();     // 当赋值完成时，如果定义了__clone()方法，则新创建的对象（复制生成的对象）中的__clone()方法会被调用【可用于修改属性的值（如果有必要的话）。】
```

#### 3） 设计模式
常见设计模式：
1. 工厂模式
2. 单例模式
3. 注册数模式
4. 适配器模式
5. 观察者模式
6. 策略模式

## 二、解题方法
> 1、着重记忆PHP面向对象的基本语法，记忆模式方法；
> 2、理解常见设计模式

## 三、真题
### 请写出PHP的构造函数和析构函数
```php
__construct(); // 构造函数
__destruct(); // 析构方法
```
**注：如果 `方法名`和 `类名` 一致，该方法也是构造函数。**