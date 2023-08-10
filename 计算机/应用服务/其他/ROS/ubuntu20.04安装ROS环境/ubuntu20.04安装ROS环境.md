# ubuntu20.04安装ROS环境

## 目录

-   [安装](#安装)
-   [设置](#设置)

<http://wiki.ros.org/noetic/Installation/Ubuntu>

## 安装

添加镜像源

`sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'`

添加密钥

`sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654`

更新apt

`sudo apt update`

完整版安装

`sudo apt install ros-noetic-desktop-full`

安装ROS功能包程序

`sudo apt-get install python3-rosinstall python3-rosinstall-generator python3-wstool build-essential `

## 设置

初始化rosdep(为某些功能包按照系统依赖)

```纯文本
sudo resdop init
rosdep update
```

设置环境变量

`echo "source /opt/ros/noetic/setup.zsh" >> ~/.zshrc`

测试

`roscore`
