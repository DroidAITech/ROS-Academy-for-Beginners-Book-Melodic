navfn提供了一个快速插入的导航函数，用于对机器人移动底座进行全局路径规划。规划器假设机器人底座

为圆形，用代价地图的形式找到代价最小的路径。使用的算法是Dijkstra，同时也支持A*算法。


**发布的topic**

    ~<name>/plan(nav_msgs/Path) #由navfn计算得到的最新路径，每次planner计算得到新的路径，都会发布

**参数**

    ~<name>/allow_unknown(dool, default:true) #是否允许navfn创建穿过未知区域的路径
    ~<name>/planner_window_x(double, default:0.0) #限制规划器作用的范围
    ~<name>/planner_window_y(double, default:0.0) #限制规划器作用的范围
    ~<name>/default_tolerance(double, default:0.0) #目标点的容错距离，规划器规划到的点距离真正
    的目标点有偏差
    ~<name>/visualize_potential(bool, default:false) #是否显示navfn用PointCloud2计算得到的势函数
