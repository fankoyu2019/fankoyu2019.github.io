---
title: 基础学习|Python初认识
tags:
 - 读书笔记
 - python
date: 2021-7-30 16:31:01
categories:
 - 读书笔记
---





## 注释

1.#

Python 实际上没有多行注释的语法。

要添加多行注释，您可以为每行插入一个 ＃：

2."""   """

由于 Python 将忽略未分配给变量的字符串文字，因此您可以在代码中添加多行字符串（三引号） 

<!--more-->

## 变量

字符串变量可以使用单引号或双引号进行声明 

#### global 关键字

通常，在函数内部创建变量时，该变量是局部变量，只能在该函数内部使用。

要在函数内部创建全局变量，您可以使用 global 关键字。

要在函数内部更改全局变量的值，请使用 global 关键字引用该变量： 

#### 随机数

Python 没有 random() 函数来创建随机数，但 Python 有一个名为 random 的内置模块，可用于生成随机数： 

## 字符串

注释：在结果中，换行符插入与代码中相同的位置。 

但是，**Python 没有字符数据类型，单个字符就是长度为 1 的字符串。** 

获取从位置 2 到位置 5（不包括）的字符： 

```
print(b[2:5])
```

**左闭右开 -->[ , )**

使用负索引从字符串末尾开始切片

```
b = "Hello, World!"
print(b[-5:-1])#输出orld
print(b[-5:])#输出orld!
```

####  字符串方法

strip() 方法删除开头和结尾的空白字符 

split() 方法在找到分隔符的实例时将字符串拆分为子字符串 

#### 检查字符串

如需检查字符串中是否存在特定短语或字符，我们可以使用 in 或 not in 关键字 

```
txt = "China is a great country"
x = "ina" in txt
print(x)
```

#### 字符串格式

可以使用 format() 方法组合字符串和数字 

format() 方法接受传递的参数，格式化它们，并将它们放在占位符 {} 所在的字符串中 

您可以使用索引号 {0} 来确保参数被放在正确的占位符中 

```
quantity = 3
itemno = 567
price = 49.95
myorder = "I want to pay {2} dollars for {0} pieces of item {1}."
print(myorder.format(quantity, itemno, price))
```

**注释：所有字符串方法都返回新值。它们不会更改原始字符串。** 

## 布尔

bool() 函数可让您评估任何值，并为您返回 True 或 False。 

除空字符串外，任何字符串均为 True。

除 0 外，任何数字均为 True。

除空列表外，任何列表、元组、集合和字典均为 True。

Python 还有很多返回布尔值的内置函数，例如 isinstance() 函数，该函数可用于确定对象是否具有某种数据类型 

## 运算符

**  幂

// 整除

#### Python 逻辑运算符

逻辑运算符用于组合条件语句： 

and ,or ,not 

#### Python 身份运算符

身份运算符用于比较对象，不是比较它们是否相等，但如果它们实际上是同一个对象，则具有相同的内存位置 

is,not is

#### Python 成员运算符

成员资格运算符用于测试序列是否在对象中出现 

in ,not in



## 列表

负索引表示从末尾开始，-1 表示最后一个项目，-2 表示倒数第二个项目，依此类推。

####  复制列表

您只能通过键入 list2 = list1 来复制列表，因为：list2 将只是对 list1 的引用，list1 中所做的更改也将自动在 list2 中进行。 

```python
thislist = ["apple", "banana", "cherry"]
list2=thislist
print(list2)
#['apple', 'banana', 'cherry']
thislist[1]='apple'
print(list2)
#['apple', 'apple', 'cherry']
```

有一些方法可以进行复制，一种方法是使用内置的 List 方法 copy()。 

制作副本的另一种方法是使用内建的方法 list()。 

## 元组

元组一旦创建，您就无法向其添加项目。元组是不可改变的。

如需创建仅包含一个项目的元组，您必须在该项目后添加一个逗号，否则 Python 无法将变量识别为元组。

## 集合

要将一个项添加到集合，请使用 add() 方法。

要向集合中添加多个项目，请使用 update() 方法。

```python
thisset = {"apple", "banana", "cherry"}

thisset.update(["orange", "mango", "grapes"])

print(thisset)
```

## 字典

通过使用 items() 函数遍历键和值： 

```
for x, y in thisdict.items():
  print(x, y)
```

