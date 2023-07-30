# ros2笔记

## 环境：Windows11 wsl2(Ubuntu22.04) docker desktop

1、安装 docker desktop(WSL2 后端)
[https://docs.docker.com/desktop/windows/wsl/#download](https://docs.docker.com/desktop/windows/wsl/#download)

2、拉取ros2的镜像（在wsl2中执行）
docker pull osrf/ros:humble-desktop

3、创建docker（在wsl2中执行）
docker run -it osrf/ros:humble-desktop

4、在docker中运行GUI程序

- 打开MobaXterm，并记录DISPLAY
![DISPLAY](./resource/DISPLAY.png)
- 在docker的终端上输入

```bash
export DISPLAY=<MobaXterm 上的值>
```

## 常见错误

1. libEGL warning: failed to open /dev/dri/renderD128: Permission denied

解决方法：将当前用户加入render组中
```bash
sudo usermod -aG render  $USER
``````

2. libEGL warning: MESA-LOADER: failed to open vgem: /usr/lib/dri/vgem_dri.so: cannot open shared object file: No such file or directory (search paths /usr/lib/x86_64-linux-gnu/dri:\$${ORIGIN}/dri:/usr/lib/dri, suffix _dri)

解决方法：
```bash
export LIBGL_ALWAYS_INDIRECT=1
export LIBGL_ALWAYS_SOFTWARE=1
``````

## 基本操作

### topic

```bash
# topics帮助
ros2 topic -h

# 话题列表
ros2 topic list
ros2 topic list -t # 显示相应的消息类型

# 查看话题内容
ros2 topic echo <topic_name>
ros2 topic echo /turtle1/cmd_vel

# 显示话题相关信息,类型
ros2 topic info <topic_name>
ros2 topic info /turtle1/cmd_vel # 输出/turtle1/cmd_vel话题相关信息

# 显示接口相关信息
ros2 interface show <msg_type>
ros2 interface show geometry_msgs/msg/Twist # 输出geometry_msgs/msg/Twist接口相关信息

# 发布topic
ros2 topic pub <topic_name> <msg_type> '<args>' 
# 发布速度命令
ros2 topic pub --once /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
# 按一定频率发布速度命令
ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"

# 查看话题发布的频率
ros2 topic hz <topic_name>
#输出/turtle1/cmd_vel发布频率
ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```

### node

```bash
# nodes帮助
ros2 node -h

# 查看节点列表
ros2 node list

# 查看节点关系图
rqt_graph

# 重映射
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle
ros2 node list

# 查看节点信息
ros2 node info <node_name>
ros2 node info /my_turtle
```
