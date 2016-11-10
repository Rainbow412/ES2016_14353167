# ROS配置

---
My Ubuntu is **Vivid (15.04)**.
## Configure your Ubuntu repositories
Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse."  
![Configure repositories][1]  

## Setup your sources.list
Setup your computer to accept software from packages.ros.org. ROS Jade ONLY supports Trusty (14.04), Utopic (14.10) and Vivid (15.04) for debian packages.  
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

## Set up your keys
```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```

## Installation
First, make sure your Debian package index is up-to-date:  
```
sudo apt-get update
```
There are many different libraries and tools in ROS. 

 - **Desktop-Full Install**: (Recommended) : ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception
 - **Desktop Install**: ROS, rqt, rviz, and robot-generic libraries
 - **ROS-Base**: (Bare Bones) ROS package, build, and communication libraries. No GUI tools.
 - **Individual Package**: You can also install a specific ROS package (replace underscores with dashes of the package name)
  
I installed **Desktop-Full Install**:
```
sudo apt-get install ros-jade-desktop-full
```
  
To find available packages, use:
```
apt-cache search ros-jade
```

## Initialize rosdep
Before you can use ROS, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS.
```
sudo rosdep init
rosdep update
```

## Environment setup
It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:
```
echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

## Getting rosinstall
rosinstall is a frequently used command-line tool in ROS that is distributed separately. It enables you to easily download many source trees for ROS packages with one command.

To install this tool on Ubuntu, run:
```
sudo apt-get install python-rosinstall
```




  [1]: http://ww3.sinaimg.cn/large/69347328jw1f9lyrcmc2cj20me0k2q8e.jpg