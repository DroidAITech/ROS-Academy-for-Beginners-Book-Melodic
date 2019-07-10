
**注册的Topic**

    ~<name>/footprint(geometry_msgs/Polygon) #机器人底盘几何特性，描述形状

**发布的Topic**

    ~<name>/grid(nav_msgs/OccupancyGrid) #损失地图的网格值
    ~<name>/grid_updates(map_msgs/OccupancyGridUpdate) #损失地图更新后的值
    ~<name>/voxel_grid(costmap_2d/VoxelGrid) #选择性发布voxel格点

**Hydro前版本相关参数**

Hydro之后的ROS版本都提供costmap_2d层的插件。如果你没有提供插件参数，程序默认设置为

Hydro前版本，且会加载一系列默认命名空间内的默认插件。默认的命名空间有static_layer,

obstacle_layer和inflation_layer。

**插件**

    ~<name>/plugins(sequence, default:Hydro版本前) #插件序列。每种插件都是一个字典，名字和类型。名字用来定义命名空间。

**坐标系和tf参数**

    ~<name>/global_frame(string, default:"/map") #代价地图的全局坐标系
    ~<name>/robot_base_frame(string, default:"base_link") #机器人底座所对应的坐标系
    ~<name>/transform_tolerance(double, default:0.2) #坐标转换延迟，单位s

**速率参数**

    ~<name>/update_frequency(double, default:0.5) #地图更新频率，Hz
    ~<name>/publish_frequency(double, default:0.0) #地图发布频率

**地图管理参数**

    ~<name>/rolling_window(bool, default:false) #是否使用滚动窗口查看代价地图，如果static_map设置为true，此参数必须设置为false
    ~<name>/always_send_full_costmamp(bool, default:false) #如果为true，则如果代价地图有更新，则将整个代价地图发送到~<name>/grid，如果为false，则只发送改变的部分到~<name>/grid_updates

    ~<name>/width(int, default:10) #地图宽度
    ~<name>/height(int, default:10) #地图高度
    ~<name>/resolution(int, default:0.05) #地图分辨率
    ~<name>/origin_x(double, default:0.0) #全局坐标系的x坐标
    ~<name>/origin_y(double, default:0.0) #全局坐标系的y坐标
    
