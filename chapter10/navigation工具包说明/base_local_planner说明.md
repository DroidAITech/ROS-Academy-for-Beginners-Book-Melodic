### base_local_planner工具包

#### 发布的topic

    ~<name>/global_plan(nav_msgs/Path) #使用全局规划出的路径的一部分，以便局部规划器沿迹前进，同时也可显示在屏幕上
    ~<name>/global_plan(nav_msgs/Path) #得分最高的局部规划或轨迹，用以显示
    ~<name>/cost_cloud(sensor_msgs/PointCloud2) #损失网格，主要是为直观显示，可修改publish_cost_grid_pc参数是否启用此功能

#### 注册的topic

    odom(nav_msgs/Odometry) #接收里程计消息，包含机器人当前的速度信息。

#### 参数

此工具包中包含的参数主要有：机器人设置相关，目标容差，前向模拟，路径得分，震荡预防，全局规划

##### 机器人设置参数

    ~<name>/acc_lim_x(double, default:2.5) #x方向最大加速度
    ~<name>/acc_lim_y(double, default:2.5) #y方向最大加速度
    ~<name>/acc_lim_theta(double, default:3.2) #旋转角加速度最大值
    ~<name>/max_vel_x(double, default:0.5) #最大前进速度
    ~<name>/min_vel_x(double, default:0.1) #最小前进速度，设置到适当的值以保证机器人底盘能够克服摩擦力前进
    ~<name>/max_vel_theta(double, default:1.0) #最大旋转角速度
    ~<name>/min_vel_theta(double, default:-1.0) #最小旋转角速度
    ~<name>/min_in_place_vel_theta(double, default:0.4) #原地旋转最小旋转角速度
    ~<name>/backup_vel(double, default:-0.1) #不建议使用，已改换成escape_vel
    ~<name>/escape_vel(double, default:-0.1) #退出速度，单位为m/s，数值为负
    ~<name>/holonomic_robot(bool, default:true) #确定速度命令是从holonomic还是非holonomic的机器人上f发出的
    ~<name>/y_vels(list, default:[-0.3, -0.1, 0.1, 0.3]) #holonomic机器人的偏向移动速度

##### 目标容错相关参数

    ~<name>/yaw_goal_tolerance(double, default:0.05) #到达目的地的偏航角偏差阈值
    ~<name>/xy_goal_tolerance(double, default:0.10) #到达目的地的x&y平面距离偏差阈值
    ~<name>/latch_xy_goal_tolerance(bool, default:false) #如果目标点锁定，（）

##### 前进模拟参数

    ~<name>/sim_time(double, default:1.0s) #前进模拟的时间
    ~<name>/sim_granularity(double, default:0.025m) #步长大小，轨迹上采样点间的距离长度
    ~<name>/angular_sim_granularity(double, default:~<name>/sim_granularity) #步长大小，旋转分量
    ~<name>/vx_samples(integer, default:3) #每次使用x方向速度的样点数目
    ~<name>/vtheta_samples(integer, default:20) #每次使用的旋转方向的样点数目
    ~<name>/controller_frequency(double, default:20.0) #控制器被调用的频率

##### 轨迹评分模型参数

cost = pdist_scale*(轨迹终点到路径的距离，以地图网格数或者m做单位，与meter_scoring参数有关)
     + gdist_scale*(轨迹终点到局部目标点的距离，以地图网格数目或者m做单位，与meter_scoring有关)
     + occdist_scale*(轨迹上的最大障碍物损失)

    ~<name>/meter_scoring(bool, default:false) #决定是否使用gdist_scale和pdist_scale参数分别确定goal_distance和
    path_distance以地图网格数或m来表示
    ~<name>/pdist_scale(double, default:0.6) #决定控制器与规划路径的接近程度，最大为5.0
    ~<name>/gdist_scale(double, default:0.8) #决定控制器与局部目标点的接近程度，同时控制速度，最大为5.0
    ~<name>/occdist_scale(double, default:0.01) #决定控制器试图避障的程度
    ~<name>/heading_lookahead(double, default:0.325) #当原地转动得分不同时，向前看到的距离
    ~<name>/heading_scoring(bool, default:faulse) #是根据机器人与路径的方向还是根据其与路径的距离打分
    ~<name>/heading_scoring_timestep(double, default) #以时间s衡量的沿轨迹的转动幅度(启用heading_scoring)
    ~<name>/dwa(bool, default:true) #是否使用dwa算法，另外可用Trajectory Rollout,实验证明dwa与TR性能相似，但计算量更小
    ~<name>/publish_cost_grid_pc(bool, default:false) #规划路径时是否发布损失函数网格图
    ~<name>/global_frame_id(string, default:odom) #cost_cloud的参考系，应当与局部损失图全局坐标一致

##### 防振荡模型参数

    ~<name>/oscillation_reset_dist(double, default:0.05) #振荡标志距离，即来回运动的距离在0.05时可认为时振动

##### 全局规划参数

    ~<name>/prune_plan(bool, default:true) #确定是否抹去机器人走过的轨迹，设置为true时，机器人会抹去身后1m以外的轨迹
