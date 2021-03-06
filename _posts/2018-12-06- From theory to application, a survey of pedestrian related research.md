---
layout:     post
title:      From theory to application, a survey of pedestrian related research.
subtitle:   行人相关理论应用调研
date:       2018-12-06
author:     Hongyuan
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - 论文
    - 原创
---

> 原创笔记, 论文调研

### Theory about video

#### 视频数据形式

##### [Compressed Video Action Recognition](https://arxiv.org/pdf/1712.00636.pdf) (CVPR 2018)

[![compress-video.png](https://i.postimg.cc/ncB9NQ99/compress-video.png)](https://postimg.cc/GH3pytQb)
1. 简介: 
训练强大的深度视频表示比学习深度图像表示更具挑战性。主要是因为视频流中高度的时间和空间冗余，真正有价值的信息往往被巨大的冗余信息所覆盖。使用H.264，HEVC等压缩编码方式可以最多将数据量减少两个数量级，因此本文直接在压缩视频上训练深度网络。
2. 评论:
其实之前也发现了这个问题，在使用视频数据训练的时候，先把视频拆帧再读入连续帧图片，拆帧就是一个解码的过程，不会带来任何信息收益，但是数据量一下子由本来的几十M变成上G的数据。而且在训练的时候往往由于数据量过大，视频的batch大小非常有限，训练速度也很慢。之前请教过师兄可不可以用来做跟踪，师兄说这个方法可能不适用于在线视频。在又调研一些文献后发现视频监控的在线视频也是要压缩的，只是没有了B帧，压缩尺度没有离线的那么大。流程如下:
[![dvr.png](https://i.postimg.cc/pVZKdjLN/dvr.png)](https://postimg.cc/z3yytVhp)

##### [Multi Frame Quality Enhancement for Compressed Video](https://arxiv.org/pdf/1803.04680.pdf) (cvpr 2018)
1. 简介:
近年来，视频在互联网上的传播愈发频繁，人们对高清视频的需求也越来越大。由于网络传输带宽的限制，视频必须经过压缩编码才能在互联网中传播。然而，压缩编码不可避免地会给视频带来失真，这会严重影响视频观看者的主观感受。因此，针对有损压缩视频，研究一种有效的质量增强方法是十分必要的。

2. 评论: 视频中模糊大量存在，清晰度增强很有必要，但是文章的模型过于复杂，如果可以是一个小模块的形式会更加实用。


##### 启发
* 针对这种压缩数据流设计高效视频检测跟踪网络，由于数据量减小，信息量不变。因此可以把batch做得很大，理论上更加高效。
* 反过来，在做视频超分和视频生成的时候可以生成有压缩的视频，再进一步还原。
* 视频清晰度增强模块来提高视频清晰度进而增强检测跟踪能力，实时场景下视频超分辨应该于检测跟踪融为一体。

#### 视频卷积核等特征提取器

##### [MiCT: Mixed 3D/2D Convolutional Tube for Human Action Recognition](http://openaccess.thecvf.com/content_cvpr_2018/papers/Zhou_MiCT_Mixed_3D2D_CVPR_2018_paper.pdf) (CVPR 2018)

[![mict.png](https://i.postimg.cc/KjWTydnZ/mict.png)](https://postimg.cc/8fhcRXK3)
[![mitc-net.png](https://i.postimg.cc/yYJ9T8HW/mitc-net.png)](https://postimg.cc/ppHpWxyt)
1. 简介:
视频中的人为动作是三维（3D）信号。最近的尝试使用3D卷积神经网络（CNN）来探索用于人类动作识别的时空信息。虽然很有前景，但3D CNN在这项任务方面并没有取得高性能，因为它们在静止图像中用于视觉识别的完善的二维（2D）对应物。文章认为，时空融合的高度训练复杂性和3D卷积的巨大内存成本阻碍了当前3D CNN，通过输出对于高级任务至关重要的更深层特征图，逐层堆叠3D卷积。因此，文章提出了一种混合卷积（MiCT），它将2D CNN与3D卷积模块相结合，以生成更深入，更丰富的信息特征图，同时降低每轮时空融合的训练复杂性。基于MiCT，还提出了一种新的端到端可训练深度3D网络MiCTNet，以更好地探索人类行为中的时空信息。对三个着名的基准数据集（UCF101，Sport1M和HMDB-51）的评估表明，所提出的MiCT-Net显着优于原始的3D CNN。与UCF101和HMDB51上最先进的动作识别方法相比，文章的MiCT-Net产生了最佳性能。
2. 评论:
混合卷积

##### [Trajectory Convolution for Action Recognition](https://papers.nips.cc/paper/7489-trajectory-convolution-for-action-recognition) (NIPS 2018)

[![Trajectory-Convolution.png](https://i.postimg.cc/NfWXCvsD/Trajectory-Convolution.png)](https://postimg.cc/QHJVVPWK)
1. 简介:
如何利用时间维度是视频分析中的一个主要问题。目前主要方法是将3D卷积分解为分别用于空间的单独分量和时间卷积。然而，时间卷积带有一个
隐式假设，即跨时间步长的特征映射能很好地对齐，以便可以聚合相同位置特征。这个假设可以在实际应用中过于强大，特别是在运动识别方面。
运动是一个至关重要的线索。在这项工作中，提出了一个新的CNN架构TrajectoryNet，包含轨迹卷积，一种新的操作沿时间维度整合特征，以替换现有的时间
卷积。此操作明确考虑了内容的更改由变形或运动引起，可以沿着运动路径聚合视觉特征。
2. 评论:
其实是可变形卷积的三维扩展。

##### [End-to-End Learning of Motion Representation for Video Understanding](https://arxiv.org/abs/1804.00413) (CVPR 2018)
1. 简介:
尽管端到端学习的表征近来取得了成功，但视频分析任务广泛使用的仍然还是人工设计的光流特征。为了弥补这一不足，我们提出了一种全新的可端到端训练的神经网络 TVNet，可从数据中学习类光流特征。TVNet 是一种特定的光流求解器 TV-L1 方法，并且通过将 TV-L1 中的优化迭代展开成神经层而进行了初始化。因此，TVNet 无需任何额外训练就能直接使用。
此外，它还可以自然地嫁接到其它特定任务的网络上，从而得到端到端的架构；因此，这种方法无需预计算和在磁盘上预存储特征，比之前的多阶段方法更加高效。最后，TVNet 的参数可以通过端到端训练进一步优化。这让 TVNet 不仅可以学习光流，而且还可以学习到更丰富的且更针对任务的动作模式。我们在两个动作识别基准上进行了广泛的实验，结果表明了我们提出的方法的有效性。我们的 TVNet 实现了比所有同类方法更优的准确度，同时在特征提取时间方面也能与最快的同类方法媲美。

2. 评论:
替代光流的特征提取器，不训练也可以用。

##### 启发
* 这些3维卷积的应用场景都是视频以及给定好了，对于监控等有实时需求的任务难以应用。而convlstm在使用过程中效果有限，剧烈变化场景容易失效，因此需要设计一种针对在线视频任务的新卷积核。

#### 视频模式下的注意力机制

##### [Non-local Neural Networks](https://arxiv.org/pdf/1711.07971.pdf) (CVPR 2018)

1. 简介:
将non local思想引入视频分类。这篇文章受传统的non-local mean operation启发，核心思想就是：our non-local operation computes the response at a position as a weighted sum of the features at all positions. 简单讲单纯的一层卷积操作其实都是很local的，因此为了达到non local的效果（也就是文中说的获取 long range dependency），一般就是不断叠加这样的特征提取层，这样高层网络的感受野（receptive field）就越来越大，获取的信息分布广度也越来越高。但是这种不断叠加的方式必然会带来计算量的增加和优化难度的增加，因此就有了本文作者提出的non local机制。
2. 评论:
扩大感受野，类似self-attention。在视频中加过，检测性能不升反降。

##### [Deep Attentive Tracking via Reciprocative Learning](https://arxiv.org/pdf/1810.03851.pdf) (NIPS 2018)

[![recipracative.png](https://i.postimg.cc/Y0HwXxnb/recipracative.png)](https://postimg.cc/hzpwvVqd)
1. 简介: 这是一篇跟踪的文章。对于视觉跟踪，外观剧烈变化时具有较大挑战性。注意力机制有助于解决这个问题，但是传统注意力方法是学习一组预测判别用的参数。该文章是讲注意力机制运用到网络的训练中。本文基于 tracking-by-detection 框架提出一种新颖的 Attention 机制来复制跟踪，实现在跟踪过程中自动 attend target object regions

2. 评论: 还可以这么用注意力机制，很新颖的方法。

#### 降维
##### [Learning Low-Dimensional Temporal Representations](http://proceedings.mlr.press/v80/su18a/su18a.pdf) (ICML 2018)

1. 简介:
低维判别表示在性能和复杂性可以增强机器学习方法，这促进了维数降低(DR)的发展。大多数DR方法要求数据为i.i.d.但是，在某些域中，数据自然地以序列形式出现，其中观察在时间上是相关的。文章提出了一种称为LT-LDA的DR方法来学习低维时间表示。文章通过提升基于时间对齐建立的整体时间结构来构建序列类之间的可分离性，可以在不同的子空间中改变。文章通过优化有利于易分离的时间结构的目标来共同学习子空间和相关的对齐，并且表明该目标与对齐的推断​​相关联，因此可以迭代。

2. 评论: 序列降维方法，相关方法可以关注下。

#### 其他
##### [A VARIATIONAL U-NET FOR CONDITIONAL APPEARANCE AND SHAPE GENERATION](https://arxiv.org/pdf/1804.04694.pdf)(CVPR 2018)

[![VAE-UNET.png](https://i.postimg.cc/8PbZ9VPh/VAE-UNET.png)](https://postimg.cc/JGG3HvXn)
1. 简介:
深度生成模型在图像合成领域展现了优异的性能。然而，由于它们是直接生成目标的图像，而没有对其本质形状和外观之间的复杂相互影响进行建模，所以在空间转换时就会存在性能退化。我们针对形状指导图像生成提出了条件 U-Net，将变分自编码器输出的外观条件化。这个方法在图像数据集上进行端到端的训练，不需要同一个物体在不同的姿态或者外观下的采样。实验证明，这个模型能够完成条件图像生成和转换。所以，查询图像的外观或者形状能够被保留，同时能够自由地改变未被保留的另一个。此外，在保留形状的时候，由于外观的随机潜在表征，它可以被采样。

2. 评价:
模型比较有意思，可以考虑扩展到其他任务中。论文写得好，后面仔细学习下写作。


### Application

#### 行为识别
##### [Temporal relation reasoning in videos](https://arxiv.org/pdf/1711.08496.pdf)

[![temperal-relation.png](https://i.postimg.cc/R0KKrSCG/temperal-relation.png)](https://postimg.cc/XZNpd0Cy)
1. 简介:
视频中的行为识别大部分方法是围绕Two-stream和C3D展开的。
1）Two-stream，即利用视频帧图像（spatial）以及根据帧图像通过无监督提取的光流场图像（Flow）来各自train一个模型，并在网络产生结果之后，对结果做一个后融合。这两个modality分别表示静态信息和短时序信息。在这个思路上有很多杰出的工作，如Fusion-NN，TSN，salient area based, spatial-temporal bilinear pooling, temporal blinear encoding等能取得当时的SOTA。
2）C3D，比之C2D,多了一个时序上的密集采样 卷积，以此弥补Spatial modality上的短时序信息缺乏问题，虽然从整体结果来看，效果并不如two-stream fusion后的结果。
在这上面的尝试，有 I-3D, ResNet-3D, P-3D, non-local-3D 等等。
上面的方法基本可以很有效的解决从一张图就能识别出行为的类别，比如踢球，你看到运动员在踢球，那这肯定是在踢足球了，再加上光流信息的融合，概率就更高了（我们能知道踢的动作了）。但是这些方法都不能解决长时序问题，或者更具体的说： 如果这个行为是因为前后的时序变化而产生的，那么仅仅看单帧，或者短时序，是不能识别出这个类别的。比如人在叠被子，被子的变化很难通过单帧和短时序被CNN学习，只有看到叠被子前和叠被子后的变化，你才知道他在叠被子。这种情况对于真实场景非常常见， 不是一些小量数据集能反应出来的。

2. 评论:
可以扩展到其他任务中的长时间记忆保持。文章做法可以简单的理解成改变了TSN的信息汇总的选举方式，感觉虽然是对temporal relation的一个研究，但结构简单，只限于每帧过完CNN后的feature以及选择MLP的结构做为关系的联接，这个方向还有很大的空间。


#### 检测
##### [Fully Motion-Aware Network for Video Object Detection](http://openaccess.thecvf.com/content_ECCV_2018/papers/Shiyao_Wang_Fully_Motion-Aware_Network_ECCV_2018_paper.pdf) (ECCV 2018)

[![MANET.png](https://i.postimg.cc/YCzjQbR2/MANET.png)](https://postimg.cc/K14Z27CC)
1. 简介:
视频物体检测中当某些帧突然恶化会很影响检测结果。典型的解决方案之一是通过聚合相邻帧来增强每帧特征。由于物体和相机的运动，物体的特征通常不是空间校准的。本文提出了一种称为全动态感知网络（MANet）的端到端模型，它在统一的框架中实现像素级和实例级对象的特征校准。像素级校准在建模详细运动时非常灵活，而实例级校准可捕获更多全局运动提示，以便对遮挡具有鲁棒性。

2. 评论: 这篇也是视频中特征校准，因为视频变化剧烈，所以特征很容易对不齐，可变性3D卷积核也是做这个的。

##### 启发
视频中的landmark感觉可以做一下，视频校准很有必要。可以建个数据集再做个baseline方法。

#### 异常行为检测

##### [Joint Detection and Recounting of Abnormal Events by Learning Deep Generic Knowledge](https://arxiv.org/pdf/1709.09121.pdf) (ICCV 2017)

[![ab1.png](https://i.postimg.cc/pVj0RFkY/ab1.png)](https://postimg.cc/Dm2Qcm98)
1. 简介:
本文讨论了联合检测和重新计算视频中异常事件的问题。重述异常事件，即解释为什么它们被判断为异常，是视频监控中尚未探索但至关重要的任务，因为它有助于人类观察者快速判断它们是否是误报警。为了以事件叙述的人类可理解的形式描述事件，学习关于视觉概念（例如，对象和动作）的一般知识是至关重要的。尽管卷积神经网络（CNN）已经在学习中获得了有希望的结果，但对于如何有效地使用CNN进行异常事件检测，这仍然是一个悬而未决的问题，主要是由于异常检测的环境依赖性。 在本文中，我们通过集成通用CNN模型和环境相关的异常检测器来解决这个问题。我们的方法首先学习具有多个视觉任务的CNN，以利用对于检测和重述异常事件有用的语义信息。通过将模型适当地插入异常探测器，我们可以检测并重新计数
在利用CNN的判别力的同时发生异常事件。

2. 评价:
异常行为可解释下，也就是学习目标和环境物体之间的关系来发现异常。但是这是基于单帧图片的，视频模式下的关系建立可以探索一下。

##### [A Revisit of Sparse Coding Based Anomaly Detectionin Stacked RNN Framework](http://openaccess.thecvf.com/content_ICCV_2017/papers/Luo_A_Revisit_of_ICCV_2017_paper.pdf) (ICCV 2017)

[![ab2.png](https://i.postimg.cc/QMn6xgRS/ab2.png)](https://postimg.cc/4KVvWc4h)
1. 简介: 
受基于稀疏编码的异常检测能力的启发，我们提出了一种时间相干性
我们强制执行类似相邻帧的稀疏编码（TSC）用相似的重建系数编码。然后我们用特殊类型的堆叠递归神经网络（sRNN）映射TSC。通过利用sRNN同时学习所有参数，可以避免对TSC的非平凡超参数选择，同时利用浅sRNN，可以在正向通道内推断重构系数，这降低了学习稀疏系数的计算成本。本文的贡献有两方面：i）我们提出了一个TSC，它可以映射到sRNN，这有助于参数优化并加速 异常预测。 ii）我们构建了一个非常大的数据集，该数据集甚至比根据数据量和场景多样性的异常检测的所有现有数据集的总和还要大。

2. 评价:
魔改RNN,稀疏编码加提出新的数据集。

##### [Unsupervised Anomaly Detection with Generative Adversarial Networks to Guide Marker Discovery](https://arxiv.org/pdf/1703.05921.pdf) (IPMI 2017)

[![ab3.png](https://i.postimg.cc/Ss2gX244/ab3.png)](https://postimg.cc/5YJBTtjK)
1. 简介:
获得捕获与疾病进展和治疗监测相关的成像标志物的模型是具有挑战性的。模型通常基于大量数据，带有注释的已知标记示例，旨在自动检测。高注释努力和对已知标记词汇的限制限制了这种方法的力量。在这里，我们进行无监督学习，以识别成像数据中的异常作为标记的候选者。我们提出AnoGAN，一个深度卷积生成对抗网络 学习一系列正常的解剖变异性，伴随着一种新的异常评分方案，该方案基于从图像空间到潜在空间的映射。应用于新数据，模型标记异常，并对图像补丁进行评分，表明它们适合学习的分布。视网膜光学相干断层扫描图像的结果表明该方法正确识别异常图像，例如含有视网膜液或高反射灶的图像。

2. 评价:
用GAN做医学疾病诊断，DCGAN的简单改进，效果没有很好。

##### [Future Frame Prediction for Anomaly Detection – A New Baseline](https://arxiv.org/pdf/1712.09867.pdf) (CVPR 2018)

[![ab4.png](https://i.postimg.cc/Bb2Ngrtw/ab4.png)](https://postimg.cc/hQP943L9)
1. 简介:
视频中的异常检测是指识别不符合预期行为的事件。然而，几乎所有现有方法都通过最小化训练数据的重建误差来解决该问题，这不能保证异常事件的较大重建误差。在本文中，我们建议在视频预测框架内解决异常检测问题。据我们所知，这是第一项利用预测的未来框架与其基本事实之间的差异来检测异常事件的工作。为了预测正常事件的质量更高的未来帧，除了对强度和梯度的常用外观（空间）约束之外，我们还通过强制预测帧和地面实况之间的光流来在视频预测中引入运动（时间）约束。帧是一致的，这是第一个将时间约束引入视频预测任务的工作。这种空间和运动约束有助于未来正常事件的帧预测，并因此有助于识别那些不符合期望的异常事件。对玩具数据集和一些公开可用数据集的广泛实验验证了我们的方法在对正常事件中的不确定性的稳健性和对异常事件的敏感性方面的有效性。

2. 评价:
预测下一帧来发现异常，仅仅一帧应该不够的。

##### [Real-world Anomaly Detection in Surveillance Videos]() (CVPR 2018)

[![ab5.png](https://i.postimg.cc/mDv87LZH/ab5.png)](https://postimg.cc/bDRxhjty)
1. 简介:
监控视频能够捕获各种逼真的异常情况。在本文中，我们建议通过利用正常和异常视频来学习异常。为了避免注释反常段或剪辑培训视频，这是非常耗时的，我们提出通过深多个实例异常学会利用弱标记的训练视频排名框架，即培训标签（异常或正常）是在video-级别而不是剪辑级别。在我们的方法中，我们将正常和异常视频视为包和视频片段作为多实例学习（MIL）中的实例，并自动学习深度异常排名模型，该模型预测异常视频片段的高异常分数。此外，我们在排名损失函数中引入稀疏性和时间平滑性约束，以更好地定位训练期间的异常。我们还推出了一个新的大规模的第一个128小时视频数据集。它包括1900个长而未经修剪的现实监控视频，包括13个现实异常现象，如战斗，交通事故，入室盗窃，抢劫等，以及正常活动。此数据集可用于两个任务。首先，考虑一组中的所有异常和另一组中的所有正常活动的一般异常检测。第二，识别13个异常活动中的每一个。我们的实验结果表明，与最先进的方法相比，我们的MIL异常检测方法在异常检测性能方面取得了显着的进步。我们提供了最近几个关于异常活动识别的深度学习基线的结果。这些基线的低识别性能表明我们的数据集非常具有挑战性，并为未来的工作开辟了更多的机会。

2. 评价:
弱监督标注数据集，但是方法居然不是在线处理而是划分视频片段。

##### 总结
比较小的方向，但是可以用到很多高级方法。最近的论文都是伴随新的数据集形式出现的。

#### Pose 相关
参见workshop和[CVPR2018 人体姿态相关](http://www.cvmart.net/community/article/detail/286)

#### Human parsing
参见workshop

#### Video based re-id and MOT

##### dataset MARS(2016)
1. Tracklets matching
2. shortcome: far from practical application
3. 改进方向:
 * 视频模式下可以考虑人脸步态等信息， 但是目前的视频数据集不够支持。
 * 使用注意力机制，关注到part， [Deeply-Learned Part-Aligned Representations for Person Re-Identification](https://arxiv.org/pdf/1707.07256.pdf)
 * Pose guided 

##### [Features for Multi-Target Multi-Camera Tracking and Re-Identification](https://arxiv.org/pdf/1803.10859.pdf) (CVPR 2018)

[![MOT1.png](https://i.postimg.cc/SR0bjbwZ/MOT1.png)](https://postimg.cc/VrFVHpsM)
1. 方法简介:
多目标多相机跟踪（MTMCT）通过从多个相机拍摄的视频跟踪许多人。人员重新识别（Re-ID）从图库中检索与人查询图像类似的人的图像。我们通过卷积神经网络学习MTMCT和Re-ID的良好特征。我们的贡献包括用于训练的自适应加权三元组丢失和用于硬标识挖掘的新技术。

2. 评论: 
这也是一个离线的方法，用聚类做reid。

##### 总结
视频里的reid，跟踪可以可以拓展为,目标发现(spotting),根据一个目标的照片实时跟踪整个目标并且分析其行为。


### Recent competetion about human
选取的比赛主要考虑以下几个方面:
* 比赛的延续性，年年都举办的一般是比较好的。
* 数据规模和参赛人数，越大越好
* 选题是否新颖


#### ICCV 2017 workshop

##### 3D Face Tracking in-the-wild Competition

1. 简介:
[链接](https://ibug.doc.ic.ac.uk/resources/1st-3d-face-tracking-wild-competition/)， 感觉比较新颖，虽然不会去做，不过可以得到一些启发。

2. 评价:
目前数据集比较小，是单一人脸的3D跟踪。可以扩展到多3D人脸跟踪，3D行人跟踪等等。

##### PoseTrack Challenge: Human Pose Estimation and Tracking in the Wild

1. 简介:
目标是推动基于视频的人体姿态估计和精准跟踪的技术发展。

2. 比赛:
本次比赛有三个赛道：单帧多人姿态估计，视频中的多人姿态估计以及多人精准跟踪。
比赛链接: [https://posetrack.net/workshops/eccv2018/](https://posetrack.net/workshops/eccv2018/)


3. 评价:
比较火的一个比赛，ICCV, ECCV都会举办。貌似目前做的人不多。

##### SIXD Challenge

1. 简介:
刚性物体的姿势具有六个自由度，并且在许多机器人和增强现实应用中需要其全部知识。挑战的目标是评估来自RGB或RGB-D图像的6D物体姿态估计的方法。

2. 比赛:
比赛链接: [http://cmp.felk.cvut.cz/sixd/challenge_2017/](http://cmp.felk.cvut.cz/sixd/challenge_2017/)

3. 评价:
6D数据和点云都是和机器人视觉密切相关的方向。


#### CVPR 2018 workshop

##### Visual Understanding of Humans in Crowd Scene and the 2nd Look Into Person (LIP) Challenge

1. 简介:
在野外情景中开发全面的人类视觉理解解决方案，被视为计算视觉中最基本的问题之一，可能在许多工业应用领域产生至关重要的影响，如自动驾驶，虚拟现实，视频监控，人机交互等。和人类行为分析。例如，人体解析和姿态估计通常被认为是更高级别活动/事件识别和检测的第一步。尽管如此，现实生活中应用所需的内容与基于现代计算机视觉技术可实现的内容之间似乎存在巨大差距。

2. 比赛:
包含五个部分: 单人解析，多人解析， 单人pose估计， 多人pose估计， 细粒度多人解析。相关论文多，参与者也多，规模大。
比赛链接: [https://vuhcs.github.io/](https://vuhcs.github.io/)

3. 评论:
图片，视频数据集都有，其他模态的数据集好像没有，比如红外，加入句子理解。

##### ActivityNet Large Scale Activity Recognition Challenge

1. 简介:
理解视频中人的动作和行为，是计算机视觉领域的挑战性问题，也是视频内容理解的关键，极具应用前景。ActivityNet挑战赛旨在催生视频行为理解的新算法和新技术，是目前视频行为理解领域数据规模最大、最具影响力的技术竞赛，已成功举办三届，吸引了全球计算机视觉领域诸多强队积极参赛。

2. 比赛:
任务1: 未修剪视频动作分类；
任务2: 修剪视频动作识别（Kinetics数据）；
任务3: 时序动作片段候选提名；
任务4: 时序动作定位；
任务5: 视频事件检测和描述。
比赛链接: [http://activity-net.org/challenges/2018/index.html](http://activity-net.org/challenges/2018/index.html)

3. 评论:
数据集丰富，参与者众多，竞争大。

#### ECCV 2018 workshop

##### Person in Contex (PIC)

1. 简介:
Person in Context (PIC) 图像关系分割竞赛和workshop

2. 比赛:
主要是分割出图片中各个任务的关系。
![PIC.png](https://i.postimg.cc/yYtksgxK/PIC.png)
比赛链接: [http://picdataset.com/challenge/task-intro/](http://picdataset.com/challenge/task-intro/)

3. 评价:
参与者很少，数据集易于扩展到视频模式。

##### YouTube-8M

1. 简介:
为了推动视频分析和视频理解相关技术的进步，Google AI 已经公布了一个大型视频数据集 —— YouTube-8M，本次比赛是基于 YouTube 8M 数据集的第二次Kaggle比赛。

2. 比赛: 
主要是视频分类

3. 评价:
异常火爆，几乎没法做了。

##### Open Images Challenge
1. 简介: 
Google开放 Open Images V4 数据库，并基于这一数据集举办 ECCV 2018 公开图像挑战赛，比赛分为两个赛道：对象类别检测，视觉关系检测

2. 比赛:
![openimg.png](https://i.postimg.cc/fTw6yB3j/openimg.png)
比赛链接: [https://storage.googleapis.com/openimages/web/challenge.html](https://storage.googleapis.com/openimages/web/challenge.html)

3. 评价:
和PIC人与人关系相比，这个多了很多物体的，人和物体的关系。同理可以扩展到视频模式。

#### 补充

还有大热的VOT跟踪以及COCO分割比赛没有放上去。