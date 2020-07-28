---
title: learn pytorch chapter3
author: lihy
tags:
  - 2020夏
  - pytorch
  - linear regression
  - softmax
top: false
cover: false
date: 2020-07-28 14:07:38
categories: 笔记
summary:
img:
coverImg:
---

## class 3.2 线性回归从零实现

&emsp;&emsp;[本章节传送门](https://tangshusen.me/Dive-into-DL-PyTorch/#/chapter03_DL-basics/3.2_linear-regression-scratch?id=_324-%e5%ae%9a%e4%b9%89%e6%a8%a1%e5%9e%8b)
&emsp;&emsp;一些需要注意的点

- 步骤有：
   1. create dataset
   2. reading dataset: data_iter(batch_size, features, labels)
   3. initiate parameters
   4. define model: linreg(X, w, b)
   5. define loss function: squared_loss(y_hat, y)
   6. define Optimization algorithm: sgd(params, lr, batch_size)
   7. train model
   8. gain predict parameters
- 语法上的一些点：
   1. torch包的随记函数：`torch.randn(num_examples, num_inputs, dtype=torch.float32)`
   2. torch和numpy的转换以及numpy的正态分布`labels += torch.tensor(np.random.normal(0, 0.01, size=labels.size()), dtype=torch.float32)`
   3. 对需要更新的参数设置梯度`w.requires_grad_(requires_grad=True)`
   4. torch的数据格式：torch.LongTensor为整数
   5. yield生成迭代器
   6. torch.mm函数实现矩阵乘法

```python
import torch
from IPython import display
from matplotlib import pyplot as plt
import numpy as np
import random
from d2lzh_pytorch import data_iter, linreg, squared_loss, sgd, set_figsize

# 1. create dataset
num_inputs = 2
num_examples = 1000
true_w = [2, -3.4]
true_b = 4.2
features = torch.randn(num_examples, num_inputs, dtype=torch.float32)
labels = true_w[0] * features[:,0] + true_w[1] * features[:, 1] + true_b
labels += torch.tensor(np.random.normal(0, 0.01, size=labels.size()), dtype=torch.float32)
#plt.scatter(features[:,1].numpy(), labels.numpy(), 1)

# 2. reading dataset: data_iter(batch_size, features, labels)
# 3. initiate parameters
w = torch.tensor(np.random.normal(0, 0.01, (num_inputs, 1)), dtype=torch.float32)
b = torch.zeros(1, dtype=torch.float32) #b.data
w.requires_grad_(requires_grad=True)
b.requires_grad_(requires_grad=True)

# 4. define model: linreg(X, w, b)
# 5. define loss function: squared_loss(y_hat, y)
# 6. define Optimization algorithm: sgd(params, lr, batch_size)
# 7. train model

lr = 0.03
num_epochs = 3
batch_size = 10
net = linreg
loss = squared_loss

for epoch in range(num_epochs):  # 训练模型一共需要num_epochs个迭代周期
    # 在每一个迭代周期中，会使用训练数据集中所有样本一次（假设样本数能够被批量大小整除）。
    # X和y分别是小批量样本的特征和标签
    for X, y in data_iter(batch_size, features, labels):
        l = loss(net(X, w, b), y).sum()  # l是有关小批量X和y的损失
        l.backward()  # 小批量的损失对模型参数求梯度
        sgd([w, b], lr, batch_size)  # 使用小批量随机梯度下降迭代模型参数

        # 不要忘了梯度清零
        w.grad.data.zero_()
        b.grad.data.zero_()
    train_l = loss(net(features, w, b), labels)
    print('epoch %d, loss %f' % (epoch + 1, train_l.mean().item()))
print(true_w, w, true_b, b, sep='\n')
```

## class 3.3 线性回归简洁实现

- 步骤有：
   1. create dataset
   2. reading dataset: data_iter(batch_size, features, labels)
   3. define model: `net = nn.Sequential(nn.Linear(num_inputs, 1))`
   4. initiate parameters: `init.normal_(net[0].weight, mean=0, std=0.01);init.constant_(net[0].bias, val=0)`
   5. define loss function: loss = nn.MSELoss()
   6. define Optimization algorithm: optimizer = optim.SGD(net.parameters(), lr=0.03)
   7. train model
   8. gain predict parameters
- 语法上的一些点：
   1. `torch.utils.data`模块提供了有关数据处理的工具
   2. `torch.nn`模块定义了大量神经网络的层
   3. `torch.nn.init`模块定义了各种初始化方法
   4. `torch.optim`模块提供了很多常用的优化算法。

```python
# class 3.3线性回归简洁实现
import torch
import torch.nn as nn
from torch.nn import init # 4. initiate parameters
import torch.optim as optim # 6. define Optimization algorithm
from IPython import display
from matplotlib import pyplot as plt 
import numpy as np 
import random
import torch.utils.data as Data # 2. reading data

from d2lzh_pytorch import data_iter, linreg, squared_loss, sgd, set_figsize

# 可以手动集成nn.Module, 文中推荐使用nn.sequential构成网络
class LinearNet(nn.Module):
    def __init__(self, n_feature):
        super(LinearNet, self).__init__()
        self.linear = nn.Linear(n_feature, 1)
    # forward 定义前向传播
    def forward(self, x):
        y = self.linear(x)
        return y

# 1. create dataset
num_inputs = 2
num_examples = 1000
true_w = [2, -3.4]
true_b = 4.2
features = torch.tensor(np.random.normal(0, 1, (num_examples, num_inputs)), dtype=torch.float)
labels = true_w[0] * features[:, 0] + true_w[1] * features[:, 1] + true_b
labels += torch.tensor(np.random.normal(0, 0.01, size=labels.size()), dtype=torch.float)

# 2. reading data
batch_size = 10
dataset = Data.TensorDataset(features, labels)
data_iter = Data.DataLoader(dataset, batch_size, shuffle=True)

# 3. define model
net0 = LinearNet(num_inputs) # print(net) 
# 用nn.Sequential来更加方便地搭建网络，Sequential是一个有序的容器，网络层将按照在传入Sequential的顺序依次被添加到计算图中
net = nn.Sequential(nn.Linear(num_inputs, 1)) # net.add_module('linear', nn.Linear(1, 1))
# net.parameters()返回一个生成器，包含全部网络参数

# 4. initiate parameters
init.normal_(net[0].weight, mean=0, std=0.01)
init.constant_(net[0].bias, val=0)  # 也可以直接修改bias的data: net[0].bias.data.fill_(0)

# 5. define loss function
loss = nn.MSELoss()

# 6. define Optimization algorithm
optimizer = optim.SGD(net.parameters(), lr=0.03) # print(optimizer)
# 可以为不同子网络设置不同的学习率，这在finetune时经常用到
# optimizer =optim.SGD([
#                 # 如果对某个参数不指定学习率，就使用最外层的默认学习率
#                 {'params': net.subnet1.parameters()}, # lr=0.03
#                 {'params': net.subnet2.parameters(), 'lr': 0.01}
#             ], lr=0.03)
# 调整学习率
# for param_group in optimizer.param_groups:
#     param_group['lr'] *= 0.1 # 学习率为之前的0.1倍

# 7. train model
num_epochs = 3
for epoch in range(1, num_epochs + 1):
    for X, y in data_iter:
        output = net(X)
        l = loss(output, y.view(-1, 1))
        optimizer.zero_grad() # 梯度清零，等价于net.zero_grad()
        l.backward()
        # 调用optim实例的step函数来迭代模型参数。
        optimizer.step()
    print('epoch %d, loss: %f' % (epoch, l.item()))

# output predict patameters
dense = net[0]
print(true_w, dense.weight)
print(true_b, dense.bias)
```

## class 3.4 softmax回归

&emsp;&emsp;前文的模型是回归的，当标签处于离散状态，可以定义$y_i$来表示第i个标签, $\hat{y}_i$表示预测出的概率。  
&emsp;&emsp;对于单样本，softmax回归对样本i分类的矢量计算表达式为
$$\mathbf{o}^{(i)} =\mathbf{x}^{(i)} \mathbf{W}+\mathbf{b},$$
$$\hat{\mathbf{y}}^{(i)} =softmax (\mathbf{o}^{(i)})$$
&emsp;&emsp;对于批量样本分类，softmax回归对样本i分类的矢量计算表达式为
$$\mathbf{O}^{(i)} =\mathbf{X} \mathbf{W}+\mathbf{b},$$
$$\hat{\mathbf{Y}} =softmax (\mathbf{O})$$
