##全局路径规划器

###标准方式

所有参数使用默认值

###网格路径方式

    use_grid_path=True

此中方式下，路径是一系列网格中心的连线

###简单势函数计算

    use_quadratic=False

**发布的topic**

    ~<name>/plan(nav_msgs/Path) #最新计算得到的规划路径，每次解算出新的路径，路径规划器都会将其发布

**相关参数**

    ~<name>/allow_unknown(bool, default:true) #是否允许规划器规划穿过未知区域的路径
    ~<name>/default_tolerance(double, default:0.0) #路径规划器规划出的终点容错距离
    ~<name>/visualize_potential(bool, default:false) #是否显示从PointCloud2计算得到的势区域
    ~<name>/use_dijkstra(bool, default:true) #如果设置为true，则使用dijkstra算法，否则使用A*算法
    ~<name>/use_quadratic(bool, default:true) #如果设置为true，使用二次函数近似势函数。否则使用更加简单的计算方式
    ~<name>/use_grid_path(bool, default:false) #如果设置为true，则创建一条沿着网格边界的路径，否则使用梯度下降法。
    ~<name>/old_navfn_behavior(bool, default:false) #如果出于某些原因，你想让global_planner完全复制navfn的行为，则设置为true
    
