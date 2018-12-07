---
layout:     post
title:      From theory to application, a survey of pedestrian related research.
subtitle:   行人相关理论应用调研
date:       2018-12-06
author:     Hongyuan
header-img: img/post-bg-git.jpg
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

#####[End-to-End Learning of Motion Representation for Video Understanding](https://arxiv.org/abs/1804.00413) (CVPR 2018)
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
2. 评论
扩大感受野，类似self-attention

#####

#### 降维
##### [Learning Low-Dimensional Temporal Representations](http://proceedings.mlr.press/v80/su18a/su18a.pdf) (ICML 2018)

1. 简介:
低维判别表示在性能和复杂性可以增强机器学习方法，这促进了维数降低(DR)的发展。大多数DR方法要求数据为i.i.d.但是，在某些域中，数据自然地以序列形式出现，其中观察在时间上是相关的。文章提出了一种称为LT-LDA的DR方法来学习低维时间表示。文章通过提升基于时间对齐建立的整体时间结构来构建序列类之间的可分离性，可以在不同的子空间中改变。文章通过优化有利于易分离的时间结构的目标来共同学习子空间和相关的对齐，并且表明该目标与对齐的推断​​相关联，因此可以迭代。

2. 评论: 序列降维方法，相关方法可以关注下。

#### 视频网络结构

#### 基础检测（图像和视频）


### Application

#### Human parsing

#### Video based re-id (MARS)
1. dataset MARS(2016)
2. Tracklets matching
3. shortcome: far from practical application
4. 改进方向:
 * 视频模式下可以考虑人脸步态等信息， 但是目前的视频数据集不够支持。
 * 使用注意力机制，关注到part， [Deeply-Learned Part-Aligned Representations for Person Re-Identification](https://arxiv.org/pdf/1707.07256.pdf)
 * Pose guided 

#### Video based re-id (MTMC)
1. 论文： [Features for Multi-Target Multi-Camera Tracking and Re-Identification](https://arxiv.org/pdf/1803.10859.pdf)
2. 方法简介:
3. 不足:
4. 可能的改进:



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