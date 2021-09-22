--- 
layout:     post
title:      "Hello"
subtitle:   " \"leetcode 673\""
category:  leetcode
author:     "iamjing66"
tags:
    "leetcode"
--- 
# Leetcode 673 python

<hr>        

`记录`        
`leetcode 637`      
> 给定一个未排序的整数数组，找到最长递增子序列的个数。        
> 示例 1:     
> 输入: [1,3,5,4,7]       
> 输出: 2         
> 解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
>       
> 示例 2:     
> 输入: [2,2,2,2,2]       
> 输出: 5         
> 解释: 最长递增子序列的长度是1，并且存在5个子序列的长度为1，因此输出5。        
> 注意: 给定的数组长度不超过 2000 并且结果一定是32位有符号整数。

`自解`
```python
from typing import List


def findNumberOfLIS(nums: List[int]) -> int:
    l1 = [[nums[0]]]
    x = 0
    for i in nums[:-1]:
        if i > l1[x][-1]:
            l1[x].append(i)
        else:
            l1.append([i])
            x += 1
    return len(l1)
```     
测试用例`[1, 2, 4, 3, 5, 4, 7, 2]`报错
应输出 `3`, 程序输出`4`，不理解三组后的 2 不为子序列的原因，故记录。

<hr>        

##### 时间: 2021-9-22 16:20:24
##### 作者: **iamjing66**
