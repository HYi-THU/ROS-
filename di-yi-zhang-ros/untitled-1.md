# 1. 4 ROS架构

**1. 4. 1 ROS文件系统**

![img](file://C:\Users\lhy39\OneDrive\SLAM\typora_pic\%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F.jpg?lastModify=1633347513)

* build：编译空间，用于存放CMake和catkin的缓存信息、配置信息和其他中间文件。
* devle：开发空间，用于存放编译后生成的目标文件，包括头文件、动态&静态链接库、可执行文件等。
* src：源码
  * package：功能包
    * CMakeLists.txt 配置编译规则，比如源文件、依赖项、目标文件
    * package.xml 包信息，比如:包名、版本、作者、依赖项...\(以前版本是 manifest.xml\)
    * scripts 存储python文件
    * src 存储C++源文件
    * include 头文件
    * msg 消息通信格式文件
    * srv 服务通信格式文件
    * action 动作格式文件
    * launch 可一次性运行多个节点
    * config 配置信息
  * CMakeLists.txt: 编译的基本配置

**1. 4. 2 ROS常用指令**

```text
1.增
catkin_create_pkg 自定义包名 依赖包 === 创建新的ROS功能包
sudo apt install xxx === 安装 ROS功能包
​
2.删
sudo apt purge xxx ==== 删除某个功能包
​
3.查
rospack list === 列出所有功能包
rospack find 包名 === 查找某个功能包是否存在，如果存在返回安装路径
roscd 包名 === 进入某个功能包
rosls 包名 === 列出某个包下的文件
apt search xxx === 搜索某个功能包
​
4.改
rosed 包名 文件名 === 修改功能包文件
（需要安装 vim）
比如:rosed turtlesim Color.msg
​
5.执行
roscore     是 ROS 的系统先决条件节点和程序的集合， 必须首先运行才能使 ROS 节点进行通信
rosrun      包名 可执行文件名 === 运行指定的ROS节点
roslaunch   包名 launch文件名 === 执行某个包下的 launch 文件
```

**1. 4. 3 ROS计算图**

如果前期把所有的功能包（package）都已经安装完成，则直接在终端窗口中输入

rosrun rqt\_graph rqt\_graph

如果未安装则在终端（terminal）中输入

```text
$ sudo apt install ros-melodic-rqt
$ sudo apt install ros-melodic-rqt-common-plugins
```

