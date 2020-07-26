Homework 7

一. GraphSGAN框架理解

本文研究了生成对抗网(GANs)在图半监督学习中的潜力，GraphSGAN是运用对抗生成网络（GAN）对图进行半监督学习的一个新框架。

与GraphGAN相比，加入了半监督学习，从而将性能更加有效的提升。GraphGAN是生成对抗网络模型（Generative Adversarial Network）在图表征学习中的最新进展。GANs最初是为了生成图像而设计的，通过训练两个玩最小-最大游戏的神经网络：判别器discriminator D试图区分真实样本和假样本，生成器generator G试图生成“真实”样本来欺骗判别器discriminator。GraphSGAN则在此基础上加入了半监督学习，通过加入部分人工标记的数据学习特征模型，提升了性能。

图的半监督学习在理论和实践上都受到广泛关注。 GraphSGAN框架解决的问题是：有由一小组有标记的节点和一组无标记的节点组成的图，如何学习一个模型，可以预测无标记节点的标记。

GraphSGAN将会把小部分有标记节点的图映射到特征空间，学到一个表示，并共同训练生成器网络和分类器网络。

D是分类器，G是生成器，分类器和生成器形成了多次感知机模型。生成器以在输入层和全连接层加入随机层，生成器以高斯噪声作为输入.

论文在GitHub中代码选用core数据集。Cora数据集由机器学习论文组成，是近年来图深度学习很喜欢使用的数据集。在数据集中，论文分为以下七类之一:基于案例、遗传算法、神经网络、概率方法、强化学习、规则学习、理论。
论文的选择方式是，在最终语料库中，每篇论文引用或被至少一篇其他论文引用。整个语料库中有2708篇论文。在词干堵塞和去除词尾后，只剩下1433个独特的单词。文档频率小于10的所有单词都被删除。cora数据集包含1433个独特单词，所以特征是1433维。0和1描述的是每个单词在paper中是否存在。变量data是个scipy.sparse.csr.csr_matrix，类似稀疏矩阵，输出得到的是矩阵中非0的行列坐标及值。

二. 作业复现结果：

Eval: correct 824 / 1000, Acc: 82.40

论文中是83±1.3%。

结果在论文区间。

三. 改进工作

参数调整。经过修改LR参数调整，提高lr=0.004，performance反而变低，降低lr到0.002，performance无变化。

可以考虑改进数据集预处理质量，增进模型的表现。本部分工作以后继续开展。

四. 损失分析

loss_supervised = 0.0026, loss_unsupervised = 0.3886, loss_gen = 0.4801 

loss_supervised = 0.0041, loss_unsupervised = 0.3513, loss_gen = 0.4761

在模型中，有监督的损失占5%以下，无监督的占36%左右，二者占近半，gen的损失也占比近半。无监督和gen部分还有较高的提升空间，有监督的损失较小，模型拟合较好。

五. 实现细节

主要有两个bug花费较多时间，一是版本问题导致，后来修改成功；二是电脑是CPU版本的，没有CUDA，修改代码为CPU版本；后用亚马逊EC2运行有CUDA的代码也成功运行。

六. 收获

学会了CPU和CUDA代码转换；python版本问题解决，等。

七. 不足与待改进之处

作业八训练比预期耗费了太多时间，本来预期两天训练完毕，用三天时间再深入研究论文和代码，写好两份作业的报告，但作业八的复现实际训练五天，导致作业七只用了一天训练，半天时间看论文写报告，论文只看了一遍，理解深度不够，代码也没有仔细研究，模型理解不透彻，需要接下来的几天继续深入研究，看看是否能够找到改进模型，提高performance的方法。

感谢老师的批阅，请随时指正，感谢！

