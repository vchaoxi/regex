# Python 正则表达式里的单行s和多行m模式

Python 的 re 模块内置函数几乎都有一个 flags 参数，规定了正则匹配时的各种策略模式，其中有两个模式：单行（re.DOTALL, 或者re.S）和多行（re.MULTILINE, 或者re.M）模式。

> 单行模式突破了换行符 \n 的阻碍，将匹配视野扩大到整个字符串
> 多行模式引入了换行符 \n 的分隔，将匹配视野缩小到一行之内，并且逐行匹配
> 两种模式都改变了对换行符 \n 的处理策略。

## 单行模式 re.DOTALL
 
正则表达式（ Python2 ， Python3 ）里， 点号（.）能匹配除换行符以外的所有字符。也就是说，用`.*`这样的模式匹配到换行符的前面时，匹配即停止。例如下面这样的字符串：
```
This is the first line.
This is the second line.
This is the third line.
```

如果想匹配出整个字符串，只用点号是不行的，例如：

```
>>> a = 'This is the first line.\nThis is the second line.\nThis is the third line.'
>>> print(a)
This is the first line.
This is the second line.
This is the third line.
>>> import re
>>> p = re.match(r'This.*line\.' ,a)      #默认为贪婪模式，尽可能多的匹配
>>> p.group(0)
'This is the first line.'
>>>
```

在上面的例子里，即使是默认的贪婪（greedy）模式，仍然在第一行的结尾初停止了匹配。如果想完整匹配出字符串，就需要进入 单行模式 。

```
>>> q = re.match(r'This.*line\.', a, flags=re.DOTALL)
>>> q.group(0)
'This is the first line.\nThis is the second line.\nThis is the third line.'
```

> 结论：默认模式下，点号.的匹配动作到换行符即停止；单行模式下，点号.也能匹配换行符，字符串被整体匹配。单行模式改变了点号（.）的匹配策略。

## 多行模式 re.MULTILINE

有时候我们想找出一篇文章里符合特定条件一共有几行。比如在下面的例子里，我们希望找出以 This 开头，line 结尾的行。

```
>>> a = 'This is the first line.\nThis is the second line.\nThis is the third line.'
>>> print a
This is the first line.
This is the second line.
This is the third line.
>>> import re
>>> re.findall(r'^This.*line\.$', a)
[]
>>>
匹配结果为空，从 上一
```

匹配结果为空，从 上一节 我们知道，点号默认不匹配换行符，我们需要进入单行模式，设置re.DOTALL。

```
>>> re.findall(r'^This.*line\.$', a, flags=re.DOTALL)
['This is the first line.\nThis is the second line.\nThis is the third line.']
>>> 
```


匹配出了整个字符串，但这并不是我们想要的， 因为原字符串的三行都满足匹配条件，应该有三条结果。用问号 ? 切换成非贪婪模式试试：

```
>>> re.findall(r'^This.*?line\.$', a, flags=re.DOTALL)
['This is the first line.\nThis is the second line.\nThis is the third line.']
>>> 
```

仍然是整个字符串，这是因为正常情况下，行首符^和行尾符$仅仅匹配整个字符串的起始和结尾。这就是引入多行模式的原因。

在多行模式下，会把每一行看做单个字符串，用 ^ 和 $ 匹配。或者说，^除了匹配整个字符串的起始位置，还匹配换行符后面的位置；$除了匹配整个字符串的结束位置，还匹配换行符前面的位置。

```
>>> re.findall(r'^This.*line\.$', a, flags=re.MULTILINE)
['This is the first line.', 'This is the second line.', 'This is the third line.']
>>> 
```

> 结论：多行模式改变了^和$符号的匹配策略，当字符串中间有 换行符 \n 时，将字符串的每行分别匹配。

当需要在一个文本文件里跨行匹配时，单行和多行模式尤其有用。
