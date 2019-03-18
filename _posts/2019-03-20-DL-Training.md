---
title: DL Training
date: 2019-03-20 20:30:00
categories:
- Deep Learning
---



Discussion on issues in training a network, including gradient exploding& vanishing, mini-batch issue, overfitting, etc.



## A general model training process

> 0. pre-set hyper-parameters
> 1. initialize model parrameters
> 2. repeat over certain number of epochs
>    - shuffle while training data
>    - for each mini-batch data
>      - 加载数据 load mini-batch data
>      - 计算loss函数对参数的梯度 compute gradient of loss over parameters
>      - 用梯度下降更新模型参数 update parameter with gradient descent
> 3. Save model （结构和参数）



## 梯度爆炸/消失

### 问题描述

对于多层网络（如图）而言，有很多隐藏的梯度问题。

例如，在求loss函数对参数$w_1​$的导数时应用链式法则我们有如下式子（注意第一行仅有连乘的最后一项是对模型参数$w​$求导，其他都是对激活函数的输出$h_i​$求导）：
$$
\begin{align}
\frac{\delta l}{\delta w_1} &=\frac{\delta l}{\delta h_l}*(\frac{dh_l}{du_l}*\frac{du_l}{dh_{l-1}})*(\frac{dh_{l-1}}{du_{l-1}}*\frac{du_{l-1}}{dh_{l-2}})*...*(\frac{dh_{1}}{du_1}*\frac{du_1}{dw_1})\\
&=\frac{\delta l}{\delta h_{l}}*(g'(u_l)*w_l)*(g'(u_{l-1})*w_{l-1})*...*(g'(u_1)*x)
\end{align}
$$
梯度爆炸问题的含义就是，如果每个$g'(u_i)w_i​$都大于1，那么$|\frac{\delta l}{\delta w_1}|​$将远大于1；反之则称为梯度消失问题。



### 方法：防止梯度爆炸

梯度爆炸的表现在训练过程不稳定上；若能同时满足$|g'(u_i)|<1​$和$|w_i|<1​$，就能够避免梯度爆炸问题。

1. 通过对激活函数的观察（如图，蓝色曲线为激活函数图像，绿色图像为激活函数导数的图像），已知$|g'(u_i)|<1成立。$

   ![D4gAB.jpg](https://ww1.yunjiexi.club/2019/03/18/D4gAB.jpg)
   

2. 权值

   - **Weight initialization** 权值初始化的时候需要使$|w_i|<1​$

   - 在训练过程中使用 **Weight re-normalization** 

3. rescaling 输入 $x​$ 使得输入满足$|x|<1​$

### 方法：减少梯度消失

梯度消失问题会使得训练过程（梯度下降）变得非常缓慢。为了缓解这个问题，需要让连乘项不要太小：

1. 考虑选择 ReLU 作为激活函数，因为在$u_i>0$时$g'(u_i) = 1$满足条件，而Sigmiod和tanh的导数在正无穷的极限为0；
2. 要使得大多数$|w_i|$不接近于0，$|w_i|$的方差不能太小
   - **Weight initialization** 时主流分布有两种，第一种为均匀分布，$w_i\sim U(-a, a)$；第二种为正态分布$w_i\sim N(0,\sigma^2)$，之后会详细说明。
   - **Weight re-normalization**

### Weight initialization

原则就一句话，不好翻译就不翻译了，大家自由心证一下：

> **Rule**：Signal across layer does not shrink and explode
>
> **or** : variance of signal across layer does not change

#### Xavier's method

Xavier方法有两个假设

1.  激活函数$g(u_{l,k})​$在$u_{l,k}​$比较小的时候是线性的（例如tanh），那么就有

$$
h_{l,k} \approx \sum_{j=1}^{n}h_{l-1}w_{j,k}
$$

2. 输入信号$\{h_{l-1,j}\}​$和权值$w_{j,k}​$都是独立同分布的，且均值为0。*概率与统计中可证，加权和是线性的，方差亦然* ；那么就有
   $$ {align}
   \begin{align}
   Var(h_l,k)&\approx\sum_{j=1}^nVar(h_{l-1,j})Var(w_{j,k})\\
   Var(h_l)&\approx nVar(h_{l-1})Var(w)
   \end{align}
   $$


   为了使得方差序列满足Rule，即$Var(h_l)\approx Var(h_{l-1})​$:
   $$
   Var(w) = \frac{1}{n}
   $$

3. 同时需要 variance of backward gradient signal across layer does not change（网络层级之间的后向梯度信号的方差不变）：
   $$
   Var(w) = \frac{1}{m}
   $$

- 由于网络输入维度$n​$和输出维度$m​$经常是不同的，就折中一下好了。一般来说采样分布选高斯分布或者均匀分布就够用了：

  - 采样自高斯分布

  $$
  E(w) = 0\ \ \ ,\ \ \ \ \ Var(w) = \frac{2}{n+m}
  $$

  - 采样自均匀分布
    $$
    w\sim U[-\frac{\sqrt{6}}{\sqrt{n+m}},\frac{\sqrt{6}}{\sqrt{n+m}}]
    $$
    
#### He's method

刚才说啦，由于Xavier方法做出了假设2，它并不适用于ReLU激活函数（均值一定是正数）。当激活函数变为ReLU的时候，用一下分布比较好：

- 采样自高斯分布
  $$
  E(w) = 0\ \ \ ,\ \ \ \ \ Var(w) = \frac{2}{n}
  $$

- 采样自均匀分布
  $$
  xxxxxxxxxx1 1w\sim U[-\frac{\sqrt{6}}{\sqrt{n}},\frac{\sqrt{6}}{\sqrt{n}}]
  $$

论文指路：*K. He. Zhang, S.Ren, and J.Sun, Delving Deep into Rectifiers: Surpassing Human-Level Performance on ImageNet Classification, 2015*

-------

## Mini-batch Issue

如前所述，mini-batch是将数据集分成若干组进行训练的方法。*这意味着经常会有不同mini-batch数据子集有不同的分布的问题，使得网络的每一层的输入都会有不同的mini-batch分布，且一个minibatch的分布对于一层网络随着时间也会变化，每一层网络都需要连续变化以适应不同的新的分布，mini-batch的方向不一定是合适的方向*。

上面斜体是课件的直接翻译，然而太难读懂了，于是我借用了一下大佬的博客中的表述：==reference==

为了解决这个问题，我们对mini-batch输入做一些处理以使得它们具有相似的数据分布。

### Batch normalization (BN)

对于每一个mini-batch输入$\{x_n\}​$，对于每一维度进行归一化处理，其中$E(x_k)​$和$Var(x_k)​$分别为$\{x_n\}​$所有第k维数据的均值和方差：
$$
\hat{x_k} = \frac{x_k-E(x_k)}{\sqrt{Var(x_k)+\delta}}
$$
直接使用这种归一化会降低每个输入输出的多样性，训练网络变得非常缓慢因为每次的输入都“差不多”，于是大佬们又设计出恢复神经元*representation variety*的方法，即用一个线性函数将归一化后的输入拉开差距：
$$
y_k = \gamma_k\hat{x_k}+\beta_k \equiv BN_{\gamma_k,\beta_k(x_k)}
$$
其中 都独立于mini-batch 数据，使得它们不会影响数据本身。同时，不同的维度可能会有不同的$\gamma_k  ​$和$\beta_k​$。为了确定$\gamma_k  ​$和$\beta_k​$的值，将它们作为模型参数的一部分进行训练，在训练中学习合适的值。

下面两张图说明了BN部分可以放在hidden 内部不同的位置。左边的图将BN放在加和函数之前，右边的放在激活函数之前，实验证明（？）, 对于从非线性的激活函数得到的归一化输入效果并不理想；而右边BN输出结果更近似于一个高斯分布。

![D4nMj.jpg](https://ww1.yunjiexi.club/2019/03/18/D4nMj.jpg)

**Notes**：BN能使得训练过程更快，得到更高的accuracy，但是使用BN的batch_size不能太小，比如4（x

----



## Over-fitting Issue

Over-fitting的问题在函数拟合中又说，这里就不赘述了。为了避免过拟合的问题重点是找到合适数量的model parameter ，既不致使loss非常大，也不至于有过拟合问题。

### Regularization

#### $L_p$ norm

$L_p$ regularization的思路是通过在loss函数上添加一个参数作为”惩罚“（penalty），这个参数服从$L_p$范数，一般比较大，以减少过拟合的可能性：
$$
L(\theta) = \frac{1}{N}\sum_{n=1}^Nl(y,f)+\lambda||\theta||_p
$$
其中：

- $L_p$范数 $||\theta||_p \equiv (\sum_i|\theta_i|p)^{1/p}$
- $\lambda:$ 一个超参数，用于决定范数项的权重
- $p=1$：得到更少的非零权值参数
- $p=2$：得到更小的权值（"weight decay"）

#### Dropout

>  论文指路：Srivastava et al., Dropout: A simple Way to Prevent Neural Networks from Overfitting, 2014

- 思路：在训练时，每个隐藏层神经元有大小为p的概率参与运算；在测试时，每个神经元总是参与运算，且$w' = pw$，这样测试输出就与训练时的期望输出相同。

- 原理：每次训练时，每个神经元都强制通过更少的别的神经元的作用下完成任务；

- 缺点：训练时间是普通网络的2-3倍。



## Ensemble model

当然了，经常来说一个网络是不足以满足实践要求的，通常需要不同的**初始权值**或者不同的**模型架构**得到一个混合网络，最后通过**平均**或者**投票**的方式得到最终预测。









