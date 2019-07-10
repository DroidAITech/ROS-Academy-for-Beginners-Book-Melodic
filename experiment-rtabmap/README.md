# SLAM实验之RTAB-Map算法

RTAB-Map (Real-Time Appearance-Based Mapping) 是一种RGB-D SLAM算法。该算法用到了基于SIFT/SURF特征点法的视觉里程计，基于词袋的回环检测，利用了点云和三角网格地图，是一个大而全的解决方案。在实现上，RTAB-Map通过限制回环检测和图优化中的定位点的数量来管理存储空间，所以该方案在较大场景下的实时性还不错。RTAB-Map提供了ROS的二进制包，读者可以直接下载使用。另外该算法很适合手持式的RGB-D摄像头或者双目摄像头，在本实验中，我们会利用模拟器里的RGB-D摄像头来实现RTAB-Map建图的方案。

关于RTAB-Map算法的具体介绍，请看http://wiki.ros.org/rtabmap 和http://introlab.github.io/rtabmap/ ，本书只讨论它的配置和应用。