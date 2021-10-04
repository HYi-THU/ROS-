# 1. 3 ROS集成环境配置

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

