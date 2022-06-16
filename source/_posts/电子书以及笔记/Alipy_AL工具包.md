---
title: 工具学习|Alipy
tags:
 - 读书笔记
 - Alipy
date: 2022-3-4 15:05:01
categories:
 - 读书笔记
---

# Alipy使用（一）

官网：http://parnec.nuaa.edu.cn/huangsj/alipy/

博客文档讲解：https://blog.csdn.net/weixin_44575152/article/details/100783835

```python
import alipy as ap
import numpy as np
from alipy.metrics import *
from sklearn.datasets import load_iris
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, Y = load_iris(return_X_y=True)
    tool = ap.ToolBox(X=X, y=Y)
    idx_tr, idx_te, idx_lab, idx_unlab = tool.split_AL(test_ratio=0.1, initial_label_rate=0.1, split_count=1)
    """============================================="""
    model = tool.get_default_model()
    model.fit(X[idx_lab[0]], Y[idx_lab[0]])
    Y_pre = model.predict(X=X[idx_unlab[0]])
    # 这里测试一下准确率
    print(accuracy_score(Y_pre, Y[idx_unlab[0]]))
    """============================================="""


if __name__ == '__main__':
    test()
```



## Alipy概述

 Alipy提供了一个基于模块的主动学习框架，其目标是用各种工具功能支持实验实现。这些工具是以低耦合的方式设计的，以便用户可以根据自己的习惯来编程实验项目。支持的模块如下：

| 序列 | 模块名                                           | 功用                                     |
| ---- | ------------------------------------------------ | ---------------------------------------- |
| 1    | alipy.data_manipulate                            | 数据划分                                 |
| 2    | alipy.query_strategy                             | 数据查询策略                             |
| 3    | alipy.index.IndexCollection                      | 索引管理                                 |
| 4    | alipy.metric                                     | 模型性能计算                             |
| 5    | alipy.experiment.state/alipy.experiment.state_io | 保存每次查询后的中间结果并从断点恢复程序 |
| 6    | alipy.experiment.stopping_criteria               | 获取停止标准                             |
| 7    | alipy.experiment.experiment_analyser             | 可视化实验结果                           |
| 8    | alipy.oracle                                     | 提供清洁、噪音或者代价敏感的oracle       |
| 9    | alipy.utils.multi_thread                         | 多次实验                                 |

支持以下算法：

## 高级封装



### 1.1.1 划分数据

```python
alibox = ToolBox(X=data_set, y=label_set, query_type='AllLabels', saving_path='.')
alibox.split_AL(test_ratio=0.3, initial_label_rate=0.1, split_count=1)
```

执行这段代码后会在当前路径 生成4个数据文件，如果后期更改这个函数的参数，必须先把生成的文件删除，再执行程序

即：若文件存在，会自动调用已经生成的文件，不会再调用该函数生成文件

# Alipy使用（二）

## 1 数据操作

### 1.1 数据划分

```python
import alipy as ap
import numpy as np
from sklearn.datasets import load_iris
np.random.seed(1)  # 保证你我的结果一直


def test():
    data = load_iris()
    X, Y = data["data"], data["target"]
    tool = ap.ToolBox(X=X, y=Y)
    idx_tr, idx_te, idx_lab, idx_unlab = tool.split_AL(test_ratio=0.1, initial_label_rate=0.1, split_count=1)
    print("训练集索引：", idx_tr)
    print("测试集索引：", idx_te)
    print("有标签集索引：", idx_lab)
    print("无标签集索引：", idx_unlab)


if __name__ == '__main__':
    test()
```

### 1.2 特征缩放

包括最大最小和标准缩放：
  1）**最大最小缩放**把数据集的属性值统一到[lower, upper]内：

```
import numpy as np
from sklearn.datasets import load_iris
from alipy.data_manipulate import minmax_scale
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, _ = load_iris(return_X_y=True)
    # 通常将范围限定在[0, 1]
    X = minmax_scale(X=X, feature_range=(0, 1))
    print(X[:5])


if __name__ == '__main__':
    test()
```

2）**标准缩放**将指定值减去所在属性列的平均值，随后结果除以相应的标准差来处理：

```python
import numpy as np
from sklearn.datasets import load_iris
from alipy.data_manipulate import StandardScale
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, _ = load_iris(return_X_y=True)
    X = StandardScale(X=X)
    print(X[:5])


if __name__ == '__main__':
    test()
```

## 2 查询策略

简单说来，查询策略即是**如何从无标签集中查询实例的策略？** 在Alipy中，已实现了七种不同的查询策略，具体可以参见：https://blog.csdn.net/weixin_44575152/article/details/100783835

### 2.1 使用内置的逻辑回归模型选择

