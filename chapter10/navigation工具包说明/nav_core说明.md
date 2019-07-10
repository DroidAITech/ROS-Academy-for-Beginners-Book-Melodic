nav_core包提供了机器人导航的通用接口，现在主要提供了BaseGlobalPlanner,BaseLocalPlanner

和RecoveryBehavior接口。所有希望用做move_base插件的规划器和修复器都必须继承自这些接口。

**BaseGlobalPlanner**

nav_core::BaseGlobalPlanner提供了全局规划器的接口，所有的全局规划器要想作为插件用在move_base

节点中，都必须继承自这个接口。目前使用此接口的全局规划器有：

    global_planner
    navfn
    carrot_planner

**BaseLocalPlanner**

nav_core::BaseLocalPlanner提供了局部规划器的接口，所有的局部规划器若想作为插件应用在move_base

中，都必须继承此接口。目前使用此接口的局部规划器有：

    base_local_planner
    eband_local_planner
    teb_local_planner

**RecoveryBehavior**

nav_core::RecoveryBehavior提供修复机制的接口，所有的修复机制若要作为插件应用在move_base

中，都必须继承此接口，目前使用此接口的修复机制有：

    clear_costmap_recovery
    rotate_recovery
