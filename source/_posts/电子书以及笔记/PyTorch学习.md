---
title: 工具学习|Pytorch初学习
tags:
 - 读书笔记
 - pytorch
date: 2022-3-24 18:01:01
categories:
 - 读书笔记
---

### Dataset 与 Dataloader

![image-20220324180417557](PyTorch学习/image-20220324180417557.png)

Dataset ： 提供一种方式去获取数据及其label，告诉我们总共有多少数据

Dataloader : 为后面的网络提供不同的数据形式

<!--more-->

### TensorBoard

tensorboard --logdir=logs --port=6007



### CNN

#### 卷积层

![image-20220329175337100](PyTorch学习/image-20220329175337100.png)

#### 池化层

![image-20220329205728266](PyTorch学习/image-20220329205728266.png)

input = torch.tensor([[    ]] , dtype = torch.float32)

#### 非线性激活

### 现有模型的修改

![image-20220404203839263](PyTorch学习/image-20220404203839263.png)

### 模型的保存

```python
net = Net()
# 保存方式1
torch.save(net, "net_method1.pth")
# 方式1-> 模型定义必须带过来
model = torch.load("net_method1.pth")
```

### 利用GPU训练

![image-20220405165703264](PyTorch学习/image-20220405165703264.png)

![image-20220405173525678](PyTorch学习/image-20220405173525678.png)

