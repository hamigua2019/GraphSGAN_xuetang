Homework 7

一. GraphSGAN框架理解

本文研究了生成对抗网(GANs)在图半监督学习中的潜力，GraphSGAN是运用对抗生成网络（GAN）对图进行半监督学习的一个新框架。

与GraphGAN相比，加入了半监督学习，从而将性能更加有效的提升。GraphGAN是生成对抗网络模型（Generative Adversarial Network）在图表征学习中的最新进展。GANs最初是为了生成图像而设计的，通过训练两个玩最小-最大游戏的神经网络：判别器discriminator D试图区分真实样本和假样本，生成器generator G试图生成“真实”样本来欺骗判别器discriminator。GraphSGAN则在此基础上加入了半监督学习，通过加入部分人工标记的数据学习特征模型，提升了性能。

图的半监督学习在理论和实践上都受到广泛关注。 GraphSGAN框架解决的问题是：有由一小组有标记的节点和一组无标记的节点组成的图，如何学习一个模型，可以预测无标记节点的标记。

GraphSGAN将会把小部分有标记节点的图映射到特征空间，学到一个表示，并共同训练生成器网络和分类器网络。

D是分类器，G是生成器，分类器和生成器形成了多次感知机模型。生成器以在输入层和全连接层加入随机层，生成器以高斯噪声作为输入.

二. 作业复现结果：

Eval: correct 824 / 1000, Acc: 82.40

论文中是83±1.3%。

结果在论文区间。

三. 改进工作

参数调整。经过修改LR参数调整，提高lr=0.004，performance反而变低，降低lr到0.002，performance无变化。

四. 损失分析

loss_supervised = 0.0026, loss_unsupervised = 0.3886, loss_gen = 0.4801 

loss_supervised = 0.0041, loss_unsupervised = 0.3513, loss_gen = 0.4761

在模型中，有监督的损失占5%以下，无监督的占36%左右，二者占近半，gen的损失也占比近半。