```
import alipy as ap
import numpy as np
from sklearn.datasets import load_iris
from alipy.data_manipulate import StandardScale
from alipy.query_strategy import QueryInstanceQBC
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, Y = load_iris(return_X_y=True)
    X = StandardScale(X=X)
    # 初始化工具箱
    tool = ap.ToolBox(X=X, y=Y)
    idx_tr, idx_te, idx_lab, idx_unlab = tool.split_AL(test_ratio=0.1, initial_label_rate=0.1, split_count=1)
    # 指定查询策略，当然不同的查询策略有不同的超参数
    qbc = QueryInstanceQBC(X, Y)
    # 使用默认模型
    model = tool.get_default_model()
    model.fit(X[idx_lab[0]], Y[idx_lab[0]])
    """这里是重点 --> 获取查询结果"""
    idx_query = qbc.select(idx_lab[0], idx_unlab[0], batch_size=10, model=model)
    print(idx_lab[0])
    print(idx_query)


if __name__ == '__main__':
    test()
```

输出如下：

```
[ 14  98  75  16 131  56 141  44  29 120  94   5 102  51]
[123 128 126 121 138 149 114  85 111 103] 
```

查询将在无标签集中进行，所选择的样本可以用于之后的模型训练。

## 3 Oracle

 1）指定索引和标签初始化：

```python
import alipy as ap
import numpy as np
from sklearn.datasets import load_iris
from alipy.data_manipulate import StandardScale
from alipy.oracle import Oracle
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, Y = load_iris(return_X_y=True)
    X = StandardScale(X=X)
    # 初始化工具箱
    tool = ap.ToolBox(X=X, y=Y)
    idx_tr, idx_te, idx_lab, idx_unlab = tool.split_AL(test_ratio=0.1, initial_label_rate=0.1, split_count=1)
    """============================================="""
    # 其实就是把无标记集的标签和索引告诉他
    # 无标记集不是没有标签吗？
    # 这里可以看作是你通过魔镜获取的 哈哈哈
    oracle = Oracle(labels=Y[idx_unlab[0]], indexes=idx_unlab[0])
    print(idx_unlab)
    print(oracle.index_keys)
    """============================================="""


if __name__ == '__main__':
    test()
```

### 2.2 知识库

`alipy.oracle.ElementRepository `是一个类，用于存储查询的信息（例如，查询的标签，选择的索引，成本），它是oracle的支持工具。

对于干净的预言机，此类通常是不必要的，因为您可以通过使用所选索引为您的真实标签矩阵编制索引来获取查询的标签。然而，在一些特殊设置（例如，嘈杂的预言机）中，用户可能希望存储查询的信息以供进一步分析。

知识库的功能包括：

1、免费获取查询信息

2.历史记录

3.获取训练模型的标签集

下面我们介绍一下这个类的基本用法。

```python
import alipy as ap
import numpy as np
from sklearn.datasets import load_iris
from alipy.data_manipulate import StandardScale
from alipy.oracle import Oracle, Oracles, ElementRepository, MatrixRepository
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, Y = load_iris(return_X_y=True)
    X = StandardScale(X=X)
    # 初始化工具箱
    tool = ap.ToolBox(X=X, y=Y)
    idx_tr, idx_te, idx_lab, idx_unlab = tool.split_AL(test_ratio=0.1, initial_label_rate=0.1, split_count=1)
    """============================================="""
    # 记录已标记或者已查询的
    # 如果不传入examples的话，两者没有区别
    # 传入的话，前者的样本可以是复杂对象，后者只能是array
    rep_ele = ElementRepository(labels=Y[idx_lab[0]], indexes=idx_lab[0])
    rep_mat = MatrixRepository(labels=Y[idx_lab[0]], indexes=idx_lab[0])
    print("更新前", idx_lab)
    # 更新一下
    idx_select = [idx_unlab[0][0]]
    rep_ele.update_query(labels=Y[idx_select], indexes=idx_select)
    X, Y, idx_lab = rep_ele.get_training_data()
    print("更新后", idx_lab)
    """============================================="""


if __name__ == '__main__':
    test()
```

## 3 索引管理



```python
import alipy as ap
import numpy as np
from sklearn.datasets import load_iris
np.random.seed(1)  # 保证你我的结果一直


def test():
    X, Y = load_iris(return_X_y=True)
    tool = ap.ToolBox(X=X, y=Y)
    idx_tr, idx_te, idx_lab, idx_unlab = tool.split_AL(test_ratio=0.1, initial_label_rate=0.1, split_count=1)
    """============================================="""
    idx_collect = tool.IndexCollection(idx_lab[0])
    print("索引初始化：", idx_collect.index)
    # 添加重复元素会警告，添加后却不是有序的，有点扯拐
    idx_collect.add(idx_unlab[0][0])
    print("添加一个元素：", idx_collect.index)
    idx_collect.discard(idx_unlab[0][0])
    print("删除一个元素：", idx_collect.index)
    idx_collect.update(idx_unlab[0][:3])
    print("添加多个元素：", idx_collect.index)
    idx_collect.difference_update(idx_unlab[0][:3])
    print("删除多个元素：", idx_collect.index)
    """============================================="""


if __name__ == '__main__':
    test()
```

