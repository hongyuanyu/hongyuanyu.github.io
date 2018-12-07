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

#### 视频数据形式与视频数据的建模

##### [Compressed Video Action Recognition](https://arxiv.org/pdf/1712.00636.pdf) (CVPR 2018 spotlight)

<!--<img src="https://i.postimg.cc/ncB9NQ99/compress-video.png" width="100%" hegiht="100%" align=center /> -->

![](https://i.postimg.cc/ncB9NQ99/compress-video.png)

* 简介: 
训练强大的深度视频表示比学习深度图像表示更具挑战性。主要是因为视频流中高度的时间和空间冗余，真正有价值的信息往往被巨大的冗余信息所覆盖。使用H.264，HEVC等压缩编码方式可以最多将数据量减少两个数量级，因此本文直接在压缩视频上训练深度网络。
* 启发:
其实之前也发现了这个问题，在使用视频数据训练的时候，先把视频拆帧再读入连续帧图片，拆帧就是一个解码的过程，不会带来任何信息收益，但是数据量一下子由本来的几十M变成上G的数据。而且在训练的时候往往由于数据量过大，视频的batch大小非常有限，训练速度也很慢。之前请教过师兄可不可以用来做跟踪，师兄说这个方法可能不适用于在线视频。在又调研一些文献后发现视频监控的在线视频也是要压缩的，只是没有了B帧，压缩尺度没有离线的那么大。流程如下:

![](https://i.postimg.cc/pVZKdjLN/dvr.png)

具体而言:
* 针对这种压缩数据流设计高效视频检测跟踪网络，由于数据量减小，信息量不变。因此可以把batch做得很大，理论上更加高效。
* 反过来，在做视频超分和视频生成的时候可以生成有压缩的视频，再进一步还原。



#### 视频模式下的注意力机制

#### 视频卷积核等特征提取器

#### 视频网络结构


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