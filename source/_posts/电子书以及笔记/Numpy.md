---
title: 基础学习|Numpy初认识
tags:
 - 读书笔记
 - numpy
date: 2021-8-15 16:31:01
categories:
 - 读书笔记
---



## NumPy简介

具有自动广播的机制！

NumPy 是用于处理数组的 python 库。 

<!--more-->

```
import numpy as np 

arr = np.array([1, 2, 3, 4, 5]) 

print(arr)
```

要创建 ndarray，我们可以将列表、元组或任何类似数组的对象传递给 array() 方法，然后它将被转换为 ndarray：

####  0-D数组

0-D 数组，或标量（Scalars），是数组中的元素。数组中的每个值都是一个 0-D 数组。

就是指一个数

#### 1-D数组

其元素为 0-D 数组的数组，称为一维或 1-D 数组。

####  2-D数组

 其元素为 1-D 数组的数组，称为 2-D 数组。

它们通常用于表示矩阵或二阶张量。

NumPy 有一个专门用于矩阵运算的完整子模块 numpy.mat。



```python
import numpy as np

arr = np.array([[1, 2, 3], [4, 5, 6]])

print(arr)
```

#### 3-D数组

其元素为 2-D 数组的数组，称为 3-D 数组。

 

```python
import numpy as np

arr = np.array([[[1, 2, 3], [4, 5, 6]], [[1, 2, 3], [4, 5, 6]]])

print(arr)
```



NumPy 数组提供了 ndim 属性，该属性返回一个整数，该整数会告诉我们数组有多少维。

#### ndmin

 在创建数组时，可以使用 ndmin 参数定义维数。

 

```
import numpy as np

arr = np.array([1, 2, 3, 4], ndmin=5)

print(arr)
print('number of dimensions :', arr.ndim)
```

## 数组索引

要访问二维数组中的元素，我们可以使用逗号分隔的整数表示元素的维数和索引。 

访问第一维中的第二个元素：

```python
import numpy as np

arr = np.array([[1,2,3,4,5], [6,7,8,9,10]])

print('2nd element on 1st dim: ', arr[0, 1])
```

使用负索引从尾开始访问数组。 

```python
import numpy as np

arr = np.array([[1,2,3,4,5], [6,7,8,9,10]])

print('Last element from 2nd dim: ', arr[1, -1])
```

## 数组裁切

