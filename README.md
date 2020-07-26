Homework 7

一. GraphSGAN框架理解

本文研究了生成对抗网络(GANs)在图半监督学习中的潜力，GraphSGAN是运用对抗生成网络（GAN）对图进行半监督学习的一个新框架。与GraphGAN相比，加入了半监督学习，从而将性能更加有效的提升。

在此前的研究中，GraphGAN是生成对抗网络模型GAN在图表征学习中的最新进展。GANs最初是为了生成图像而设计的，通过训练两个玩最小-最大游戏的神经网络：判别器discriminator D试图区分真实样本和假样本，生成器generator G试图生成“真实”样本来欺骗判别器discriminator。

GraphSGAN则在这些框架基础上加入了半监督学习，通过加入部分人工标记的数据学习特征模型，提升了性能。GraphSGAN框架解决的问题是：有由一小组有标记的节点和一组无标记的节点组成的图，如何学习一个模型，可以预测无标记节点的标记。GraphSGAN将会把小部分有标记节点的图映射到特征空间，学到一个表示，并共同训练生成器网络和分类器网络。由生成器生成伪输入，通过拼接原始特征wi和学习嵌入qi得到真实输入。将真实输入和由生成器生成的伪样本输入到分类器中，实现分类目标。

论文在GitHub中选用core数据集，该由机器学习论文组成，数据集被使用的目标是把所有的论文分类到正确的类别。论文分为七类。论文的选择方式是，每篇论文引用或被至少一篇其他论文引用。整个语料库中有2708篇论文。在词干堵塞和去除词尾后，只剩下1433个独特的单词。文档频率小于10的所有单词都被删除。cora数据集包含1433个独特单词，所以特征是1433维。0和1描述的是每个单词在paper中是否存在。

模型使用500个节点的early stop来训练模型进行验证(Cora上平均20个epoch)。每个epoch包含100批，批大小为64。该方法的性能明显优于所有基于正则化和嵌入的方法，也优于Chebyshev和graph convolution networks (GCN)，同时也略优于GCN 注意力模型(GAT)。与基于卷积的方法相比，GraphSGAN对标注数据更加敏感，因此可以观察到较大的方差。

GraphSGAN的内存消耗在大规模数据中具有较大优势。对于小数据集，GraphSGAN由于结构最复杂，占用的空间最大。但对于大型数据集，GraphSGAN使用的GPU内存最少。

二. 复现结果

Eval: correct 824 / 1000, Acc: 82.40

论文中是83±1.3%。

结果在论文区间。

三. 改进工作

1. 参数调整方面：
   由后面的损失分析可以看出，loss_unsupervised较大，因此我们将原权重参数的子参数 mean(logz_unlabel)由0.5进一步减小到0.3，得到performance的提高。由82.4%的准确率提高到83%。
   LR参数调整方面，提高lr=0.004，performance反而变低，降低lr到0.002，performance无变化。

2. 可以考虑改进数据集预处理质量，增进模型的表现。本论文是用openNE实现，可以考虑用deepwalk或者NetMF尝试实现。本部分工作由于时间分配关系，以及水平关系尚未进行，以后再继续改进、开展。

四. 损失分析

选取第一个和最后一个损失进行分析。如下：

loss_supervised = 0.0026, loss_unsupervised = 0.3886, loss_gen = 0.4801，可以计算得到，pt_loss = 0.1287

loss_supervised = 0.0041, loss_unsupervised = 0.3513, loss_gen = 0.4761，可以计算得到，pt_loss = 0.16.85

从上述损失分布情况可以看出，

在模型中，有监督的损失占5%以下，无监督的占36%左右，gen的损失占比近半，占比最大，pt的损失占14%左右。

表明，无监督和gen部分还有较高的提升空间，有监督的损失较小拟合较好，pt的损失中等。

五. 实现细节

主要有两个bug花费较多时间，一是版本问题导致，后来修改成功；二是电脑是CPU版本的，没有CUDA，修改代码为CPU版本；后用亚马逊EC2运行有CUDA的代码也成功运行。

六. 收获

学会了CPU和CUDA代码转换；python版本问题解决，等等。

七. 不足与待改进之处

本次作业在时间分配和安排上出现问题。作业七投入的时间少于应有时间。

主要由于作业八训练比预期耗费了太多时间，本来预期两天训练完毕，用三天时间再深入研究论文和代码，写好两份作业的报告，但作业八的复现实际训练五天，导致作业七只用了一天，从而导致对GraphSGAN的理解深度不够，需要接下来的几天继续深入研究，看是否能够找到改进模型提高performance的方法。

感谢老师的批阅，请随时指正，感谢！

