---
title: CNN知识脉络梳理
tag: deep-learning
date: 2018-05-01 09:32:30
---

这里梳理了一些CNN的知识脉络。

用来构建CNN的各种层

数据输入层/Input layer 
去均值；归一化；PCA/白化
卷积计算层/CONV layer 
局部关联；窗口（receptive field）滑动；参数共享机制；深度/depth、步长/stride、填充值/zero-padding
ReLU激励层/ReLU layer 
Sigmoid、Tanh（双曲正切）、ReLU（优选）、Leaky ReLU（次优选）、ELU、Maxout（次优选）；梯度消失问题
池化层/Pooling layer 
下采样、压缩数据和参数量、减小过拟合；Max pooling、average pooling
全连接层/FC layer
Batch Normalization layer(Option)
输出层/Softmax layer(Option)
CNN训练算法

第一步，先定义Loss function
找到最小化损失函数的W和b，CNN中用的算法是SGD
SGD需要计算W和b的偏导
BP算法是计算偏导用的，其核心是求导链式法则
CNN优缺点

优点： 
- 权重共享，极大减少参数量 
- 特征是训练出来的，无需手动选取 
- 层次深、抽取图像信息丰富，表达效果好 
缺点： 
- 需要调参，需要大样本量，硬件GPU 
- 物理含义不明确

正则化与Dropout

传统机器学习正则化算法：L1、L2正则化
Dropout(随机失活)正则化：防止过拟合；集成学习的思想
典型CNN网络

LeNet(上世纪90年代)、AlexNet(2012)、ZFNet(2013)、GoogLeNet(2014)、VGGNet(2014)、ResNet(2015)

> [呼呼呼山]()(http://code4fun.me)
> 2019-05-01 09:32:30
