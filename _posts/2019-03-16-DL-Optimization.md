---
title: DL Optimization
date: 2019-03-16 23:30:00
categories:
- Deep Learning
---





GD（BP）, SGD, mini-batch, hyper-parameter tuning



## Problem 

to train a two-layer network classifier

![two layer network](https://s2.ax1x.com/2019/03/13/Akt8ds.jpg)



### Back propagation

- 主要思路是使用梯度将预测误差  $\hat{y} -y $  传播回整个模型

- 模型中符号

  - 网络输出 Networkoutput
    $$
    \hat{y} = f(x;\theta) = )(\hat{y_1},\hat{y_2},...,\hat{y_k})
    $$

  - 理想输出 Ideal output
    $$
    y = (y_1,...,y_K) = (0,...,1,...0)
    $$

  - 交叉熵 Cross emtropy loss 
    $$
    l(y,\hat y) = -\sum_{k=1}^Ky_klog\hat{y_k}
    $$

  - ReLU $g(u_i) = max(0, u_i)$, Softmax $\sigma(z)$

  - 模型参数 $\theta = [w_1, ... ,w_8,b_1,...,b_4 ]^T$

  $$
  W^h = \begin{bmatrix}
  w_1 & w_2\\ 
  w_3 & w_4
  \end{bmatrix},b^h = \begin{bmatrix}
  b_1 \\ 
  b_2
  \end{bmatrix}, W^o = \begin{bmatrix}
   w_5 & w_7\\ 
   w_4 & w_8 
  \end{bmatrix},b^o = \begin{bmatrix}
   b_3 \\ 
   b_4
  \end{bmatrix}
  $$

- 用链式法则计算导数的例子：计算$\frac{\delta \ l}{\delta  w_8}$

  - 根据链式法则，可以得到所求导数的表达式
    $$
    \frac{\delta  l}{\delta  w_8} = \frac{\delta l}{\delta z_2}\frac{\delta z_2}{\delta w_8}
    $$

  - 由于pre-softmax funciton $z_2 = w_6h1+w_8h_2+b_4$，可得
    $$
    \frac{\delta z_2}{\delta w_8} = h_2  \tag{1}
    $$

  - 为了计算$\frac{\delta l}{\delta z_2}$，需要计算softmax函数的导数即$\frac{\delta \hat{y_j}}{\delta z_2}$；

    预测值的表达式为$\hat{y_j} = \sigma (z)_j = \frac{e^{z_j}}{\sum_{k=1}^Ke^{z_k}} for j = 1,...,K$，则$\hat{y_j}$对$z_i$的导数可以表示为
    $$
    \begin{align}
    \frac{\hat{y_j}}{\delta z_i}
    &= \frac{e^{z_j}}{\sum{k=1}^Ke^{z_k}}\frac{dz_j}{dz_i}-\frac{e^{z_j}}{\sum{k=1}^Ke^{z_k}}e^{z_i}\\
    &=\hat{y_j}\frac{dz_j}{dz_i} - \hat{y_j}\hat{y_i}\\
    &= \left\{\begin{matrix}
     \hat{y_i}(1-\hat{y_i})& if i = j\\ 
     -\hat{y_j}\hat{y_i}& if i \neq j
    \end{matrix}\right.\tag{2}
    \end{align}
    $$

  - 根据链式法则，
    $$
    \begin{align}
    \frac{\delta l}{\delta z_i} &= -\sum_{k=1}^Ky_k\frac{\delta\ log\hat{y_k}}{\delta\ z_i}\\
    &=\sum_{k=1}^Ky_k\frac{1}{y_k}\frac{\delta\ \hat{y_k}}{\delta\ z_i}\\
    
    \end{align}
    $$
    带入(1)式得
    $$
    \begin{align}
    &=-y_i\frac{1}{\hat{y_i}}(1-\hat{y_i})\ -\ \sum_{k\neq i} y_k\frac{1}{\hat{y_i}}( -\hat{y_j}\hat{y_i})\\
    &=\hat{y_i}(\sum^{K}_{k=1}y_k)-y_i \\
    &=\hat{y_i}-y_i\\
    \end{align} \tag{3}
    $$

  - 根据(1)式和(3)式可得
    $$
    \frac{\delta  l}{\delta  w_8} = h_2(\hat{y_2}-y_2)
    $$
    同理可得输出$l$对其他模型参数的导数

- 参数更新：
  $$
  w' = w - lr\ *\ \frac{\delta l}{\delta w}
  $$
  

### Stochastic optimization

快速梯度下降

- 符号

  - 训练数据集 Training dataset
    $$
    D = \{(x_n, y_n) | n = 1,...,N \}
    $$

  - Loss funtion = 交叉熵损失函数的均值
    $$
    L(\theta) = \frac{1}{N}\sum^N_{n=1}l(y_n, f(x_n;\theta))
    $$

- 使用梯度下降和所有数据更新模型参数过程如下
  $$
  \begin{align}
  \theta_{t+1} &= \theta_t-\eta\ \triangledown L(\theta_t)\\
  &=\frac{\eta}{N}\sum^N_{n=1}l(y_n, f(x_n;\theta))
  \end{align}
  $$

  >  使用所有数据会使得训练变得非常慢，并且要存储的数据量很大，故需要找损失函数的Loss funtion；



### Hyper-parameter tuning

### Deep learning frameworks



## 实验课

### numpy实现

### pytorch实现

1. autograd库
   - set requires_grad=True
2. Gradient
   - .backward(Tensor);
3. Neural networks
   - typical procedure
     - 定义一个有可学习参数的神经网络
     - Iterate over a dataset of inputs
     - process input through the network
     - 清空优化器中的参数
     - 计算损失函数（预测和真实值之间的距离）
     - 在网络参数中反向传播梯度
     - 更新网络权值，如简单地使用$L$
   - 定义一个网络：继承nn.Network