popitem() 方法删除最后插入的项目（在 3.7 之前的版本中，删除随机项目）： 

#### 复制字典

一种方法是使用内建的字典方法 copy()。 

制作副本的另一种方法是使用内建函数dict()。 

#### dict() 构造函数

```
thisdict = dict(brand="Porsche", model="911", year=1963)
# 请注意，关键字不是字符串字面量
# 请注意，使用了等号而不是冒号来赋值
print(thisdict)
```
## If

if 语句不能为空，但是如果您处于某种原因写了无内容的 if 语句，请使用 pass 语句来避免错误。

## For循环

注意：range(10) 不是 0 到 10 的值，而是值 0 到 9。 

range() 函数默认 0 为起始值，不过可以通过添加参数来指定起始值：range(3, 10)，这意味着值为 3 到 10（但不包括 10）：

range() 函数默认将序列递增 1，但是可以通过添加第三个参数来指定增量值：range(2, 30, 3)：

#### For循环中的else

for 循环中的 else 关键字指定循环结束时要执行的代码块：



for 语句不能为空，但是如果您处于某种原因写了无内容的 for 语句，请使用 pass 语句来避免错误。



## 函数

#### 任意函数

如果您不知道将传递给您的函数多少个参数，请在函数定义的参数名称前添加 *。

这样，函数将接收一个参数元组，并可以相应地访问各项：



函数定义不能为空，但是如果您出于某种原因写了无内容的函数定义，请使用 pass 语句来避免错误。

*args 元组

**kwargs 词典

## lambda

lambda 函数是一种小的匿名函数。 

**lambda 函数可接受任意数量的参数，但只能有一个表达式。** 

语法：

```python
lambda arguments : expression
例如
lambda a : a + 10 
print(x(5))
//print(5+10)
```



假设您有一个带一个参数的函数定义，并且该参数将乘以未知数字：

 

```
def myfunc(n):
  return lambda a : a * n

mydoubler = myfunc(2)

print(mydoubler(11))
```



## 类和对象

所有类都有一个名为 __init__() 的函数，它始终在启动类时执行。

使用 __init__() 函数将值赋给对象属性，或者在创建对象时需要执行的其他操作：

```python
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

p1 = Person("Bill", 63)

print(p1.name)
print(p1.age)
```

提示：self 参数是对类的当前实例的引用，用于访问属于该类的变量。  	

它不必被命名为 self，您可以随意调用它，但它必须是类中任意函数的*首个参数*： 



类定义不能为空，但是如果您处于某种原因写了无内容的类定义语句，请使用 pass 语句来避免错误。 

## 继承

要创建从其他类继承功能的类，请在创建子类时将父类作为参数发送：

 

```python
class Person:
  def __init__(self, fname, lname):
    self.firstname = fname
    self.lastname = lname

  def printname(self):
    print(self.firstname, self.lastname)

class Student(Person):
  pass

x = Student("Elon", "Musk")
x.printname()

```

当您添加 __init__() 函数时，子类将不再继承父的 __init__() 函数。

 注释：子的 __init__() 函数会覆盖对父的 __init__() 函数的继承。

如需保持父的 __init__() 函数的继承，请添加对父的 __init__() 函数的调用：



Python 还有一个 super() 函数，它会使子类从其父继承所有方法和属性：

## Python迭代器

 迭代器是一种对象，该对象包含值的可计数数字。

迭代器是可迭代的对象，这意味着您可以遍历所有值。

从技术上讲，在 Python 中，迭代器是实现迭代器协议的对象，它包含方法 __iter__() 和 __next__()。



列表、元组、字典和集合都是可迭代的对象。它们是可迭代的容器，您可以从中获取迭代器（Iterator）。

所有这些对象都有用于获取迭代器的 iter() 方法：

```python
mytuple = ("apple", "banana", "cherry")
myit = iter(mytuple)

print(next(myit))
print(next(myit))
print(next(myit))
```

字符串也是可迭代的对象，包含一系列字符 

​	

提示：for 循环实际上创建了一个迭代器对象，并为每个循环执行 next() 方法。 

#### 创建迭代器

要把对象/类创建为迭代器，必须为对象实现 __iter__() 和 __next__() 方法。

正如您在 Python 类/对象 一章中学到的，所有类都有名为 __init__() 的函数，它允许您在创建对象时进行一些初始化。

