---
title: 深度学习周报week01&week02
date: 2018-03-30 22:16:12
categories:
- deep learning
tags:
- TensorFlow
toc: true
---



# Week1

## 配置Cuda、Cudnn和Tensorflow

要注意**版本对应**

<!-- more -->

## 学习基础知识

### [神经网络基本原理](http://www.ruanyifeng.com/blog/2017/07/neural-network.html)

#### 感知器

![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017071202.png)
一个圆圈表示一个感知器，x1、x2、x3...为输入，output为对应的输出。为了简化问题，output只取0或1.

#### 权重和阈值

![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017071203.png)
threshold为阈值，xi为输入，wi为对应的权重，表示输入的重要性。

#### 矢量化

- 将输入x1,x2,x3,...写为矢量**x**: < x1,x2,x3,... >
- 将权重w1,w2,w3,...写为矢量**w**: < w1,w2,w3,... >
- 则 **w·x** = ∑ wx
- 设 b 等于负的阈值 b = -threshold
  ![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017071206.png)

#### 实际的决策模型

多个感知器组成的多层网络：
![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017071205.png)

#### 神经网络的运作过程

- 确定输入和输出
- 找到一种或多种算法，可以从输入得到输出（决定决策模型）
- 找到一组已知答案的数据集，用来训练模型，估算w和b
  **估算w和b：试错法**
  首先获取一组随机的**w**和**x**，将**w**（或**b**）进行微小变动，记作**Δw**（或**Δb**），然后观察输出有什么变化。不断重复这个过程，直至得到对应最精确输出的那组**w**和**b**，就是我们要的值。这个过程称为**模型的训练**。
- 一旦新的数据产生，输入模型，就可以得到结果，同时对w和b进行校正

#### 输出的连续性

为了保证能观察到**w**和**b**的微小变化对结果造成的影响，必须将"输出"改造成一个连续性函数。一般使用**sigmoid**函数。

- 将output记为z：`z = wx + b` 
- 则结果的sigmoid函数为σ(z)：`σ(z) = 1 / (1 + e^(-z))`

