### 第6章 多层感知器入门

神经网络很神奇，但是一开始学起来很痛苦，涉及大量术语和算法。本章主要介绍多层感知器的术语和使用方法。本章将：

- 介绍神经网络的神经元、权重和激活函数
- 如何使用构建块建立网络
- 如何训练网络

我们开始吧。

#### 6.1 绪论

本节课的内容很多：

- 多层感知器
- 神经元、权重和激活函数
- 神经元网络
- 网络训练

我们从多层感知器开始谈起。

#### 6.2 多层感知器

（译者注：本书中的“神经网络”一般指“人工神经网络”）

在一般的语境中，```人工神经网络```一般指```神经网络```，或者，```多层感知器```。感知器是简单的神经元模型，大型神经网络的前体。这个领域主要研究大脑如何通过简单的生物学结构解决复杂的计算问题，例如，进行预测。最终的目标不是要建立大脑的真正模型，而是发掘可以解决复杂问题的算法。

神经网络的能力来自它可以从输入数据中学习，对未来进行预测：在这个意义上说，神经网络学习了一种对应关系。数学上说，这种能力是一种有普适性的近似算法。神经网络的预测能力来自网络的分层或多层结构：这种结构可以找出不同尺度或分辨率下的不同特征，将其组合成更高级别的特征。例如，从线条到线条的集合到形状。

#### 6.3 神经元

神经网络由人工神经元组成：这些神经元有计算能力，使用激活函数，利用输入和权重，输出一个标量。

![6.1 简单的神经元](https://i.imgur.com/Lwy7Hpy.png)

##### 6.3.1 神经元权重

线性回归的权重和这里的权重类似：每个神经元也有一个误差项，永远是1.0，必须被加权。例如，一个神经元有2个输入值，那就需要3个权重项：一个输入一个权重，加上一个误差项的权重。

权重项的初始值一般是小随机数，例如，0~0.3；也有更复杂的初始化方法。和线性回归一样，权重越大代表网络越复杂，越不稳定。我们希望让权重变小，为此可以使用正则化。

##### 6.3.2 激活函数

神经元的所有输入都被加权求和，输入激活函数中。激活函数就是输入的加权求和值到信号输出的映射。激活函数得名于其功能：控制激活哪个神经元，以及输出信号强度。历史上的激活函数是个阈值：例如，输入加权求和超过0.5，则输出1；反之输出0.0。

激活函数一般使用非线性函数，这样输入的组合方式可以更复杂，提供更多功能。非线性函数可以输出一个分布：例如，逻辑函数（也称为S型函数）输出一个0到1之间的S形分布，正切函数可以输出一个-1到1之间的S形分布。最近的研究表明，线性整流函数的效果更好。

#### 6.4 神经元网络

神经元可以组成网络：每行的神经元叫做一层，一个神经网络可以有很多层。神经网络的结构叫做网络拓扑。

![6.2 简单的神经网络](https://i.imgur.com/3UxEXFE.png)

##### 6.4.1 输入层

神经网络最底下的那层叫输入层，因为直接和数据连接。一般的节点数是数据的列数。这层的神经元只将数据传输到下一层。

##### 6.4.2 隐层

输入节点后的层叫隐层，因为不直接和外界相连。最简单的网络中，隐层只有一个神经元，直接输出结果。随着算力增加，现在可以训练很复杂，层数很高的神经网络：历史上需要几辈子才能训练的网络，现在有可能几分钟就能训练好。

##### 6.4.3 输出层

神经网络的最后一层叫输出层，输出问题需要的值。这层的激活函数由问题的类型而定：

- 简单的回归问题：有可能只有一个神经元，没有激活函数
- 两项的分类问题：有可能只有一个神经元，激活函数是S型函数，输出一个0到1之间的概率，代表主类别的概率。也可以用0.5作为阈值：低于0.5输出0，大于0.5输出1.
- 多项的分类问题：有可能有多个神经元，每个代表一类（例如，3个神经元，代表3种不同的鸢尾花 - 这是个经典问题）。激活函数可以使用Softmax函数，每个输出代表是某个类别的概率。最有可能的类别就是输出最高的那组。

#### 6.5 网络训练

##### 6.5.1 预备数据

预处理一下数据：数据必须是数值，例如，实数。如果某一项是类别，需要通过独热编码将其变成数字：对于N种可能的类别，加入N列，对其取0或1代表是否属于该类别。

独热编码也可以对多个类别进行编码：建立一个二进制向量表示类别，输出的结果可以对类别进行分类。神经网络需要所有的数据单位差不多：例如，将所有的数据缩放到0和1之间，这步叫归一化。或者，对数据进行缩放（正则化），让每列的平均值为0，标准差为1。图像的像素数据也应该这样处理。文字输入可以转化为数字，例如某个单词出现的频率，或者用其他的什么办法转化。

##### 6.5.2 随机梯度下降

随机梯度下降很经典，现在还是很流行。使用的方式是正向传递：每次对网络输入一行数据，激活每层神经元，得出一个输出值。对数据进行预测也用这种方式。

我们把输出和预计值进行比较，算出误差；这个错误通过网络反向传播，更新权重数据。这个算法叫反向传播算法。我们在所有的训练数据上重复此过程，每次网络全部更新叫一轮。神经网络可以训练几十乃至成千上万轮。

##### 6.5.3 权重更新

神经网络的权重可以每次训练都更新，这种方式叫在线更新，速度很快但是有可能造成灾难性结果。或者也可以保存误差数据，最后只更新一次：这种更新叫批量更新，一般而言更稳妥。

因为数据集有可能很大，为了计算速度，每次更新的数据量一般不大，只有几十到几百个数据。权重的更新数量由学习速率（步长）这个参数控制，规定神经网络针对错误的更新速度。这个参数一般很小，0.1或者0.01，乃至更小。也可以调整其他参数：

- 动量：如果上次和这次的方向一样，则加速变化，即使这次的错误不那么大。用于加速网络训练。
- 学习速率衰减：随着训练次数增加而减少学习速率。在一开始加速训练，后面微调参数。

##### 6.5.4 进行预测

训练好的神经网络就可以进行预测了。可以使用测试数据进行测量，看看能不能预测新的数据；也可以部署网络，进行预测。网络只需要保存拓扑结构和最终的权重。将新的数据喂进去，经过前向传输，神经网络就会做出预测了。

#### 6.6 总结

本章关于利用人工神经网络进行机器学习。总结一下：

- 神经网络不是大脑的模型，而是计算模型，可以解决复杂的机器学习问题
- 神经网络由带有权重和激活函数的神经元组成
- 神经网络分层，用随机梯度下降训练
- 应该预处理数据

##### 6.6.1 下一章

你已经了解了神经网络：下一章我们用Keras动手制作第一个神经网络。

