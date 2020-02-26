---
title: DL detection
date: 2019-04-11 13:00:00
categories:
- Deep Learning
tag:
- dl-notes
---







R-CNN

- step
  - use CNN or CNN+SVM
  - 得到初始区域之后需要微调（优化），于是进行regression。Fine-tune bouding box location and size 
  - 输出的数量C+4
- disadvantage
  - 慢

Fast R-CNN

- step
  - 先进行CNN再在输出的feature map上提取特征
  - **Rol Pooling**，将输出的不同大小的区域归于一个同样大小$S\times S$的矩形区域作为下一个全连接层的输入
  - 最花时间的：找初始的可能位置（region proposal）。于是将这一块分离出来，通过一个新的网络RPN，输入CNN后的feature map，输出feature map中有多少个可能的区域及区域大小和位置。



Feature pyramid network（FPN）

- step
  - FIP 将一个原始图像分为尺度金字塔，并将金字塔中的每一层都输入神经网络，得到多个预测结果，最后将结果集成。
  - FPN，从高层开始传导的结果，加入中层，再次传导，再加入低层
- advantage：不仅仅使用最后一个尺度的信息，准确率高（semantically strong）
  - 是一个结构，可以独立于backbone网络（如ResNet）
  - 可用于RPN, fast r-cnn
- disadvantage：慢，不适合实时任务



Mask R-CNN

- step

  - Fast R-CNN+FCN
  - $K$通道，feature map的个数
  - training loss $L = L_{cls}$
  - 解耦分割和验证任务，不使用softmax使得概率和为1，一个像素可能属于两个物体，

  - 不取整操作



YOLO

- step



SSD single shot 

- step
  - 分成





- 在每一个可能的位置都取多个尺度或者多个size的，很多都是背景区域，使得物体区域与背景区域的比例严重失衡。不同类数据量不一样严重影响性能；

- solution：新损失函数，focal loss

  - 思路：如果现有的网络已经能够比较好得预测某些数据，在计算损失函数的时候就降低这部分数据的影响，自动多从“难样本”中进行学习；

  - CE:交叉熵损失 FC:focal loss $p_t$模型预测
    $$
    CE(p_t) = -log(p_t)\\
    FL(p_t)=-（1-p_t）^\gamma
    $$



RetinaNet

- ResNet+FPN+2FCNs



FCOS

- 省去了超参数设置
- 对于每一个像素都预测四个只，对于物体四个周边的距离