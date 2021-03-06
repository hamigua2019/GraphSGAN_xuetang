Homework 7 (2020.7.30)

一. 论文阅读与GraphSGAN理解

在本文的框架出现之前，运用图深度学习进行节点分类的框架是无监督学习的GCN、GAT，它们在cora数据集上准确率表现分别为80.1 ± 0.5、83.0 ± 0.7，具有不错的表现，后来又提出了加入了生成对抗网络GAN的框架加入了GraphGAN，该框架在cora的表现论文中未给出，给出的是其他数据集，但估计也同样具有不错的表现，并有一定的改进。

以上模型基于cora数据集的任务是，将所有的论文分类到正确的类别。cora数据集共有2708篇论文，分为七类。论文的选择方式是，每篇论文引用或被至少一篇其他论文引用。

以上框架都是基于无监督的基础，设想如果对图节点分类加入半监督学习，通过半监督学习提生生成器的performance，从而通过博弈提高判别器的performance，是否有利于图节点分类performance的提高？在cora数据集的任务中，意即，能否通过半监督学习，对一小部分论文标记，学习这部分论文的模型，生成高质量的论文节点，通过生成器和判别器的博弈，提高判别器的准确率，从而提升论文分类的准确性？

答案是肯定的。

在GraphGAN框架中加入了半监督学习Semi-supervised Learning ，构成了GraphSGAN框架，在cora数据集上performance达到83.0 ± 1.3，比GAT提高了0.6%，在Citeseer上更是提高了1.7%。

半监督学习performance提高原动力来自于，生成的假样本具有已标记样本的performance，从而在博弈中提高了判别器对真实样本的判别能力。

二. 论文与框架实现

本模型时间复杂度较低，训练时间较短。

准确率为82.4%。论文中是83±1.3%。基本符合论文中的描述。

Eval: correct 824 / 1000, Acc: 82.40

三. 损失分析

选取第一个和最后一个损失进行分析。如下：

loss_supervised = 0.0026, loss_unsupervised = 0.3886, loss_gen = 0.4801，可以计算得到，pt_loss = 0.1287

loss_supervised = 0.0041, loss_unsupervised = 0.3513, loss_gen = 0.4761，可以计算得到，pt_loss = 0.16.85

从上述损失分布情况可以看出，

在模型中，有监督的损失loss_supervised占5%以下，无监督的损失loss_unsupervised占36%左右，loss_gen的损失占比近半，占比最大，pt_loss的损失占14%左右。

表明，无监督loss_unsupervised和loss_gen部分还有较高的提升空间，有监督的损失较小拟合较好，pt_loss的损失中等。

四. 模型改进

1. 参数调整后，performance达到83%，提高了0.6%。

*  由损失分析可以看出，loss_unsupervised较大，因此我们将原权重参数的子参数 mean(logz_unlabel)由0.5进一步减小到0.3，performance得到提高，准确率由82.4%提高到83%。

*  当lr参数提高由0.003到0.004时，performance反而变低；降低到0.002，performance无变化，因此lr不做改动。
   
2. 数据集选择及模型升级。

*  LiuYuhang运用原cora数据集而非经过加工后的cora数据集和GCN模型，以及运用图神经网络工具PyTorch Geometric (PyG)，performance是84.4%，代码已附，见github文件包GCN_PyG。

3. 其他模型改进方法。

*  如老师提示，还有集成学习、AutoML及其他高级图神经网络框架可以运用，留待以后继续学习改进。

感谢老师的批阅，请多多批评指正，再次感谢：）

参考资料：

1. Ming Ding, Jie Tang, and Jie Zhang. 2018. Semi-supervised Learning on Graphs with Generative Adversarial Nets . In The 27th ACM International Conference on Information and Knowledge Management (CIKM ’18), October 22–26, 2018, Torino, Italy. ACM, New York, NY, USA, 10 pages.

2. Hongwei Wang, Jia Wang, Jialin Wang, Miao Zhao, Weinan Zhang, Fuzheng Zhang, Xing Xie and Minyi Guo. 2017. GraphGAN: Graph Representation Learning with Generative Adversarial Nets.

3. LiuYuhang，GNN综述阅读报告，https://github.com/LYuhang/GNN_Review



