# 第九章 SLAM
## 本章简介

机器人研究的问题包含许许多多的领域，我们常见的几个研究的问题包括：建图(Mapping)、定位(Localization)和路径规划（Path Planning），如果机器人带有机械臂，那么运动规划（Motion Planning）也是重要的一个环节。而同步定位与建图（SLAM）问题位于定位和建图的交集部分。

SLAM需要机器人在未知的环境中逐步建立起地图，然后根据地区确定自身位置，从而进一步定位。

![](/pics/slam.png)
这一章我们来看ROS中SLAM的一些功能包，也就是一些常用的SLAM算法，例如Gmapping、Karto、Hector、Cartographer等算法。这一章我们不会去关注算法背后的数学原理，而是更注重工程实现上的方法，告诉你SLAM算法包是如何工作的，怎样快速的搭建起SLAM算法。