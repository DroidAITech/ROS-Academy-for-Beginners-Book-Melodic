**简介**

move_base包采用action机制，使移动底座到达给定的目标点。move_base这个节点连接全局规划器和局部

规划器，以便能够完成全局导航任务。它能够支持任何继承自nav_core::BaseGlobalPlanner接口的

全局规划器和任何继承自nav_core::BaseLocalPlanner接口的局部规划器。move_base维护两个代价地图

一个属于全局规划器，另一个属于局部规划器

**Action注册的topic**

    move_base/goal(move_base_msgs/MoveBaseActionGoal) #move_base所搜寻的目标
    move_base/cancel(actionlib_msgs/GoalID) #请求取消目标点

**Action发布的topic**

    move_base/feedback(move_base_msgs/MoveBaseActionFeedback) #反馈包括底座当前相对于世界坐标系的位置
    move_base/status(actionlib_msgs/GoalStatusArray) #提供发送给move_base action的目标的状态信息
    move_base/result(move_base_msgs/MoveBaseActionResult) #对move_base action来说，result为空

**节点注册的topic**

    move_base_simple/goal(geometry_msgs/PoseStamped) #为不需要跟踪目标执行状态的情况提供非action接口

**节点发布的topic**

    cmd_vel(geometry_msgs/Twist) #速度指令流，用以移动底座

**Services**

    ~make_plan(nav_msgs/GetPlan) #允许用户在指定位姿后获取规划出的路径，而不必真正沿着路径运动
    ~clear_unknown_space(std_srvs/Empty) #允许用户直接清除机器人周围的未知区域，适合costmap停止了很长时间，在一个新的地方重新启动的时候使用
    ~clear_costmaps(std_srvs/Empty) #允许用户告诉move_base清除costmaps中的障碍物，可能导致撞上物体

**相关参数**

    ~base_global_planner(string, default:"navfn/NavfnROS") #指定用于move_base的全局规划器插件名称
    ~base_local_planner(string, default:"base_local_planner/TrajectoryPlannerROS") #指定用于move_base的局部规划器名称
    ~recovery_behaviors(list, default:[{name:conservative_reset, type:clear_costmap_recovery/ClearCostmapRecovery},{name:rotate_recovery,type:rotate_recovery/RotateRecovery},{name:aggressive_reset,type:clear_costmap_recovery/ClearCostmapRecovery}]) #用于move_base修复的插件列表，当move_base找不到可行的路径规划方案时，move_base将按照这些顺序执行操作。每个操作执行完后，move_base会在此尝试生成路径规划方案，如果成功，则继续正常操作；否则，启动下一个修复操作。
    ~controller_frequency(double, default:20.0) #控制循环的执行频率，也可认为是速度发布指令的频率
    ~planner_patience(double, default:5.0) #规划器在空间清除操作前,能够进行路径规划的时间
    ~conservative_reset_dist(double, default:3.0) #当在地图中清理出空间时，距离机器人几米远的障碍会从代价地图中清除
    ~recovery_behavior_enabled(bool, default:true) #是否启用move_base修复机制
    ~clearing_rotation_allowed(bool, default:true) #在清除空间操作时，是否允许底座原地旋转
    ~shutdown_costmaps(bool, default:false) #当move_base不活动时，是否关闭costmaps
    ~oscillaiton_timeout(double, default:0.0) #执行修复机制前，允许振荡的时长
    ~oscillation_distance(double, default:0.5) #来回运动在多大距离以上不会被认为是振荡
    ~planner_frequency(double, default:0.0) #全局规划操作的执行频率。如果设置为0.0，则全局规划器仅在接收到新的目标点或者局部规划器报告路径堵塞时才会重新执行规划操作
    ~max_planning_retries(int32, default:-1) #在执行修复操作前，允许规划器重新规划路径的次数，-1.0表示无限多次重新尝试
