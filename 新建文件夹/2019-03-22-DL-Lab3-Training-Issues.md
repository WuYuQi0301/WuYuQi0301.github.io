---
title: DL-Lab3 Training Issues
date: 2019-03-22 22:30:00
categories:
- Deep Learning
mathjax: true
tag:
- dl-lab
---

使用数据MNIST和一个简单三层结构体验如何训练一个网络。



MNIST dataset

# CNN

## Define a FeedForward Neural Network

Activation Funciton

- ReLU
- Sigmoid

Input & Output

- Inputs：对于每个batch都有：

  > [B,C,H,W\] = [batch_size, channels, height, width]

- Output：每幅图像的预测分数

  > [batch_size, classes]

- Network structure

  ```
      Inputs                Linear/Function        Output
      [128, 1, 28, 28]   -> Linear(28*28, 100) -> [128, 100]  # first hidden lyaer
                         -> ReLU               -> [128, 100]  # relu activation function, may sigmoid
                         -> Linear(100, 100)   -> [128, 100]  # second hidden lyaer
                         -> ReLU               -> [128, 100]  # relu activation function, may sigmoid
                         -> Linear(100, 100)   -> [128, 100]  # third hidden lyaer
                         -> ReLU               -> [128, 100]  # relu activation function, may sigmoid
                         -> Linear(100, 10)    -> [128, 10]   # Classification Layer                                                          
  ```


- 源码共读之定义一个神经网络类：

  - *_\_init_\_*初始化函数中将网络是否使用dropout/bn、每一层网络的函数和激活函数都定义为*self*的一个属性/方法。

  - *forward*前向传播函数（手动网络）将各层的输出扔进本层的激活函数中，再将激活函数的输出扔进下一层的输入

  - *set_use_dropout*设置是否使用dropout

  - *set_use_bn*设置是否使用bn

  - *get_grad*获得隐藏23层的梯度，且看一下函数：==?==

    ```python
    hidden2_average_grad = np.mean(np.sqrt(np.square(self.hidden2.weight.grad.detach().numpy())))
    ```

    

## Training

1. Pre-set hyper -parameters

   - 学习率，通常从比较大的数值开始例如 1e-1, 1e-2, 1e-3；

   - n_epochs：第一次训练需要设得大一点让模型能收敛；

   - batch_size：常用2的指数倍；较大的batch size能更好的利用GPU，且收敛所需的epoch更少

   - 除了这三个的input_size, output_size由数据集和期望输出决定 ,而 hidden_size, l2_norm = 0 , dropout , get_grad等等都是可选择项

   - 定义了这些数据（以及之前的网络类）之后就能得到对应生成的网络对象*model*

   - 从nn中拿的交叉熵损失函数*loss_fn*

     ```python
     loss_fn = torch.nn.CrossEntropyLoss()
     ```

   - 优化器*potimizer*

     ```python
     optimizer = torch.optim.SGD(model.parameters(), lr=learning_rate, weight_decay=l2_norm) 
     ```

     

