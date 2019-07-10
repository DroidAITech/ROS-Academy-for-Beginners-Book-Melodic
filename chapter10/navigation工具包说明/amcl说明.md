# Navigation 工具包说明

source: https://github.com/ros-planning/navigation

## amcl工具包

输入参数： 地图(包括了初始位置)，扫描数据(scan)，tf

输出参数： 位置估计

注册topic：

    scan(sensor_msgs/LaserScan) #激光数据
    tf(tf/tfMessage) #坐标转换
    initialpose(geometry_msgs/PoseWithCovarianceStamped) #初始位置和均值和方差
    map(nav/msgs/OccupancyGrid) #当use_map_topic设置为True时，amcl使用map这个topic接收地图

发布topic：

    amcl_pose(geometry_msgs/PoseWithCovarianceStamped) #机器人在地图中的位姿估计，包括估计方差
    particlecloud(geometry_msgs/PoseArray) #滤波器估计的位置
    tf(tf/tfMessage) #发布从odom到map的转换关系

Services：

    global_localization(std_srvs/Empty) #初始化全局定位，所有粒子完全随机分布在地图上
    request_nomotion_update(std_srvs/Empty) #手动更新粒子并发布更新后的粒子

调用的Services：

    static_map(nav_msgs/GetMap) #amcl调用此服务接收地图，用以基于激光扫描的定位

 相关参数：

amcl中可设置的相关参数，分为滤波器filter相关，扫描模型laser相关，里程计odometry模型相关

    ~min_particles(int, default:100) #最小粒子数
    ~max_particles(int, default:5000) #最大粒子数
    ~kld_err(double, default:0.01) #估计出的分布模型与真实分布模型的最大允许误差
    ~kld_z(double, default:0.99) #
    ~update_min_d(double, default:0.2 m) #两次滤波器更新之间平移的距离
    ~update_min_a(double, default:pi/6.0 rad) #两次滤波器更新之间旋转的弧度
    ~resample_interval(int, default:2) #两次采样之间滤波器更新的次数
    ~transform_tolerance(double, default:0.1 s) #
    ~recovery_alpha_slow(double, default:0.0) #slow average weight滤波器指数衰减速率，用于决定何时通过添加随机位姿进行恢复，可设置为0.001
    ~recovery_alpha_fast(double, default:0.0) #slow average weight滤波器衰减速率，可设置为0.1
    ~initial_pose_x(double, default:0.0 m) #初始位姿mean(x)，用以初始化高斯滤波器
    ~initial_pose_y(double, default:0.0 m) #初始位姿mean(y)，初始化高斯滤波器
    ~initial_pose_a(double, default:0.0 rad) #初始位姿mean(yaw)，初始化高斯滤波器
    ~initial_cov_xx(double, default:0.5*0.5) #初始位姿协方差covariance(x*x)，初始化高斯滤波器
    ~initial_cov_yy(double, default:0.5*0.5) #初始位姿协方差covariance(y*y)，初始化高斯滤波器
    ~initial_cov_aa(double, default:(pi/12)*(pi/12)) #初始位姿协方差covariance(yaw*yaw)，初始化高斯滤波器
    ~gui_publish_rate(double, default:-1.0 Hz) #扫描数据和路径可视化的最大速率，-1.0表示禁止
    ~save_pose_rate(double, default:0.5 Hz) #保存位姿(位姿+方差)的速率，保存的位姿用于不断初始化滤波器
    ~use_map_topic(bool, default:false) #设置为True时，amcl注册到map这个topic上，接收地图
    ~first_map_only(bool, default:false) #设置为True时，amcl使用固定的初始地图，即地图不会更新

**激光模型相关参数：**

    ~laser_min_range(double, default:-1.0) #最小扫描距离
    ~laser_max_range(double, default:-1.0) #最大扫描距离
    ~laser_max_beams(int, default:30) #均分扫描激光束数目
    ~laser_z_hit(double, default:0.95) #z_hit部分的混合权重
    ~laser_z_short(double, default:0.1) #z_short部分的混合权重
    ~laser_z_max(double, default:0.05) #z_max部分的混合权重
    ~laser_z_rand(double, default:0.05) #z_rand部分的混合权重
    ~laser_sigma_hit(double, default:0.2m) #z_hit部分的标准差
    ~laser_lambda_short(double, default:0.1) #z_short部分的指数衰减系数
    ~laser_likelihood_max_dist(double, default:2.0m) #对障碍物进行膨胀操作的最大距离，当模型为likelihood_field时使用
    ~laser_model_type(string, default:"likelihood_field") #选择模型，"beam","likelihood_field","likelihood_field_prob"

**Odometry模型相关**

当odom_model_type为“diff”时，采用sample_model_odometry算法，见Probabilistic Robotics一书第136页,

此模型使用odom_alpha1~odom_alpha4参数，各参数定义见书中。当odom_model_type设置为"omni"时，使用odom_alpha1~odom_alpha5

五个参数。

    ~odom_model_type(string, default:"diff") #设置odom模型，可选"diff","omni","diff-corrected","omni-corrected"
    ~odom_alpha1(double, default:0.2) #从运动旋转分量估计出的旋转量的估计噪声
    ~odom_alpha2(double, default:0.2) #从运动平移分量估计出的旋转量的估计噪声
    ~odom_alpha3(double, default:0.2) #从运动平移分量估计出的平移量的估计噪声
    ~odom_alpha4(double, default:0.2) #从运动旋转分量估计出的平移量的估计噪声
    ~odom_alpha5(double, default:0.2) #平移相关噪声参数
    ~odom_frame_id(string, default:"odom") #里程计使用的坐标系
    ~base_frame_id(string, default:"base_link") #机器人底座坐标系
    ~global_frame_id(string, default:"map") #定位系统所发布的坐标系
    ~tf_broadcast(bool, default:true) #设置为false时，禁止amcl发布从global frame到odometry坐标系的转换
