# re模块操作

在Python中需要通过正则表达式对字符串进行匹配的时候，可以使用一个模块，名字为re

## 1. re模块的使用过程

```python

    #coding=utf-8

    # 导入re模块
    import re

    # 使用match方法进行匹配操作
    result = re.match(正则表达式,要匹配的字符串)

    # 如果上一步匹配到数据的话，可以使用group方法来提取数据
    result.group()

```

**re.match是用来进行正则匹配检查的方法，若字符串匹配正则表达式，则match方法返回匹配对象（Match Object），否则返回None（注意不是空字符串""）。**

**匹配对象Macth Object具有group方法，用来返回字符串的匹配部分。**

## 2. re模块示例(匹配以vchaoxi开头的语句)

```python

    #coding=utf-8

    import re

    result = re.match("vchaoxi","vchaoxi.com")

    result.group()

```

运行结果为：

```python

vchaoxi

```

## 3. 说明

- re.match() 能够匹配出以xxx开头的字符串