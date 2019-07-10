保存了用于和move_base节点进行通讯的消息。这些消息是MoveBase.action自动产生的。

MoveBase.action

    geometry_msgs/PoseStamped target_pose
    ---
    ---
    geometry_msgs/PoseStamped base_position

target_pose是目标点，base_position是底座现在的位置。
