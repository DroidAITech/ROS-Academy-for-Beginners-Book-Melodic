robot_pose_ekf包是用来估计机器人的3D位姿。它使用EKF和机器人的6维度模型，结合轮式里程计，IMU，

视觉里程计等。其基本思想是提供不同传感器之间的松耦合。

**如何使用robot_pose_ekf**

配置：

在robot_pose_ekf下有一个launch文件，此文件中包含了一些可配置的参数：

    freq :滤波器的更新发布频率
    sensor_timeout :当传感器停止向滤波器发送消息，滤波器会等多长时间
    odom_used, imu_used, vo_used :是否启用这些输入

**运行**

Build：

    $ rosdep install robot_pose_ekf
    $ roscd robot_pose_ekf
    $ rosmake

Run:

    $ roslaunch robot_pose_ekf.launch

**节点**

    robot_pose_ekf

**注册的topic**

    odom(nav_msgs/Odometry) #2D位姿，实际上是3D位姿，只是忽略了z位置和roll，pitch
    imu_data(sensor_msgs/Imu) #3D姿态
    vo(nav_msgs/Odometry) #3D位姿

注意，robot_pose_ekf并不需要3种传感器同时具备，每种传感器信息都能够估计出机器人的位姿和对应

的协方差。同时，你也可以加入自己的传感器，例如GPS等

**发布的topic**

    robot_pose_ekf/odom_combined(geometry_msgs/PoseWithCovarianceStamped) #滤波器输出

**提供的tf变换**

    obom_combined -> base_footprint