注释：结果包括了开始索引，但不包括结束索引。 **[ ）**左闭右开

[*start*：*end*：*step*] 

如果我们不传递 *start*，则将其视为 0。

如果我们不传递 *end*，则视为该维度内数组的长度。

如果我们不传递 *step*，则视为 1。

#### 裁切2-D数组

从两个元素中返回索引 2： 

```python
import numpy as np

arr = np.array([[1, 2, 3, 4, 5], [6, 7, 8, 9, 10]])

print(arr[0:2, 2])
```

## 数据类型

NumPy 数组对象有一个名为 dtype 的属性，该属性返回数组的数据类型： 

我们使用 array() 函数来创建数组，该函数可以使用可选参数：dtype，它允许我们定义数组元素的预期数据类型： 

#### 转换已有数组的数据类型

更改现有数组的数据类型的最佳方法，是使用 astype() 方法复制该数组。

astype() 函数创建数组的副本，并允许您将数据类型指定为参数。

数据类型可以使用字符串指定，例如 'f' 表示浮点数，'i' 表示整数等。或者您也可以直接使用数据类型，例如 float 表示浮点数，int 表示整数。

## 副本和视图

副本和数组视图之间的主要区别在于副本是一个新数组，而这个视图只是原始数组的视图。

副本拥有数据，对副本所做的任何更改都不会影响原始数组，对原始数组所做的任何更改也不会影响副本。

视图不拥有数据，对视图所做的任何更改都会影响原始数组，而对原始数组所做的任何更改都会影响视图。

副本：x = arr.copy()

视图：x = arr.view()



如上所述，副本拥有数据，而视图不拥有数据，但是我们如何检查呢？

每个 NumPy 数组都有一个属性 base，如果该数组拥有数据，则这个 base 属性返回 None。

否则，base 属性将引用原始对象。



```
import numpy as np

arr = np.array([1, 2, 3, 4, 5])

x = arr.copy()
y = arr.view()

print(x.base)  //None
print(y.base)  //[1,2,3,4,5]
```

## 数组形状

NumPy 数组有一个名为 shape 的属性，该属性返回一个元组，每个索引具有相应元素的数量。 

## 数组重塑

#### reshape

```
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12])

newarr = arr.reshape(4, 3)

print(newarr)
```

返回原始数组，因此它是一个视图。 



您可以使用一个“未知”维度。

这意味着您不必在 reshape 方法中为维度之一指定确切的数字。

传递 -1 作为值，NumPy 将为您计算该数字。



```
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6, 7, 8])

newarr = arr.reshape(2, 2, -1)

print(newarr)
```

**注释：我们不能将 -1 传递给一个以上的维度。** 



#### 展平数组

展平数组（Flattening the arrays）是指将多维数组转换为 1D 数组。

我们可以使用 reshape(-1) 来做到这一点。



## 数组迭代

#### 使用 nditer() 迭代数组

```
import numpy as np

arr = np.array([[[1, 2], [3, 4]], [[5, 6], [7, 8]]])

for x in np.nditer(arr):
  print(x)
```

我们可以使用 op_dtypes 参数，并传递期望的数据类型，以在迭代时更改元素的数据类型。

NumPy 不会就地更改元素的数据类型（元素位于数组中），因此它需要一些其他空间来执行此操作，该额外空间称为 buffer，为了在 nditer() 中启用它，我们传参 flags=['buffered']。

```
import numpy as np

arr = np.array([1, 2, 3])

for x in np.nditer(arr, flags=['buffered'], op_dtypes=['S']):
  print(x)
```

我们可以使用过滤，然后进行迭代。 

每遍历 2D 数组的一个标量元素，跳过 1 个元素 

```
import numpy as np

arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])

for x in np.nditer(arr[:, ::2]):
  print(x)
```

#### 使用 ndenumerate() 进行枚举迭代

枚举是指逐一提及事物的序号。

有时，我们在迭代时需要元素的相应索引，对于这些用例，可以使用 ndenumerate() 方法。

```
import numpy as np

arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])

for idx, x in np.ndenumerate(arr):
  print(idx, x)
```
## 连接数组（不懂）

在 SQL 中，我们基于键来连接表，而在 NumPy 中，我们按轴连接数组。

我们传递了一系列要与轴一起连接到 concatenate() 函数的数组。如果未显式传递轴，则将其视为 0。

```
import numpy as np

arr1 = np.array([1, 2, 3])

arr2 = np.array([4, 5, 6])

arr = np.concatenate((arr1, arr2))

print(arr)
//[1 2 3 4 5 6]
```

沿着行 (axis=1) 连接两个 2-D 数组： 

```
import numpy as np

arr1 = np.array([[1, 2], [3, 4]])

arr2 = np.array([[5, 6], [7, 8]])

arr = np.concatenate((arr1, arr2), axis=1)

print(arr)
//
[[1 2 5 6]
 [3 4 7 8]]
```

#### 使用堆栈函数连接数组

堆栈与级联相同，唯一的不同是堆栈是沿着新轴完成的。

我们可以沿着第二个轴连接两个一维数组，这将导致它们彼此重叠，即，堆叠（stacking）。

我们传递了一系列要与轴一起连接到 concatenate() 方法的数组。如果未显式传递轴，则将其视为 0。

```
import numpy as np

arr1 = np.array([1, 2, 3])

arr2 = np.array([4, 5, 6])

arr = np.stack((arr1, arr2), axis=1)

print(arr)
//
[[1 4]
 [2 5]
 [3 6]]
```



NumPy 提供了一个辅助函数：hstack() 沿行堆叠。 

```
import numpy as np

arr1 = np.array([1, 2, 3])

arr2 = np.array([4, 5, 6])

arr = np.hstack((arr1, arr2))

print(arr)
//[1 2 3 4 5 6]
```



NumPy 提供了一个辅助函数：vstack() 沿列堆叠。 

```
import numpy as np

arr1 = np.array([1, 2, 3])

arr2 = np.array([4, 5, 6])

arr = np.vstack((arr1, arr2))

print(arr)
//[[1 2 3]
 [4 5 6]]

```



NumPy 提供了一个辅助函数：dstack() 沿高度堆叠，该高度与深度相同。 

```
import numpy as np

arr1 = np.array([1, 2, 3])

arr2 = np.array([4, 5, 6])

arr = np.dstack((arr1, arr2))

print(arr)
//
[[[1 4]
  [2 5]
  [3 6]]]
```



## 数组拆分

我们使用 array_split() 分割数组，将要分割的数组和分割数传递给它。 	

```
import numpy as np

arr = np.array([1, 2, 3, 4, 5, 6])

newarr = np.array_split(arr, 4)

print(newarr)
```

提示：我们也有 split() 方法可用，但是当源数组中的元素较少用于拆分时，它将不会调整元素，如上例那样，array_split() 正常工作，但 split() 会失败。 



您可以指定要进行拆分的轴。 

下面的例子还返回三个 2-D 数组，但它们沿行 (axis=1) 分割。

 

```
import numpy as np

arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12], [13, 14, 15], [16, 17, 18]])

newarr = np.array_split(arr, 3, axis=1)

print(newarr)
```

另一种解决方案是使用与 hstack() 相反的 hsplit()。 

```
import numpy as np

arr = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9], [10, 11, 12], [13, 14, 15], [16, 17, 18]])

newarr = np.hsplit(arr, 3)

print(newarr)
```

**提示：vsplit() 和 dsplit() 可以使用与 vstack() 和 dstack() 类似的替代方法。** 

## 数组搜索

#### where搜索数组

您可以在数组中搜索（检索）某个值，然后返回获得匹配的索引。

要搜索数组，请使用 where() 方法。

#### searchsorted搜索排序

有一个名为 searchsorted() 的方法，该方法在数组中执行**二进制**搜索，并返回将在其中插入指定值以维持搜索顺序的索引。

假定 searchsorted() 方法用于排序数组。

```
import numpy as np

arr = np.array([6, 7, 8, 9])

x = np.searchsorted(arr, 7)

print(x)
```

例子解释：应该在索引 1 上插入数字 7，以保持排序顺序。

该方法从左侧开始搜索，并返回第一个索引，其中数字 7 不再大于下一个值。

**返回第一个不小于7位置的索引**

从右侧搜索

默认情况下，返回最左边的索引，但是我们可以给定 side='right'，以返回最右边的索引。

**返回第一个不大于7位置的索引**



要搜索多个值，请使用拥有指定值的数组。 

查找应在其中插入值 2、4 和 6 的索引： 

```
import numpy as np

arr = np.array([1, 3, 5, 7])

x = np.searchsorted(arr, [2, 4, 6])

print(x)
```

## 数组排序

NumPy ndarray 对象有一个名为 sort() 的函数，该函数将对指定的数组进行排序。 

注释：此方法返回数组的副本，而原始数组保持不变。 

如果在二维数组上使用 sort() 方法，则将对两个数组进行排序： 

```
import numpy as np

arr = np.array([[3, 2, 4], [5, 0, 1]])

print(np.sort(arr))
```

## 数组过滤

从现有数组中取出一些元素并从中创建新数组称为过滤（filtering）。

 在 NumPy 中，我们使用布尔索引列表来过滤数组。

布尔索引列表是与数组中的索引相对应的布尔值列表。

如果索引处的值为 True，则该元素包含在过滤后的数组中；如果索引处的值为 False，则该元素将从过滤后的数组中排除。

用索引 0 和 2、4 上的元素创建一个数组：

```
import numpy as np

arr = np.array([61, 62, 63, 64, 65])

x = [True, False, True, False, True]

newarr = arr[x]

print(newarr)
```

#### 直接从数组创建过滤器

我们可以在条件中直接替换数组而不是 iterable 变量，它会如我们期望地那样工作。 

```
import numpy as np

arr = np.array([61, 62, 63, 64, 65])

filter_arr = arr > 62

newarr = arr[filter_arr]

print(filter_arr)
print(newarr)
```

## 随机数

计算机在程序上工作，程序是权威的指令集。因此，这意味着必须有某种算法来生成随机数。

如果存在生成随机数的程序，则可以预测它，因此它就不是真正的随机数。

通过生成算法生成的随机数称为伪随机数。



NumPy 提供了 random 模块来处理随机数。 



生成一个 0 到 100 之间的随机整数：

```
from numpy import random

x = random.randint(100)

print(x)
```



random 模块的 rand() 方法返回 0 到 1 之间的随机浮点数。

 生成一个 0 到 1 之间的随机浮点数：

```
from numpy import random

x = random.rand()

print(x)
```



#### 生成数组

randint() 方法接受 size 参数，您可以在其中指定数组的形状。 

```
x=random.randint(100, size=(5))
```

生成有 3 行的 2-D 数组，每行包含 5 个从 0 到 100 之间的随机整数： 

```
x = random.randint(100, size=(3, 5))
```



rand() 方法还允许您指定数组的形状。

生成包含 5 个随机浮点数的 1-D 数组： 

```
 x = random.rand(5)
```

生成有 3 行的 2-D 数组，每行包含 5 个随机数： 

```
x = random.rand(3, 5)
```





#### 从数组中生成随机数

choice() 方法使您可以基于值数组生成随机值。

choice() 方法将数组作为参数，并随机返回其中一个值。

返回数组中的值之一：

```
from numpy import random

x = random.choice([3, 5, 7, 9])

print(x)
```



choice() 方法还允许您返回一个值数组。

请添加一个 size 参数以指定数组的形状。

生成由数组参数（3、5、7 和 9）中的值组成的二维数组：

```
from numpy import random

x = random.choice([3, 5, 7, 9], size=(3, 5))

print(x)
```

## ufuncs

ufuncs 指的是“通用函数”（Universal Functions），它们是对 ndarray 对象进行操作的 NumPy 函数。 

ufunc 用于在 NumPy 中实现矢量化，这比迭代元素要快得多。

它们还提供广播和其他方法，例如减少、累加等，它们对计算非常有帮助。

ufuncs 还接受其他参数，比如：

where 布尔值数组或条件，用于定义应在何处进行操作。

dtype 定义元素的返回类型。

out 返回值应被复制到的输出数组。



如果没有 ufunc，我们可以使用 Python 的内置 zip() 方法：

```
x = [1, 2, 3, 4]
y = [4, 5, 6, 7]
z = []

for i, j in zip(x, y):
  z.append(i + j)
print(z)
```

对此，NumPy 有一个 ufunc，名为 add(x, y)，它会输出相同的结果。

### 实例

通过 ufunc，我们可以使用 add() 函数：

```
import numpy as np

x = [1, 2, 3, 4]
y = [4, 5, 6, 7]
z = np.add(x, y)

print(z)
```

 