# 第一章 ROS

本章ROS学习主要了解ROS历史、ROS安装以及ROS集成环境配置

本章学习ROS的重点：

* ROS安装
* ROS集成环境配置

重点目标：

* 使用C++或者Python实现ROS版本的HelloWorld
* 能够搭建ROS的集成开放环境



#### 1. 1 ROS简介

 为了设计一套机器人通用软件框架，提升功能模块的复用性

脱胎于2000年斯坦福机器人的一系列工程、课程

2007年柳树车库（Willow Garage）发布了ROS，目前已经成为了机器人领域的事实标准

2010年ROS发布元年

**Robot Operating System**

* 适用于机器人的**开源**元操作系统，元操作系统是指基于操作系统的二级系统，
* 集成大量库、协议、工具，类似OS所提供的功能，简化对机器人的控制
* 分布式系统，用在多台计算机上获取，构建，编写和运行代码的工具和库
* ROS设计者将ROS表述为“ROS = Plumbing + Tools + Capabilities + Ecosystem”
  * 通信：核心，简而言之，数据交互
  * 工具：例如仿真，创建机器人模型，脱离实体机器人
  * 功能：复用+自己调参，熟悉机器人之后就是再调参，体力
  * 生态：话语权

**ROS设计目标**

* 代码复用
* 分布式
* 松耦合
* 精简
* 语言独立性
* 易于测试
* 大型应用
* 丰富的组件化工具包
* 免费且开源



#### 1. 2 ROS初步体验

**1. 2. 1 官网安装与测试**

ROS Melodic

