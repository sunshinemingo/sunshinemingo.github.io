---
layout:     post
title:      "《Hybrid Neural Networks for Learning the Trend in Time Series》读书笔记"
subtitle:   "Analysis of the real usage and impact of GitHub reactions"
date:       2019-10-24
author:     "Haoyue"
header-img: "img/post-bg-time-series.jpg"
tags:
    - 读书
    - 生活
---

## 《Hybrid Neural Networks for Learning the Trend in Time Series》读书笔记

这是一篇发表第二十六届国际人工智能联合会议论文集中的论文。文中提出了一种端到端的混合神经网络——TreNet，该网络通过学习局部和全局的上下文特征来预测时间序列的变化趋势。TreNet利用卷积神经网络(CNN)从时间序列的局部原始数据中提取显著特征。同时，考虑到历史趋势序列中存在的长期依赖性，TreNet利用长-短期记忆递归神经网络(LSTM)来解决这种依赖性。然后，特征融合层融合CNN和LSTM中的特征来预测趋势。实验结果表明，TreNet对于时间序列中的子序列的预测性能较好。

## 一、Background
时间序列是按时间顺序进行排序的数据点序列。在许多应用领域，预测时间序列的趋势非常有用，例如智能能源领域和股票市场。但是对于特定数据点的传统预测只能提供很少的关于时间序列的潜在语义和动态信息。传统的时间序列趋势学习方法主要是隐马尔可夫模型和多步预测。
预测时间序列的趋势需要考虑到数据不同方面的信息。一方面，时间序列的趋势变化是历史趋势的序列，它具有时间序列长期的语义信息，会影响后续趋势的变化。另一方面，时间序列最近的原始数据点代表了时间序列的局部行为，也影响了后续趋势的变化，同时对突然变化的趋势具有特别的预测能力。
由于神经网络的发展，卷积神经网络(CNN)和循环神经网络(RNN)被用于时间序列的不同任务中。RNN具有很强的序列相关性，特别是长短期记忆网络(LSTM)，由于其内部的记忆机制，对具有长期相关性的序列数据具有良好的性能。CNN通过加强神经元之间的局部连接，能够从时间序列的原始数据中提取局部的显著性特征。
关于混合神经网络在时间序列数据的预测趋势问题的使用，目前相关研究还不是很多。

## 二、TreNet
文中只针对单变量时间序列预测子序列的趋势，通过扩充训练数据，可以将TreNet推广到多元时间序列。TreNet的思想是将CNN和LSTM结合起来，以利用它们在数据不同方面的表示能力，并学习用于趋势预测的联合特征。
TreNet学习的预测函数的输入由两部分组成，分别是LSTM从历史数据趋势中学习到的历史趋势变化的依赖关系和CNN从局部数据集中提取到的局部特征，然后特征融合层合并上述两个特征，预测子序列的趋势。

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_31.png)

#### Learning the dependency in the historical trend sequence
在训练阶段，历史趋势序列中的每个趋势的时间以及斜率都被输入到TreNet中的LSTM层，LSTM层中的每个神经元在每一时间步都保持一个记忆。记忆单元通过遗忘部分现有的记忆，增加新的记忆内容来进行更新，遗忘门调节现有记忆被遗忘的程度，输入门对新的记忆内容被添加到记忆单元中的程度进行调节。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_32.png)
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_33.png)

#### Learning local features from raw data of time series
当第k个历史趋势被输入到LSTM层中时，相应的局部原始数据被输入到TreNet的CNN层中。CNN层是由H个一维卷积层、激活层和池化层组成的堆叠层组成。每一层有特定的滤波尺寸和特定数量的滤波器，每个滤波器扫描整个数据集，提取局部特征。TreNet中CNN层的输出是最后一层H上最大池化的输出的串接。

#### Feature fusion and output layers
特征融合层结合LSTM层和CNN层的输出表示得到联合特征，然后将联合特征输入到输出层中进行趋势的预测。其中，LSTM层和CNN层的输出需要映射到相同的特征空间，然后相加得到特征融合层的激活。输出层是在特征融合层后的全联接层。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_34.png)

为了训练TreNet，文中使用了均方误差函数和正则化项。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_35.png)

代价函数是可微的，TreNet的结构允许损失函数的梯度反向传播到LSTM和CNN部分。

## 三、Experiment
实验中使用的数据集为**Power Consumption**(PC)、**Gas Sensor**(GasSensor)、**Stock Transaction**(Stock)。其中PC数据集使用电压序列，GasSensor数据集使用空气中乙烯和甲烷的混合气体时间序列。
为了便于实验结果的解释，趋势的斜率由有界值范围[-90，90]中的角度表示。趋势的持续时间通过趋势所覆盖的数据点数来衡量。将历史趋势序列、局部原始数据和目标趋势结合起来，对每个时间序列子序列进行数据实例构建。实验中对数据实例进行随机变换，其中10%用于测试数据集，其他的用于交叉验证。PC数据集中有42279个实例，GasSensor数据集中有4418个实例，Stock数据集中有10014个实例。
与TreNet进行对比的模型有**CNN**、**LSTM**、**ConvNet+LSTM(CLSTM)**、**SupportVectorRegression(SVR)**、**Pattern-based Hidden Markov Model(pHMM)**和**Naive**。
使用的评价指标是**RMSE(Root Mean Square Error，即均方根差)**。RMSE值越低，说明预测得越准确。此外，为了防止训练中出现过拟合，使用了dropout和L2正则化控制神经网络的容量。对于训练数据中需要局部原始数据的方法(例如SVRBF，SVPOLY，SVSIG和TreNet)，通过交叉验证选择局部数据集的大小。

实验中首先研究了TreNet与其他模型的预测性能。结果表明，TreNet在时间与斜率预测方面始终优于其他模型，最多可以减少约30％的误差。与CNN和LSTM模型相比，结果证明TreNet的混合结构能够利用CNN和LSTM捕获的信息来提高性能。由于HMM(隐马尔可夫模型)的表示能力有限，pHMM方法的性能较差。此外，在斜率预测方面，基于SVR的方法可以得到与TreNet差不多的结果。

接下来，实验中分析了局部数据大小对于预测性能的影响。对于输入数据包含本地时间序列数据的方法(例如SVRBF，SVPOLY，SVSIG和TreNet)，实验中调整了局部数据的大小，然后观察预测误差。结果显示，与其他模型相比，TreNet在不同窗口大小的时间预测中具有最低的误差。当窗口的尺寸增加，TreNet的预测误差逐渐减小并趋于稳定，这是因为TreNet的特征融合和输出层选择性地关注具有强大预测能力的特征，并且提供压倒性的本地数据只会带来微不足道的改进。

最后，我们使用来自每个数据集的样本测试数据实例进行趋势预测的可视化。结果显示，TreNet的预测结果与实际的趋势最为接近。TreNet的融合层选择性地使用来自CNN和LSTM的互补信息，因此尽管之前存在连续的上升趋势，但是仍然可以观察到对于局部的突然变化的数据，TreNet还是成功地预测了变化趋势。

## 四、Conclusion
文中提出了TreNet模型，一种利用了局部原始数据和相关历史趋势序列的混合神经网络，能够较好地预测时间序列的趋势发展。混合神经网络可以利用互补信息来增强预测性能。此外，这种体系结构是通用的且具有可扩展性，将其他外部时间序列提供给TreNet，就可以进行其他时间序列的预测。
