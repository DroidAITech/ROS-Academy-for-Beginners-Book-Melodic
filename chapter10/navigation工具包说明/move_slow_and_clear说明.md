move_slow_and_clear::MoveSlowAndClear是一种简单的修复机制，它将costmap中的信息清除，并

限制机器人的速度。注意，这种修复机制并不是绝对安全，机器人还是有可能撞上物体，当使用者自定义速度时

就会发生这种情况。另外，这种修复机制只兼容允许通过dynamic_reconfigure设置最大速度的局部规划器

，例如dwa_local_planner

**相关参数**

    ~<name>/clearing_distance(double, default:0.5) #半径范围内的障碍物将被清除
    ~<name>/limited_trans_speed(double, default:0.25) #执行修复机制时的平移速度
    ~<name>/limited_rot_speed(double, default:0.25) #执行修复机制时的旋转速度
    ~<name>/limited_distance(double, default:0.3) #机器人运动的距离
    ~<name>/planner_namespace(string, default:"DWAPlannerROS") #用于重新配置参数的
    规划器命名空间，其中的max_trans_vel和max_rot_vel将会被重新配置。