[http://wiki.ros.org/melodic/Installation/Ubuntu](http://wiki.ros.org/melodic/Installation/Ubuntu)

国内安装遇到问题

sudo rosdep init ERROR: cannot download default sources list from:

[https://blog.csdn.net/u013468614/article/details/102917569](https://blog.csdn.net/u013468614/article/details/102917569)

rosdep init 及update报错的解决办法汇总（亲测） [https://blog.csdn.net/weixin\_39730025/article/details/113348458\#commentBox](https://blog.csdn.net/weixin_39730025/article/details/113348458#commentBox)

可以改更新地址为gitee上的备份文件

```text
roscore
rosrun turtlesim turtlesim_node
rosrun turtlesim turtle_teleop_key
```

**1. 2. 2 工作空间与功能包**

```text
mkdir -p [name]/src
cd [name]
catKin_make
cd src
catkin_create_pkg [功能包名]helloworld [依赖]roscpp rospy std_msgs
```

ROS中的大多程序由C++和python实现

roscpp是使用C++实现的库，而rospy则是使用python实现的库，std\_msgs是标准消息库，创建ROS功能包时，一般都会依赖这三个库实现。

**注意:** 在ROS中，虽然实现同一功能时，C++和Python可以互换，但是具体选择哪种语言，需要视需求而定，因为两种语言相较而言:C++运行效率高但是编码效率低，而Python则反之，基于二者互补的特点，ROS设计者分别设计了roscpp与rospy库，前者旨在成为ROS的高性能库，而后者则一般用于对性能无要求的场景，旨在提高开发效率。

**1. 2. 3 C++ 实现Helloworld**

**1. 在 \[功能包名\] 下 src 文件夹下编写源码**

```text
// 1. 包含 ROS 的头文件
#include "ros/ros.h"
// 2. 编写 main 函数
int main(int argc, char *argv[])
{
    // 3. 执行 ros 节点初始化
    ros::init(argc,argv,"hello");
    // 4. 输出日志
    //创建 ros 节点句柄(非必须)
    ros::NodeHandle n;
    //控制台输出 hello world
    ROS_INFO("hello world!");
​
    return 0;
}
```

**2. 编辑 \[功能包名\] 文件夹中的Cmakelist.txt**

```text
add_executable(源码文件映射 [可以自定义]
  src/源码文件名.cpp
)
target_link_libraries(源码文件映射 [可以自定义]
  ${catkin_LIBRARIES}
)
```

**3. 进入工作空间目录并编译**

```text
cd catkin_ws
catkin_make
```

生成build devel .....

**4. 编译并执行**

```text
roscore
cd catkin_ws
source ./devel/setup.bash
rosrun [功能包名] [C++节点][源码文件映射]
```

**1. 2. 4 Python实现Helloworld**

**1. 在 \[功能包名\] 下 创建 scripts 文件夹并编写源码**

```text
cd [功能包名]
mkdir scripts
```

新建 python 文件

```text
#! /usr/bin/env python
## 0. 指定解释器
"""
    Python 版 HelloWorld
​
"""
## 1. 导入包
import rospy
​
## 2. 编写 main 函数
if __name__ == "__main__":
    ## 3. 初始化 ROS 节点
    rospy.init_node("Hello")
    ## 4. 输出日志
    rospy.loginfo("Hello World! By python")
```

**2. 为 python 文件添加可执行权限**

```text
chmod +x [python文件名].py
```

**3. 编辑 ros 包下的CmakeList.txt 文件**

```text
catkin_install_python(PROGRAMS scripts/[python文件名].py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

**4. 编译并执行**

```text
roscore
cd catkin_ws
source ./devel/setup.bash
rosrun [功能包名] [python文件名].py
```

#### 1. 3 ROS集成环境配置

主要使用Vscode

**1. 3. 1 依赖工具安装**

终端工具Terminator安装

```text
sudo apt-get install terminator
```

安装Vscode扩展

* C/C++
* Chinese
* CMake Tools
* Python
* ROS

**1. 3. 2 详细使用说明**

**1. 创建工作空间并启动**

```text
mkdir -p xxx_ws/src
cd xxx_ws
catkin_make
code .
```

**2. vscode中编译 ROS**

快捷键 ctrl + shift + B 调用编译，选择:`catkin_make:build`

可以点击配置设置为默认，修改.vscode/tasks.json 文件

```text
{
// 有关 tasks.json 格式的文档，请参见
    // https://go.microsoft.com/fwlink/?LinkId=733558
    "version": "2.0.0",
    "tasks": [
        {
            "label": "catkin_make:debug", //代表提示的描述性信息
            "type": "shell",  //可以选择shell或者process,如果是shell代码是在shell里面运行一个命令，如果是process代表作为一个进程来运行
            "command": "catkin_make",//这个是我们需要运行的命令
            "args": [],//如果需要在命令后面加一些后缀，可以写在这里，比如-DCATKIN_WHITELIST_PACKAGES=“pac1;pac2”
            "group": {"kind":"build","isDefault":true},
            "presentation": {
                "reveal": "always"//可选always或者silence，代表是否输出信息
            },
            "problemMatcher": "$msCompile"
        }
    ]
}
```

**3. 创建 ROS 功能包**

在src右键选择 create catkin package

**设置包名 添加依赖**

**4. C++实现**

在 src 中编写cpp文件，实现步骤如前面，需要配置CMakelists.txt

**PS1: 如果没有代码提示**

修改 .vscode/c\_cpp\_properties.json

设置 "cppStandard": "c++17"

**PS2: main 函数的参数不可以被 const 修饰**

**PS3: 当ROS\_\_INFO 终端输出有中文时，在函数开头加入下面代码的任意一句**

```text
setlocale(LC_CTYPE, "zh_CN.utf8");
setlocale(LC_ALL, "");
```

**5. Python 实现**

在 功能包 下新建scripts文件夹，并编辑，添加执行权限，如前面步骤，需要配置CMakelists.tx

**6. 编译执行**

编译: ctrl + shift + B

执行: 和之前一致，可以在 VScode 中添加终端操作

PS:

如果不编译直接执行 python 文件，会抛出异常。

1.第一行解释器声明：`#! /usr/bin/python3` 不建议

2.依然使用 ：`#!/usr/bin/env python` ， 命令行中创建一个链接符号到 python 命令:`sudo ln -s /usr/bin/python3 /usr/bin/python` 推荐使用

**1. 3. 3 launch实现**

1. 在 功能包 新建launch文件夹
2. 文件夹里面新建 \[name\].launch
3. 编辑内容

```text
<launch>
    <node pkg="功能包1" type="节点名称" name="重命名" output="screen" />
    <node pkg="turtlesim" type="turtlesim_node" name="t1"/>
    <node pkg="turtlesim" type="turtle_teleop_key" name="key1" />
</launch>
```

* node ---&gt; 包含的某个节点
* pkg -----&gt; 功能包
* type ----&gt; 被运行的节点文件
* name --&gt; 为节点命名
* output-&gt; 设置日志的输出目标

1. 运行 launch 文件：`roslaunch 包名 launch文件名`
2. 运行结果: 一次性启动了多个节点

#### 1. 4 ROS架构

**1. 4. 1 ROS文件系统**

![img](file://C:\Users\lhy39\OneDrive\SLAM\typora_pic\%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F.jpg?lastModify=1633356370)

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

