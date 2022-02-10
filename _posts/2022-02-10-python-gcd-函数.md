--- 
layout:     post
title:      "Python gcd函数"
subtitle:   " \"python gcd function\""
category:  python
author:     "iamjing66"
tags:
    "python"
---  

# Python gcd函数学习

<hr>  

## 什么情况知道的gcd函数？

主要原因是`2022年2月10日`[leetcode每日一题](https://leetcode-cn.com/problems/simplified-fractions/)  
本题划分为中等难度，我认为的主要原因是：如何更加方便地找出公约数  
题目如下：

```text
给你一个整数 n ，请你返回所有 0 到 1 之间（不包括 0 和 1）满足分母小于等于  n 的 最简 分数 。分数可以以 任意 顺序返回。
* 示例 1：  
输入：n = 2
输出：["1/2"]
解释："1/2" 是唯一一个分母小于等于 2 的最简分数。  

* 示例 2：  
输入：n = 3
输出：["1/2","1/3","2/3"]  
解释："1/2"，"1/3" 和 "2/3" 都是唯一一个分母小于等于 3 的最简分数。  

* 示例 3：  
输入：n = 4
输出：["1/2","1/3","1/4","2/3","3/4"]
解释："2/4" 不是最简分数，因为它可以化简为 "1/2" 。
```  

看到题第一想法双循环处理分母和分子，通过寻找余数不为0的组 o(￣▽￣)ｄ  
But...

```text
input: n = 4
output: ['2/3', '3/4']
```

¿¿¿  
接着直接打开评论，看到了gcd函数，并且还不止一个解答使用，那么

## 什么是gcd函数？

维基百科: `最大公因数（英语：highest common factor，hcf）也称最大公约数（英语：greatest common divisor，gcd）是数学词汇，指能够整除多个整数的最大正整数。而多个整数不能都为零。例如8和12的最大公因数为4。整数序列{\displaystyle a}a的最大公因数可以记为{\displaystyle (a_{1},a_{2},\dots ,a_{n})}{\displaystyle (a_{1},a_{2},\dots ,a_{n})}或{\displaystyle \gcd(a_{1},a_{2},\dots ,a_{n})}{\displaystyle \gcd(a_{1},a_{2},\dots ,a_{n})}。`

python版本

```python
GCD = lambda a, b: (GCD(b, a % b) if a % b else b)


# or

def gcd(a, b):
    if a % b:
        retrun gcd(b, a % b)
    else:
        return b
```  

知道了这个方法以后就能够很方便地去除掉例如`2/4`这类分数

个人最终版：

```python
from math import gcd


class Solution:
    def simplifiedFractions(self, n: int) -> List[str]:
        l = []
        for i in range(2, n + 1):
            for j in range(1, i):
                if gcd(i, j) == 1:
                    x = f"{j}/{i}"
                    l.append(x)
        return l
```

一行版：

```python
from typing import List
from math import gcd


class Solution:
    def simplifiedFractions(self, n: int) -> List[str]:
        return [f"{j}/{i}" for i in range(2, n + 1) for j in range(1, i) if gcd(j, i) == 1]
```  

<hr>        

##### 时间: 2022-2-10 10:17:30

##### 作者: **iamjing66**