![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017071209.png)
实际上，Δσ满足下面的公式：
![](http://www.ruanyifeng.com/blogimg/asset/2017/bg2017071210.png)
即Δσ和Δw和Δb之间是线性关系，变化率是偏导数。这就有利于精确推算出w和b的值了。

### 反向传播（BP）

- 即估算**w**和**b**的**试错法**的具体实现。
- 反向传播算法主要由两个过程（**激励传播、权重更新**）反复循环迭代，直到结果误差在可容忍的限度结束。

#### 激励传播

每次迭代中的传播环节包含两步：

1. 前向传播阶段——按照当前**w**和**b**计算**output（激励响应）**；
2. 反向传播阶段——将**output**和目标输出求差，从而获得隐层和输出层的**响应误差**。

#### 权重更新

对于每个权重 **wi **，按照以下步骤进行更新：

1. 将**输入激励**和**响应误差**相乘，从而获得权重的**梯度**；
2. 将这个梯度乘上一个比例并取反后加到权重上。
3. 这个比例将会影响到训练过程的速度和效果，因此称为“**训练因子**”。梯度的方向指明了误差扩大的方向，因此在更新权重的时候需要对其取反，从而减小权重引起的误差。

<u>关于算法推导（**梯度下降+链式求导**），网上的博客质量良莠不齐，因此打算等买的书到了之后再研究一下，这里就不再列出。</u>

### 卷积神经网络（CNN）

- **卷积神经网络**由三部分构成：
  - 第一部分是输入层。
  - 第二部分由n个卷积层和池化层的组合组成。
  - 第三部分由一个全连结的多层感知机分类器构成。
- **卷积神经网络**与**普通神经网络**的区别在于，卷积神经网络包含了一个特征抽取器（即第二部分）。
- **卷积神经网络**的卷积层中，一个神经元只和部分邻层神经元连接。
- 在每一个**卷积层**中，通常包含若干个**特征平面(feature map)，**每个特征平面由一些**矩形排列**的的神经元组成，同一特征平面的神经元共享权值，这里共享的权值就是**卷积核**。
- **卷积核**一般以随机小数矩阵的形式初始化，在网络的训练过程中卷积核将通过学习得到合理的权值（**反向传播**）。共享权值（卷积核）带来的直接好处是减少网络各层之间的连接（**减少参数**），同时又降低了**过拟合**（参数过多导致）的风险。
- **子采样**也叫做池化（pooling），也可以认为是下采样，通常有均值子采样（mean pooling）和最大值子采样（max pooling）两种形式。

<u>与**普通的神经网络**相比，包含**卷积和子采样**的**卷积神经网络**大大**简化了模型复杂度，减少了模型的参数**。</u>

#### **局部连接**

假设一张图大小为n1\*n2，一个卷积核的大小为m1\*m2，对于卷积后生成的每一个数据xi，它都是原图中对应位置的m1\*m2矩阵和这个卷积核对应点相乘求和得到的。

也就是说xi只和原图中对应的m1\*m2的那个矩阵中的元素连接，而不是和整张图的n1\*n2个元素连接。

因此局部连接使得参数数量变为全连接的（m1\*m2）/（n1\*n2）。

#### 权值共享

即对于一个卷积核遍历原数据矩阵，生成的一个新的数据矩阵的每一个元素来说，它们的权值都为这个卷积核。

这样就导致了权值数几乎变为了不权值共享时的数据量分之一。

#### 多卷积核

用一个卷积核对整张图卷积可以看作是提取了原图的一个特征。

使用一个卷积核只提取了一个特征，因此为了充分的提取特征，要使用多个卷积核，得到多个特征平面。

#### 下采样（池化）

当输入数据过多时，参数的量就不可避免的变得很多，为了防止参数过多导致过拟合，需要下采样。

## 常见网络结构了解

### LeNet


![LeNet](https://upload-images.jianshu.io/upload_images/3352522-2ef0a2bbb096ced0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

1. **Input Layer**：1\*32\*32图像
2. **Conv1 Layer**：包含6个卷积核，kernal size：5\*5，parameters:（5\*5+1）\*6=156个
3. **Subsampling Layer**：average pooling，size：2\*2, Activation Function：sigmoid
4. **Conv3 Layer**：包含16个卷积核，kernal size：5\*5
5. **Subsampling Layer**：average pooling，size：2\*2
6. **Conv5 Layer**：包含120个卷积核，kernal size：5\*5
7. **Fully Connected Layer**：Activation Function：sigmoid
8. **Output Layer**：Gaussian connection

### AlexNet

#### AlexNet结构图

![ImageNet](https://www.52ml.net/wp-content/uploads/2016/08/alexnet.png)

#### AlexNet结构精简版

![ImageNet](https://www.52ml.net/wp-content/uploads/2016/08/alexnet2.png)

对比一下即可理解精简版中**符号的含义**（以第一层为例）：

卷积核大小为11\*11，共有96个卷积核，步长为4，下采样矩阵大小为2\*2。

fc：full connect，全连接。

激活函数变为ReLU：斜坡函数 f(x) = max(0, x)及其变种。

### VGG

#### VGG结构图

![VGG](https://www.52ml.net/wp-content/uploads/2016/08/vgg.png)

#### VGG-19网络结构精简版

![VGG-19](https://www.52ml.net/wp-content/uploads/2016/08/vgg19.png)

### GoogLeNet

[讲解链接](https://blog.csdn.net/shuzfan/article/details/50738394)

主要特征是**重新启用全连接**以及提出了**网中网**的结构。

网上的博客写的都比较粗略，有时间看一下相关资料或者论文。

### ResNet

#### 残差网络模型

主要的创新为残差网络，本质上是要解决层次比较深时无法训练的问题：

![residual](https://www.52ml.net/wp-content/uploads/2016/08/residual.png)

#### ResNet网络结构

![resnet](https://www.52ml.net/wp-content/uploads/2016/08/resnet.png)

### DenseNet

#### DenseNet网络结构

![DenseNet](https://tse4.mm.bing.net/th?id=OIP.m8LpfrnNS-bVUC8gil9eVwHaBD&pid=Api)

#### Dense Block结构

![Dense Block](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522436431455&di=3f3b62ddecf7accdb6b2b756f942cf89&imgtype=0&src=http%3A%2F%2Fimage.bubuko.com%2Finfo%2F201802%2F20180217134048130090.png)

还是只看懂了大概，需要后续学习。

## 利用LeNet5网络模型实现MNIST手写数字识别

主要的关键点是熟悉TensorFlow相关变量和含义

完成TensorFlow官方MINIST识别教程。

使用[国内网站](http://wiki.jikexueyuan.com/project/tensorflow-zh/tutorials/mnist_pros.html)来更方便的浏览。

------

# Week2

## 利用VGG16实现CIFAR-10动物分类

[教程页链接](http://wiki.jikexueyuan.com/project/tensorflow-zh/tutorials/deep_cnn.html)

## 学习使用[TensorBoard](https://github.com/jikexueyuanwiki/tensorflow-zh/blob/master/SOURCE/how_tos/summaries_and_tensorboard/index.md)

## 了解Batch Normalization(BN)批标准化

文献链接：[Batch Normalization: Accelerating Deep Network Training by Reducing  Internal Covariate Shift](https://arxiv.org/pdf/1502.03167.pdf)



> 在网络的每一层输入的时候，又插入了一个归一化层，也就是先做一个归一化处理，然后再进入网络的下一层。不过文献归一化层，可不像我们想象的那么简单，它是一个可学习、有参数的网络层。

