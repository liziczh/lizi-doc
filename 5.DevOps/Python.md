Python3

#### Python简介

- 解释性：无编译环节。
- 交互式：在 `>>>` 后可直接执行代码。
- 面向对象：

#### 基础语法

输出语句：

```python
print("Hello World")
```

标识符：

- 由字母数字下划线组成。
- 首字符必须为字母/下划线`_`。
- 大小写敏感。

注释：

```python
# 单行注释

'''
多行注释
'''

"""
多行注释
"""
```

字符串String：

- `''`和`""`完全相同。
- 多行字符串：`"""`。
- 转义字符：`\`。
- 原生字符串(raw)：`r"\n"`。
- 字符串连接：`+`。
- 字符串重复：`*`。
- 字符串索引：  从左往右以 0 开始，从右往左以 -1 开始。
- 字符串截取： `变量[头下标:尾下标:步长]`。

```python
# 字符串定义
word = 'word'
sentence = "sentence"
paragraph = """this is first paragraph
this is second paragraph"""
print(word)
print(sentence)
print(paragraph)
# 转义字符
print("Hel\nlo") # Hel  lo
# 原生字符串"raw"
print(r"Hel\nlo") # Hel\nlo
# 字符串拼接
print("Hello" + "Wrold")  # HelloWorld
# 字符串重复
print("Hello" * 2) # HelloHello
# 字符串截取
str = 'Hello'
print(str[0:-1])  # Hell
print(str[0]) # H
print(str[1:3]) # el
```

行与缩进：python采用缩进代替`{}`。

```python
# 条件语句
if True:
    print("True")
else:
    print("False")
```

多行语句：使用 `\` 连接。

```python
total = "item1" + \
    "item2" + \
    "item3"
```

空行：函数之间使用空行分隔。

同一行显示多条语句：使用分号`;`分割。

```python
import sys; x = 'runoob'; sys.stdout.write(x + '\n')
```

导入：import 与 from...import

```python
# 导入sys
import sys
# 导入sys中的指定模块
from sys import argv,path
```

#### 数据类型

- Number（数字）不可变数据

- String（字符串）不可变数据
- Tuple（元组）不可变数据
- List（列表）可变数据
- Set（集合）可变数据
- Dictionary（字典）可变数据

判断类型：

```python
# type()：不会认为子类是一种父类类型
type(a)
# isinstance()：认为子类是一种父类类型
isinstance(a, int)
```

Number：

- int：整数。1
- bool：布尔。True（1），False（0）；
- float：浮点数。1.23
- complex：复数。1+2j。

```python
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
```

多变量赋值：

```python
a = b = c =1
a, b, c = 1, 2, "hello"
```

