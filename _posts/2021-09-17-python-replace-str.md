--- 
layout:     post
title:      "一个解决Python字符串中替换多个值的方法"
subtitle:   " \"Python replace str\""
category:   tec
author:     "iamjing66"
tags:
    "python"
--- 

# Python 中替换字符串方法       
<hr>

### 记录一次工作中的学习      

`背景`        
在某项目中需要对接收过来的字符串进行转义处理

* `第一次`
```python
# exp:    
# str_dict 为转义字典    
str_dict = {
    "<2c>": ",",
    "<2e>": ".",
    "<2f>": "/",
    "<2d>": "-",
    "<2b>": "+",
    "<3d>": "=",
    "<2a>": "*",
    }
# s1 为需要进行转义的字符串        

s1 = "<2d>47"

# 第一次默认认为只有一个特殊字符所以函数为       

def str_replace1(source: str):
    target = source
    for k, v in str_dict.items():
        if k in s1:
            target = source.replace(k, v)
    return target

# output: "-47"    
```
上面例子中没有产生问题，但是若 s1 中包含多个特殊转义字符则会出错
```python
s1 = "<2d>46<2e>5"
# 需要的输出是 "-46.5"
# 若通过 str_replace1 函数
# output: "-46.5"
```
并不能输出需要的字符串

* `第二次`     
经过思考，第一想法是**递归**[wiki-维基百科-递归](https://zh.wikipedia.org/wiki/%E9%80%92%E5%BD%92)   
> 在数学和计算机科学中，递归指由一种（或多种）简单的基本情况定义的一类对象或方法，并规定其他所有情况都能被还原为其基本情况          
            
```python
# 字典不变
str_dict = {
    "<2c>": ",",
    "<2e>": ".",
    "<2f>": "/",
    "<2d>": "-",
    "<2b>": "+",
    "<3d>": "=",
    "<2a>": "*",
    }
# s1 为多个特殊转义的字符串
s1 = "<2d>46<2e>5"

def str_replace2(source: str):
    target = source
    for k, v in str_dict.items():
        if k in s1:
            target = source.replace(k, v) # 错误行
            str_replace2(target)
    return target

# output: "-46<2e>5"
```
这里的输出还是错误？      
因为在递归时并没有把正确的前一次结果 `target` 传入函数中       
所以需要更改代码 `target = source.replace(k, v) ==> target = target.replace(k, v)`      
这样更改后输出正确

```text
output: "-46.5"
```     


* `更深考虑`   
递归会产生层数过多导致报错或者时间很长   
称为**堆栈溢出**（英语：stack overflow）[wiki-维基百科-堆栈溢出](https://zh.wikipedia.org/wiki/%E5%A0%86%E7%96%8A%E6%BA%A2%E4%BD%8D)
所以我在 **StackOverflow**网站中提问[Is there a better way to replace a string in python? -stackoverflow](https://stackoverflow.com/questions/69218161/is-there-a-better-way-to-replace-a-string-in-python/69218390#69218390)   
获得了很多答案以及问题被判了重复扣了我2点荣誉(→_→）   
个人认为很有用的一条答案是 [How to replace multiple substrings of a string?](https://stackoverflow.com/a/69195618/16239086)   

通过自定义一个 `StringReplacer`类实现
```python
import re

class StringReplacer:

    def __init__(self, replacements, ignore_case=False):
        patterns = sorted(replacements, key=len, reverse=True)
        self.replacements = [replacements[k] for k in patterns]
        re_mode = re.IGNORECASE if ignore_case else 0
        self.pattern = re.compile('|'.join(("({})".format(p) for p in patterns)), re_mode)
        def tr(matcher):
            index = next((index for index,value in enumerate(matcher.groups()) if value), None)
            return self.replacements[index]
        self.tr = tr

    def __call__(self, string):
        return self.pattern.sub(self.tr, string)

table = {
    "aaa"    : "[This is three a]",
    "b+"     : "[This is one or more b]",
    r"<\w+>" : "[This is a tag]"
}

replacer = StringReplacer(table, True)

sample1 = "whatever bb, aaa, <star> BBB <end>"

print(replacer(sample1))

# output: 
# whatever [This is one or more b], [This is three a], [This is a tag] [This is one or more b] [This is a tag]
```
时间复杂度 *(O(n))*
这样去实现将字典中键对应的值替换进字符串中，达到目的。同时很多项时时间也要比递归更快。   
很棒的方法！

<hr>

##### 时间: 2021-9-17 15:17:48
##### 作者: **iamjing66**
