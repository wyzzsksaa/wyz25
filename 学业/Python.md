# 基础

如果Python程序包含一个函数main（），这个函数与其他函数地位相同

Python函数可以不包含main函数

Python程序的main函数可以改变为其他名称

所有的if，def，while，class后都要：结尾

3<5>2 : **链式比较**（chained comparison），它允许你将多个比较操作链接在一起。



语句`print(f"{'Python':*^10.4}")`的执行结果是

- `:` 开始格式说明符。
- `*` 表示填充字符为星号 `*`。
- `^` 表示对齐方式为中心对齐。
- `10` 表示总宽度为 10 个字符。
- `.4` 表示截取前 4 个字符。

结果为** * Pyth***



- `encode()` 方法将字符串编码为指定的字节序列，默认使用 UTF-8 编码。
- `decode()` 方法将字节序列解码为字符串，默认也是使用 UTF-8 编码。



`zip()` 函数将两个字符串 `x` 和 `y` 中的字符按位置配对，生成一个元组迭代器。例如，如果 `x = "abc"` 和 `y = "abd"`，那么 `zip(x, y)` 会生成迭代器 `(('a', 'a'), ('b', 'b'), ('c', 'd'))`。



两个集合 {1, 3, 2} 和 {1, 2, 3} 实际上是相同的集合，只是元素的顺序不同。由于集合是无序的，这两个集合实际上是等价的

## 逻辑运算

**or 运算符**

- 如果第一个操作数为真（truthy），则返回第一个操作数。
- 如果第一个操作数为假（falsy），则返回第二个操作数

**and 运算符**

- 如果第一个操作数为真（truthy），则返回第二个操作数。
- 如果第一个操作数为假（falsy），则返回第一个操作数

**not运算符**

not False = True

## 身份运算符

is：两边是否相同，是返回True

is not：两边是否不同

## f-string

```
print(f"{a} + {b} = {a + b}")
```

输出类似2+ 4 = 6

# 函数库

## 内置函数

**str.find：**查找子字符串第一次出现的位置，未找到返回-1

str.find（‘xxx'，30）：从索引30的地方开始找



**chr（）：**用于将整数转换为对应的字符（Unicode）

**ord（）：**将获取的Unicode码

```
print(ord('A'))  # 输出: 65
print(chr(65))  # 输出: A
```

**all（）：**判断可迭代对象是否都为真

any（）：任意有一个



**range(start, stop, step) ：** 左闭右开



**sorted()**:

```
sorted_list = sorted(input_list, key=lambda x: (abs(x), -x), reverse=True)
```

abs(x)：按绝对值排序

-x：若绝对值相同，负数在前

reverse=True：从大到小

## math

abs（）：用于返回绝对值或模

print（abs（3+4j））：输出5

ceil（）：向上取整

gcd（a，b）：返回ab两数的最大公约数

## time

localtime() 将当前时间戳转换成本地时间的struct_time对象

strftime()：将struct_time对象按指定格式字符串转换成字符串形式的时间表示

mktime()：它的主要作用是将一个用结构化时间元组表示的本地时间转换为时间戳。这个时间戳是一个浮点数，表示从 Unix 纪元到指定时间的总秒数

## random

uniform：生成一个随机小数

## sys

sys.version：查看Python版本

## factorial

factorial(i)：计算i的阶乘

## ast

literal_eval(input())：输入一行列表

输入必须是一个合法的列表，例如：[1,2,3]

# 序列

x[:] 创建的是列表 `x` 的一个**浅拷贝**(shallow copy)

结果是得到一个与 `x` 内容完全相同但**独立的新列表对象**



转列表：list()

转元组：tuple（）



key=lambda x：x[1]

key：用于指定排序或比较的依据

lambda x：x[1]：匿名函数，输入参数x（表示列表中的每个元素），返回x[1]





![Screenshot 2025-03-03 185910](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-03-03 185910.png)

![Screenshot 2025-03-03 190540](https://wwworder.oss-cn-beijing.aliyuncs.com/img/Screenshot 2025-03-03 190540.png)

**keys：**

```
my_dict = {'a': 1, 'b': 2, 'c': 3}
for key in my_dict.keys():
    print(key)
# 输出:
# a
# b
# c
```

## 列表

list = [10,20,30,40]

list.append(50) 默认在最后添加

list.insert(2,50) 

list.remove(10) 

list.pop() 默认删掉最后一个，可以指定下标

list.clear() 清除列表

list.reverse() 翻转

list.index(m)：返回m的下标

在Python中，对列表进行越界切片操作**不会**抛出异常，而是会返回一个空列表

对于列表而言,在尾部追加元素比在中间位置插入元素速度更快一些,尤其是对于包含大量元素的列表

集合可当列表的元素

## 字典

dic = {key1: value1, key2 : value2 ...}

字典中的键不能重复，是唯一的并且不可改变 

添加：dic["key3"] = "value3"

修改：dic["key1"] = "value4"

删除：del dic["key2"]

dic.pop(key, -1)：弹出90对应的值，若无则返回-1

dic.pop(key, default)：则返回 key 本身



当对字典直接使用 `max()` 和 `min()` 时，默认比较的是字典的 **键（key）**，而不是值（value）



元组可以作为字典的“键”，集合不行，但可以当值

键值对(字典)”类型数据的组织维度是 高位数据



```
count = {}  # 空字典
value = count.get(word, 0)
```

在count中获取word，若没有则返回0

## 元组（Tuple）

具有 不可变性 和 有序性

没有类型限制

tuple = (19,"hello",True, 3,14)

tuple[1] = "hello"

一经创建，不可更改

tuple2 = (10, 20, (30, 40), 50)

print(tuple2[ 2 ] [0 ]) == 30

集合可以当元组元素

## 集合

无序且不重复，会自动除去重复元素

不支持下标访问其中元素

a = set（）

无法册除集合中指定位置的元素,只能删除特定值的元素

Python 集合中的元素可以是元组，不能是列表，字典