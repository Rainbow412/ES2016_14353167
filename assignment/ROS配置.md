# ROS配置

---
My Ubuntu is **Vivid (15.04)**.
## Configure your Ubuntu repositories
Configure your Ubuntu repositories to allow "restricted," "universe," and "multiverse."  
![Configure repositories][1]  

## Setup your sources.list
Setup your computer to accept software from packages.ros.org. ROS Jade ONLY supports Trusty (14.04), Utopic (14.10) and Vivid (15.04) for debian packages.  
To use a mirror, simply run one of the commands below when setting up your sources.list file.  
I choose the mirror in Sun Yat-Sen University (China)  
```
sudo sh -c '. /etc/lsb-release && echo "deb http://mirror.sysu.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
```  
After setup sources.list using mirrors:  
![setup sources][2]  

## Set up your keys
```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116
```  
After set up keys:  
![setup keys][3]  

## Installation
First, make sure your Debian package index is up-to-date:  
```
sudo apt-get update
```  
![after update][4]  
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
After initialize:  
![initialize rosdep][5]  

## Environment setup
It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:
```
echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
source ~/.bashrc
```  
After environment setup:  
![environment setup][6]  

## Getting rosinstall
rosinstall is a frequently used command-line tool in ROS that is distributed separately. It enables you to easily download many source trees for ROS packages with one command.

To install this tool on Ubuntu, run:
```
sudo apt-get install python-rosinstall
```  
After get rosinstall:  
![get rosinstall][7]  




  [1]: http://ww3.sinaimg.cn/large/69347328jw1f9lyrcmc2cj20me0k2q8e.jpg
  [2]: https://raw.githubusercontent.com/Rainbow412/pictures/master/LAB5/1.2.png
  [3]: https://raw.githubusercontent.com/Rainbow412/pictures/master/LAB5/1.3.png
  [4]: https://raw.githubusercontent.com/Rainbow412/pictures/master/LAB5/1.4.png
  [5]: https://raw.githubusercontent.com/Rainbow412/pictures/master/LAB5/1.5.png
  [6]: https://raw.githubusercontent.com/Rainbow412/pictures/master/LAB5/1.6.png
  [7]: https://raw.githubusercontent.com/Rainbow412/pictures/master/LAB5/1.7.png
  
