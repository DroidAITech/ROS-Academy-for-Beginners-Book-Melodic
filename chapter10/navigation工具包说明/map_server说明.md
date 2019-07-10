map_server提供了map_server节点，提供地图数据。也提供map_saver命令行工具，可以保存动态产生的地图

**Map格式**

保存的地图是图片文件和yaml文件。yaml文件描述地图信息，图片文件将地图进行编码

**图片格式**

图片文件描述地图是否有障碍无等信息，白色像素代表无障碍，黑色像素代表障碍物，两者之间的代表未知区域。

实际上地图可以是彩色或者灰度图，但大多数地图都是灰度图。YAML文件中定义三种区域之间的颜色分类阈值。

某种灰度的填充率可以用下面公式计算occ = (255 - color_avg) / 255.0。

**YAML格式**

    image: testmap.png
    resolution: 0.1
    origin: [0.0, 0.0, 0.0]
    occupied_thresh: 0.65
    free_thresh: 0.196
    negate: 0

**map_server介绍**

是一个ROS节点，用于从硬盘中读取地图文件，

**用法**

    map_server <map.yaml>

**示例**

    rosrun map_server map_server mymap.yaml

需要指出的是地图数据必须从latched topic或者service中获取。

**发布的topic**

    map_metadata(nav_msgs/MapMetaData) #从这个topic中接收地图元数据
    map(nav_msgs/OccupancyGrid) #从这个topic中接收地图

**Services**

    static_map(nav_msgs/GetMap) #从这个service中接收地图

**相关参数**

    ~frame_id(string, default:"map") #发布的地图所使用的坐标系

**map_saver**

map_saver保存地图到硬盘，例如，从SLAM的建图service保存到硬盘

    rosrun map_server map_saver [-f mapname]

map_saver接收地图数据并将其写入到map.png和map.yaml文件中。-f指定保存文件名

例子：

    rosrun map_server map_saver -f mymap

**注册的topic**

    map(nav_msgs/OccupancyGrid) #接收地图
