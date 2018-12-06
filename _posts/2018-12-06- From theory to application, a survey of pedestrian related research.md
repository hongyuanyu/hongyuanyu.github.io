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



### Recent competetion about pedestrian
选取的比赛主要考虑以下几个方面:
* 比赛的延续性，年年都举办的一般是比较好的。
* 数据规模和参赛人数，越大越好
* 选题是否新颖

#### CVPR workshop

##### Visual Understanding of Humans in Crowd Scene and the 2nd Look Into Person (LIP) Challenge

1. 简介:
在野外情景中开发全面的人类视觉理解解决方案，被视为计算视觉中最基本的问题之一，可能在许多工业应用领域产生至关重要的影响，如自动驾驶，虚拟现实，视频监控，人机交互等。和人类行为分析。例如，人体解析和姿态估计通常被认为是更高级别活动/事件识别和检测的第一步。尽管如此，现实生活中应用所需的内容与基于现代计算机视觉技术可实现的内容之间似乎存在巨大差距。

2. [比赛](https://vuhcs.github.io/):
包含五个部分: 单人解析，多人解析， 单人pose估计， 多人pose估计， 细粒度多人解析。相关论文多，参与者也多，规模大。

3. 评论:
图片，视频数据集都有，其他模态的数据集好像没有，比如红外，加入句子理解。

##### ActivityNet Large Scale Activity Recognition Challenge

1. 简介：
侧重于用户日常生活生成的视频中的识别和高级的目标导向的活动，侧重于互联网上的视频。





#### ICCV workshop

#### ECCV workshop