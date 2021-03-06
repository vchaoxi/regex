# 匹配分组

```
字符          功能
|             匹配左右任意一个表达式
(ab)          将括号中字符作为一个分组
\num          引用分组num匹配到的字符串
(?P<name>)    分组起别名
(?P=name)     引用别名为name分组匹配到的字符串

```
## 示例1：|

需求：匹配出0-100之间的数字

```python

#coding=utf-8

import re

ret = re.match("[1-9]?\d","8")
ret.group()

ret = re.match("[1-9]?\d","78")
ret.group()

# 不正确的情况
ret = re.match("[1-9]?\d","08")
ret.group()

# 修正之后的
ret = re.match("[1-9]?\d$","08")
ret.group()

# 添加|
ret = re.match("[1-9]?\d$|100","8")
ret.group()

ret = re.match("[1-9]?\d$|100","78")
ret.group()

ret = re.match("[1-9]?\d$|100","08")
ret.group()

ret = re.match("[1-9]?\d$|100","100")
ret.group()

```

运行结果：

![](/assets/02-就业班-04-8.png)

## 示例2：( )

需求：匹配出163、126、qq邮箱之间的数字

```python

#coding=utf-8

import re

ret = re.match("\w{4,20}@163\.com", "test@163.com")
ret.group()

ret = re.match("\w{4,20}@(163|126|qq)\.com", "test@126.com")
ret.group()

ret = re.match("\w{4,20}@(163|126|qq)\.com", "test@qq.com")
ret.group()

ret = re.match("\w{4,20}@(163|126|qq)\.com", "test@gmail.com")
ret.group()

```

运行结果：

![](/assets/Snip20160906_141.png)

### 练习：

```python

>>> ret = re.match("([^-]*)-(\d+)","010-12345678")
>>> ret.group()
'010-12345678'
>>> ret.group(1)
'010'
>>> ret.group(2)
'12345678'

```

## 示例3：\

需求：匹配出`<html>hh</html>`

```python

#coding=utf-8

import re

# 能够完成对正确的字符串的匹配
ret = re.match("<[a-zA-Z]*>\w*</[a-zA-Z]*>", "<html>hh</html>")
ret.group()

# 如果遇到非正常的html格式字符串，匹配出错
ret = re.match("<[a-zA-Z]*>\w*</[a-zA-Z]*>", "<html>hh</htmlbalabala>")
ret.group()

# 正确的理解思路：如果在第一对<>中是什么，按理说在后面的那对<>中就应该是什么

# 通过引用分组中匹配到的数据即可，但是要注意是原始字符串，即类似 r""这种格式
ret = re.match(r"<([a-zA-Z]*)>\w*</\1>", "<html>hh</html>")
ret.group()

# 因为2对<>中的数据不一致，所以没有匹配出来
ret = re.match(r"<([a-zA-Z]*)>\w*</\1>", "<html>hh</htmlbalabala>")
ret.group()

```

运行结果：

![](/assets/02-就业班-04-10.png)

## 示例4：\number

需求：匹配出`<html><h1>www.vchaoxi.com</h1></html>`

```python

#coding=utf-8

import re

ret = re.match(r"<(\w*)><(\w*)>.*</\2></\1>", "<html><h1>www.vchaoxi.com</h1></html>")
ret.group()

ret = re.match(r"<(\w*)><(\w*)>.*</\2></\1>", "<html><h1>www.vchaoxi.com</h2></html>")
ret.group()

```

运行结果：

![](/assets/02-就业班-04-11.png)

## 示例5：(?P<name>) (?P=name)

需求：匹配出`<html><h1>www.vchaoxi.com</h1></html>`

```python

#coding=utf-8

import re

ret = re.match(r"<(?P<name1>\w*)><(?P<name2>\w*)>.*</(?P=name2)></(?P=name1)>", "<html><h1>www.vchaoxi.com</h1></html>")
ret.group()

ret = re.match(r"<(?P<name1>\w*)><(?P<name2>\w*)>.*</(?P=name2)></(?P=name1)>", "<html><h1>www.vchaoxi.com</h2></html>")
ret.group()

```

### 注意：(?P<name>)和(?P=name)中的字母p大写

运行结果：

![](/assets/Snip20160907_165.png)