__iter__() 方法的作用相似，您可以执行操作（初始化等），但必须始终返回迭代器对象本身。

__next__() 方法也允许您执行操作，并且必须返回序列中的下一个项目。

```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self

  def __next__(self):
    x = self.a
    self.a += 1
    return x

myclass = MyNumbers()
myiter = iter(myclass)

print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
```



如果你有足够的 next() 语句，或者在 for 循环中使用，则上面的例子将永远进行下去。

为了防止迭代永远进行，我们可以使用 StopIteration 语句。

在 __next__() 方法中，如果迭代完成指定的次数，我们可以添加一个终止条件来引发错误：



## Try catch

**try 块允许您测试代码块以查找错误。**

**except 块允许您处理错误。**

**finally 块允许您执行代码，无论 try 和 except 块的结果如何。**

## 字符串格式化

**为了确保字符串按预期显示，我们可以使用 format() 方法对结果进行格式化。** 

format() 方法允许您格式化字符串的选定部分。

有时文本的一部分是你无法控制的，也许它们来自数据库或用户输入？

要控制此类值，请在文本中添加占位符（花括号 {}），然后通过 format() 方法运行值：

## 文件处理

在 Python 中使用文件的关键函数是 open() 函数。

open() 函数有两个参数：文件名和模式。

有四种打开文件的不同方法（模式）：

- "r" - 读取 - 默认值。打开文件进行读取，如果文件不存在则报错。
- "a" - 追加 - 打开供追加的文件，如果不存在则创建该文件。
- "w" - 写入 - 打开文件进行写入，如果文件不存在则创建该文件。
- "x" - 创建 - 创建指定的文件，如果文件存在则返回错误。

此外，您可以指定文件是应该作为二进制还是文本模式进行处理。

- "t" - 文本 - 默认值。文本模式。
- "b" - 二进制 - 二进制模式（例如图像）。

语法

此外，您可以指定文件是应该作为二进制还是文本模式进行处理：

```
f = open("demofile.txt")
```

以上代码等同于：

```
f = open("demofile.txt", "rt")
```

因为 "r" (读取)和 "t" (文本)是默认值，所以不需要指定它们。

注释：请确保文件存在，否则您将收到错误消息。

#### 文件读取

open() 函数返回文件对象，此对象有一个 read() 方法用于读取文件的内容： 

```
f = open("demofile.txt", "r")
print(f.read())
```

默认情况下，read() 方法返回整个文本，但您也可以指定要返回的字符数： 

您可以使用 readline() 方法返回一行： 

通过两次调用 readline()，您可以读取前两行： 

通过循环遍历文件中的行，您可以逐行读取整个文件： 

完成后始终关闭文件是一个好习惯。 

#### 文件删除

如需删除文件，必须导入 OS 模块，并运行其 os.remove() 函数： 

为避免出现错误，您可能需要在尝试删除文件之前检查该文件是否存在： 

```
import os
if os.path.exists("demofile.txt"):
  os.remove("demofile.txt")
else:
  print("The file does not exist")
```



如需删除整个文件夹，请使用 os.rmdir() 方法： 

删除文件夹 "myfolder"：

```
import os
os.rmdir("myfolder")
```

提示：您只能删除空文件夹。





## 常用函数

#### map 

注意！ 与c++ ，java  map不同

map() 会根据提供的函数对指定序列做映射。

第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含每次 function 函数返回值的新列表。

```
map(function, iterable, ...)
```

Python 3.x 返回迭代器。 

```
list(map(square, [1,2,3,4,5]))   # 使用 list() 转换为列表 
```

#### zip

**zip()** 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。

我们可以使用 list() 转换来输出列表。

如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。

```
//zip([iterable, ...])
>>>a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)     # 返回一个对象
>>> zipped
<zip object at 0x103abc288>
>>> list(zipped)  # list() 转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c))              # 元素个数与最短的列表一致
[(1, 4), (2, 5), (3, 6)]
 
>>> a1, a2 = zip(*zip(a,b))          # 与 zip 相反，zip(*) 可理解为解压，返回二维矩阵式
>>> list(a1)
[1, 2, 3]
>>> list(a2)
[4, 5, 6]
>>>
```



## 注意

python循环中创建一个临时变量

在循环外仍可调用

