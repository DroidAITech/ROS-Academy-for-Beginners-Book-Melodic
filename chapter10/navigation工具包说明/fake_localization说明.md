**简介**

本包提供一个node，fake_localization，用来替代定位系统，提供可被amcl使用的接口，常在仿真中

使用，计算量更少。它将odometry数据转换成位姿信息、粒子云、tf数据，数据格式与amcl发布的数据相同

**节点fake_localization**

**注册的topic**

    base_pose_ground_truth(nav_msgs/Odometry) #仿真器发布的机器人位置
    initialpose(geometry_msgs/PoseWithCovarianceStamped) #允许使用rviz或nav_view等工具设置位姿。

**发布的topics**

    amcl_pose(geometry_msgs/PoseWithCovarianceStamped) #转发仿真器发布的位姿信息
    particlecloud(geometry_msgs/PoseArray) #用以在rviz和nav_view中可视化的粒子云

**相关参数**

    ~odom_frame_id(string, default:"odom") #里程计坐标系
    ~delta_x(double, default:0.0) #仿真坐标系和节点发布的地图坐标系之间的x方向距离
    ~delta_y(double, default:0.0) #仿真坐标系和节点发布的地图坐标系之间的y方向距离
    ~delta_yaw(double, default:0.0) #仿真坐标系和节点发布的地图坐标系之间的偏航角弧度
    ~global_frame_id(string, default:/map) #全局坐标系
    ~base_frame_id(string, default:base_link) #机器人自身的底座坐标系
    
