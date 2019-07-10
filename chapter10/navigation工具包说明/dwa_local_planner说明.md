使用DWA算法，实现平面局部导航，给定全局规划和代价地图，局部规划起产生速度指令，发送给移动底座。

本软件支持所有底座可用闭包多边形或者圆来描述的机器人。其配置参数可在launch文件中设置。这些参数

也可动态再配置。

###简介

dwa_local_planner提供一个能够驱动底座的控制器，该控制器连接了路径规划器和机器人。使用地图，

规划器产生从起点到目标点的运动轨迹，在移动时，规划器在机器人周围产生一个函数，用网格地图表示。

控制器的工作就是利用这个函数来确定发送给机器人的速度dx, dy, dtheta

**DWA算法的基本思想**

1. 在机器人控制空间离散采样(dx, dy, dtheta)

2. 对每一个采样的速度进行前向模拟，看看在当前状态下，使用该采样速度移动一小段时间后会发生什么。

3. 评价前向模拟得到的每个轨迹，是否接近障碍物，是否接近目标，是否接近全局路径以及速度等等。舍弃非法路径

4. 选择得分最高的路径，发送对应的速度给底座

**发布的topics**

    ~<name>/global_plan(nav_msgs/Path) #局部规划器所跟踪的全局路径部分。
    ~<name>/local_plan(nav_msgs/Path) #在评价函数中得分最高的局部路径

**注册的topics**

    odom(nav_msgs/Odometry) #里程计信息给局部规划器提供当前速度信息。

**可调参数**

**机器人本身参数**

    ~<name>/acc_lim_x(double, default:2.5) #x方向加速度的绝对值
    ~<name>/acc_lim_y(double, default:2.5) #y方向加速度的绝对值
    ~<name>/acc_lim_th(double, default:3.2) #旋转加速度的绝对值
    ~<name>/max_trans_vel(double, default:0.55) #平移速度最大值绝对值
    ~<name>/min_trans_vel(double, default:0.1) #平移速度最小值的绝对值
    ~<name>/max_vel_x(double, default:0.55) #x方向最大速度的绝对值
    ~<name>/min_vel_x(double, default:0.0) #x方向最小值绝对值，如果是负值表示后退运动
    ~<name>/max_vel_y(double, default:0.1) #y方向最大速度的绝对值
    ~<name>/min_vel_y(double, default:-0.1) #y方向最小速度的绝对值
    ~<name>/max_rot_vel(double, default:1.0) #最大旋转速度的绝对值
    ~<name>/min_rot_vel(double, default:0.4) #最小旋转速度的绝对值

**目标点容错参数**

    ~<name>/yaw_goal_tolerance(double, default:0.05) #到达目标点时偏航角误差
    ~<name>/xy_goal_tolerance(double, default:0.10) #到达目标点时，在xy平面内与目标点的距离误差
    ~<name>/latch_xy_goal_tolerance(double, default:false) #设置为true时，如果到达容错距离内，则机器人就会原地旋转，即便转动时会跑出容错距离外

**前进仿真参数**

    ~<name>/sim_time(double, default:1.7) #向前仿真轨迹的时间
    ~<name>/sim_granularity(double, default:0.025) #步长，轨迹上采样点之间的距离
    ~<name>/vx_samples(integer, default:3) #x方向速度空间的采样点数
    ~<name>/vy_samples(integer, default:10) #y方向速度空间采样点数
    ~<name>/vth_samples(integer, default:20) #旋转方向的速度空间采样点数
    ~<name>/controller_frequency(double, default:20.0) #控制器被调用的频率

**轨迹评分参数**

    ~<name>/path_distance_bias(double, default:32.0) #定义控制器与给定路径接近程度
    ~<name>/goal_distance_bias(double, default:24.0) #定义控制器与局部目标点的接近程度，并控制速度
    ~<name>/occdist_scale(double, default:0.01) #定义控制器躲避障碍物的程度
    ~<name>/stop_time_buffer(double, default:0.2) #为防止碰撞，机器人必须提前停止的时间长度。
    ~<name>.scaling_speed(double, default:0.25) #启动机器人底座的速度
    ~<name>/max_scaling_factor(double, default:0.2) #最大缩放参数

**防振荡参数**

    ~<name>/oscillation_reset_dist(double, default:0.05) #机器人运动多远距离才会重置振荡标记

**全局参数**

    ~<name>/prune_plan(bool, default:true) #机器人前进时是否清除身后1m外的轨迹