2. Initialize model parameters

   - different methods to initialize parameters: [初始化方法源码](https://pytorch.org/docs/stable/_modules/torch/nn/init.html)，从名字上可以直观地知道用的是哪一个方法[原理传送门]()

     ```python
     torch.nn.init.normal_
     torch.nn.init.uniform_
     torch.nn.init.constant_
     torch.nn.init.eye_
        torch.nn.init.xavier_uniform_
        torch.nn.init.xavier_normal_
        torch.nn.init.kaiming_uniform_
     ```

   - 默认使用*uniform*，如果需要使用其他初始化方法需要reset一下

     ```python
     def weight_bias_reset(model):
         for m in model.modules():
             if isinstance(m, nn.Linear):
                 # initialize linear layer with mean and std
                 mean, std = 0, 0.1 
                 # Initialization method
                 torch.nn.init.normal_(m.weight, mean, std)
                 torch.nn.init.normal_(m.bias, mean, std)
     ```

   - 结果如图

     |      | uniform                                                      | normal                                                       | constant                                                     | xavier_uniform                                               | xavier_normal                                                |
     | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
     | 原型 | torch.nn.init.uniform_(m.weight,a=0,b=1)                     | torch.nn.init.normal_(m.weight, mean=0, std=1)               | torch.nn.init.constant_(m.weight,val 1)                      | torch.nn.init.xavier_uniform_(m.weight, gain=1)              | torch.nn.init.xavier_normal_(m.weight, gain=1)               |
     | 结果 | ![DQGRX.jpg](https://ww2.yunjiexi.club/2019/04/04/DQGRX.jpg) | ![DQbkz.jpg](https://ww1.yunjiexi.club/2019/04/04/DQbkz.jpg) | [![DQmZP.jpg](https://ww2.yunjiexi.club/2019/04/04/DQmZP.jpg)](https://www.hualigs.cn/image/DQmZP) | ![DQRiV.jpg](https://ww1.yunjiexi.club/2019/04/04/DQRiV.jpg) | ![DQN9k.jpg](https://ww2.yunjiexi.club/2019/04/04/DQN9k.jpg) |

     

3. repeat over certain numbers of epoch

   - 步骤：

     1. Shuffle whole training data
     2. For each mini-batch data
        - load mini-batch data
        - compute gradient of loss over parameters
        - update parameters with gradient descent

   - 预处理、提取数据集信息、加载数据

     - preprocessing data  [TORCHVISION.TRANSFORMS](https://pytorch.org/docs/stable/torchvision/transforms.html)

       ```python
       train_transform = transforms.Compose([
           transforms.ToTensor(), #将图像或者np的矩阵转换为tensor
           transforms.Normalize((0.1307,), (0.3081,)) #将tenor归一化为(mean, std)
       ])
       ```

     - use MNIST provided by torchvsion   *root =  root dir for processed/training.pt*

       ```python
       train_dataset = torchvision.datasets.MNIST(root='./data/MNIST', 
                                   train=True, 
                                   transform=train_transform,
                                   download=False) 
       ```

       > 此时*train_dataset*对象中并不保存真正的训练数据而是数据集而是数据集的属性：
       >
       > ```
       > Dataset MNIST
       >     Number of datapoints: 60000
       >     Split: train
       >     Root Location: ./data/MNIST
       >     Transforms (if any): Compose(
       >                              ToTensor()
       >                              Normalize(mean=(0.1307,), std=(0.3081,))
       >                          )
       >     Target Transforms (if any): None
       > ```

     - load data ：

       ```python
       train_loader = torch.utils.data.DataLoader(dataset=train_dataset, 
                                                  batch_size=batch_size, 
                                                  shuffle=False)
       ```

   - 计算损失函数对参数的梯度&用梯度更新参数值

     - *train*：

       - 输入train_loader,model, loss_fn, potimizer, get_grad；输出loss，average_grad2，average_grad3

       - 重要的函数：清空优化器中的梯度，将batch生成器得到的data扔进model得到output，计算损失函数得到loss，反向传播backward计算损失函数对各模型参数的梯度，优化器更新参数。

         ```python
         for batch_idx, (data, target) in enumerate(train_loader):
             optimizer.zero_grad() # clear gradients of all optimized torch.Tensors'
             outputs = model(data) # make predictions 
             loss = loss_fn(outputs, target) # compute loss 
             loss.backward() # compute gradient of loss over parameters          
             optimizer.step() # update parameters with gradient descent 
         ```

         

     - *evaluate*：

       - 输入loader, model, loss_fn

     - *fit*：

   - 保存模型：[SAVING AND LOADING MODELS](https://pytorch.org/tutorials/beginner/saving_loading_models.html)

     保存模型学习参数和优化器学习参数


# ResNet

使用网络层来学习残差mapping，解决“随着网络加深accuracy不下降的问题”

### SE-ResNet

global pooling（全局池化层）将c\*h\*w的输出变为c\*1\*1的输出；FC：全连接层；两层FC之间使用ReLU作为激活函数.通过两层FC后使用sigmoid激活函数激活.最后将得到的c个值与原输入c\*h\*w按channel相乘,得到c\*h\*w的输出.

# VggNet