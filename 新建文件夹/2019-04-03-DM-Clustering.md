---
title: DM Clustering
date: 2019-04-03 10:08:00
categories:
- Data Mining
tag
- dm-notes
---

概论



硬聚类和软聚类

Fuzzy C-means



multivariate gaussian models

- 高斯混合分布的目标是最大化如下目标函数。聚类的过程就是
  $$
  \begin{align}
  p(X) = \prod_ip(x_i) 
  \end{align}
  $$
  对连乘的问题，通常取该函数的log：
  $$
  logp(X)=\sum_i log\pi_cN(x_i|\mu_c,\Sigma_c)
  $$
  

EM算法求解

- E- step

  - 对于每个样本$x_i$，计算它属于簇c的概率$r_{ic}$

    - 计算它在模型c下的概率

    - 归一化
      $$
      r_{ic} = \frac{\pi_cN(x_i;\mu_c,\Sigma_c)}{\sum_{c'}}
      $$
      

- M-stap