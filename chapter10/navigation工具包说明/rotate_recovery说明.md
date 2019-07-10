本软件包提供了一种通过旋转360度来清除空间的修复机制

**旋转修复相关参数**

    ~<name>/sim_granularity(double, default: 0.017) #两次检测间转动的弧度，
    ~<name>/frequency(double, default: 20.0) #速度发送频率

**TrajectoryPlannerROS相关参数**

局部规划器相关参数

    ~TrajectoryPlannerROS/yaw_goal_tolerance(double, default:0.05) #目标点角度容错
    ~TrajectoryPlannerROS/acc_lim_th(double, default:3.2) #旋转加速度rad/sec^2
    ~TrajectoryPlannerROS/max_rotational_vel(double, default:1.0) #最大旋转速度rad/sec^2
    ~TrajectoryPlannerROS/min_in_place_rotational_vel(double, default:0.4) #最小原地旋转速度
    
