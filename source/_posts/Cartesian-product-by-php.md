---
title: 笛卡尔积算法（PHP实现）
top: false
cover: false
toc: true
mathjax: true
date: 2020-08-01 17:01:14
password:
summary:
tags:
    - 数组
    - 数学
    - PHP
categories: 数组
---
## 第一、定义
笛卡尔乘积是指在数学中，两个集合X和Y的笛卡尔积（Cartesian product），又称直积，表示为X×Y，第一个对象是X的成员而第二个对象是Y的所有可能有序对的其中一个成员

![笛卡尔积](https://bkimg.cdn.bcebos.com/pic/2934349b033b5bb57f0eb50b36d3d539b700bc6e?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U5Mg==,g_7,xp_5,yp_5)

假设集合A={a, b}，集合B={0, 1, 2}，则两个集合的笛卡尔积为{(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}。

类似的例子有，如果A表示某学校学生的集合，B表示该学校所有课程的集合，则A与B的笛卡尔积表示所有可能的选课情况。A表示所有声母的集合，B表示所有韵母的集合，那么A和B的笛卡尔积就为所有可能的汉字全拼。

设A,B为集合，用A中元素为第一元素，B中元素为第二元素构成有序对，所有这样的有序对组成的集合叫做A与B的笛卡尔积，记作AxB.
笛卡尔积的符号化为：

> A×B={(x,y)|x∈A∧y∈B}
> 例如，A={a,b}, B={0,1,2}，则
> A×B={(a, 0), (a, 1), (a, 2), (b, 0), (b, 1), (b, 2)}
> B×A={(0, a), (0, b), (1, a), (1, b), (2, a), (2, b)}

## 第二 、运算
1. 对任意集合A，根据定义有
> AxΦ =Φ , Φ xA=Φ

2. 一般地说，笛卡尔积运算不满足交换律，即
> AxB≠BxA（当A≠Φ ∧B≠Φ∧A≠B时）

3. 笛卡尔积运算不满足结合律，即
> (AxB)xC≠Ax(BxC)（当A≠Φ ∧B≠Φ∧C≠Φ时)

4. 笛卡尔积运算对并和交运算满足分配律，即
> Ax(B∪C)=(AxB)∪(AxC)
> (B∪C)xA=(BxA)∪(CxA)
> Ax(B∩C)=(AxB)∩(AxC)
> (B∩C)xA=(BxA)∩(CxA)

## 第三 、程序算法
```PHP
<?php

class Cartesian {
    private $matrix = [];

    /**
     * @param array $matrix
     */
    public function setMatrix (array $matrix): void {

        $this->matrix[] = $matrix;

    }

    public function exportData () {
        if (!$this->matrix) {
            return [];
        }
        $list = [];
        foreach ($this->matrix as $key => $value) {

            $this->logic($value, $list);
        }
        return $list;
    }

    public function logic ($item, &$list = []) {
        if (!$list) {
            foreach ($item as $value) {
                $temp = [];
                $temp[] = $value;
                array_push($list, $temp);
            }
            return "";
        }
        $forupdate = [];
        foreach ($list as $v) {
            $_temp = $v;
            foreach ($item as $i) {
                $__temp = $_temp;
                array_push($__temp, $i);
                $forupdate[] = $__temp;
            }
        }
        $list = $forupdate;

    }
}

// 使用方法

    $cartesian = new Cartesian();
    
    $cartesian->setMatrix(['红色','黄色',"蓝色"]);
    $cartesian->setMatrix(['M','L','XL','XXL']);
    $cartesian->setMatrix(['北京','郑州']);
    $cartesian->setMatrix(['男','女']);
    
    $dd =  $cartesian->exportData();
    print_r($dd);

```