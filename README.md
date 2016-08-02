# 中国机器人大赛中型组仿真比赛
## 说明
该软件包包括了robot_code模块，gazebo_visual模块，coach4sim模块和common模块，其中，参赛选手主要注重robot_code模块，里面包含的是控制机器人运动的相关程序。各个模块的介绍如下：   

 - robot_code: 机器人的感知、规划、运动控制等方面，由NuBot队伍的机器人代码改造而成,其中主要的策略等(在nubot_control软件包中)部分已经删除，选手应自行编写，其余部分可以直接沿用，也可随意改动。
 -  gazebo_visual: Gazebo仿真平台，除基本的设置（在sim_config文件中）外，不应做其他改动。最终使用版本以比赛时的版本为准。   
 - coach4sim: 用于仿真的coach，发送比赛开始、暂停、站位等指令。选手不必做改动。
 - common: 包含cmake、手柄驱动等。选手不必做改动。   

> 请先学习ROS、C++，作为基本技能和知识，然后对机器人策略等方面有一定了解，再进行robot_code的多机器人协同程序的编写。
   
## 共同开发
欢迎大家积极共同完善该仿真平台，我们将感谢每一个人的贡献。   
本软件使用git作为版本控制，托管于GitHub网站。如果想共同开发该软件包，请fork一下该软件包（首先你要注册一个GitHub帐号），然后git clone到自己的电脑去，改进完代码后，可以git push上来，然后再在GitHub里面发一个pull request。   

> **需要的基本技能和知识:**  
> 
>  - 软件编程： 
> 	 - robot_code: ROS, C++    
> 	 - gazebo_visual: ROS, C++, Gazebo插件以及编程实现    
> 	 - coach4sim:	 ROS, C++, Qt    配置文件:	 
> 	 - Linux bash或其他 
>  - 其他方面   
> 	 - 使用说明:	  txt，Mardown 
> 	 - 软件文档：	 Doxygen或其他

## 联系
软件维护者(Maitainer): abcgarden@126.com    
Nubot队伍(RoboCup team): nubot.nudt@outlook.com

# 用户手册 User manual
> **NOTE:** 
> If you want to have a basic understanding of how Gazebo and ROS combines to work for robots, it is recommended to check out the repository ['single_nubot_gazebo'](https://github.com/nubot-nudt/single_nubot_gazebo).
> This contains how to configure the environment, how to run the simulation, and how the robot is simulated. You could run the ROS tool ['rqt_graph'](http://wiki.ros.org/rqt_graph) to
> understand the basic messages and service flow. 

## Recommended Operating Environment
1. Ubuntu 14.04; 
2. ROS Indigo or ROS Jade. (It is recommended to install ROS Jade)
3. Gazebo 5.0 or 5.1 or 7.1;
4. gazebo_ros_pkgs; (please read the **NOTE** below for more information)  
Other versions of Ubuntu, ROS or Gazebo may also work, but we have not tested yet.

**NOTE:** 
Concering how to install appropriate **gazebo_ros_pkgs**, please read the following according to your own situation:   
 - 1.  If you decide to use **ROS Indigo**, please read the following:   
If you choose "desktop-full" install of ROS Indigo, there is a Gazebo 2.0 included initially. In order to install Gazebo 5.0/5.1, you should first remove Gazebo 2.0 by running:   
(**The following command is dangerous; it might delete the whole ROS, so please do it carefully or you may find other ways to delete gazebo2**)   
` $ sudo apt-get remove gazebo2* `    
Then you should be able to install Gazebo 5.0 now. To install gazebo_ros_pkgs compatible with Gazebo
5.0/5.1, run this command:   
` $ sudo apt-get install ros-indigo-gazebo5-ros-pkgs ros-indigo-gazebo5-ros-control`   
HOWEVER,     
It seems ` $ sudo apt-get install ros-indigo-gazebo5-ros-pkgs ros-indigo-gazebo5-ros-control` no longer works now.    
These packages may be moved to other places. You can checkout [gazebo_ros](https://github.com/ros-simulation/gazebo_ros_pkgs.git),
and download and install the correct version.   
 - 2. If you decide to use **ROS Jade** with **gazebo 5.0 or 5.1**, read the following   
ROS Jade has gazebo_ros_pkgs with it; so you don't have to install gazebo_ros_pkgs again.  
However, you should do the following steps to fix some of the bugs in ROS Jade related to Gazebo:        
  -  (a) `$ sudo gedit /opt/ros/jade/lib/gazebo_ros/gazebo`    
In this file, go to line 24 and delete the last '/'. So    
`setup_path=$(pkg-config --variable=prefix gazebo)/share/gazebo/`    
is changed to     
`setup_path=$(pkg-config --variable=prefix gazebo)/share/gazebo`    
You can read this link for more [information](http://answers.ros.org/question/215796/problem-for-install-gazebo_ros_package/)   
  -  (b) Install Gazebo 5.     
   `$ sudo apt-get install gazebo5`     
If this fails, try to run the ['gazebo5_install.sh'](https://github.com/nubot-nudt/simatch/blob/master/gazebo5_install.sh)(obtained from Gazebo's official website).    
Read for more [information](http://answers.ros.org/question/217970/ros-jade-and-gazebo-50-migration-problem/)   
  -  (c) Optional: copy resource files to the new gazebo folder.    
   `$ sudo cp -r /usr/share/gazebo-5.0/* /usr/share/gazebo-5.1`      
 - 3. If you decide to use **ROS Jade** with **gazebo 7.1**, read the following,    
  -  (1) Install gazebo 7.0 by running [gazebo7_install.sh](https://github.com/nubot-nudt/simatch/blob/master/gazebo7_install.sh)(obtained from Gazebo's official website);      
  -  (2) Go to the github website and download the repository of gazebo_ros_pkgs on the branch 'kinetic-devel':      
`$ git clone https://github.com/ros-simulation/gazebo_ros_pkgs.git -b kinetic-devel`     
  -  (3) Go to gazebo_ros_pkgs and create src/, put all other files or folders inside src/.    
` $ cd src/`   
` $ catkin_init_workspace`   
  -  (4) To install dependencies, run    
` $ cd ../../`   
` $ rosdep install --from-paths gazebo_ros_pkgs --ignore-src --rosdistro=jade`      
  -  (5) Then install all these files to /opt/ros/jade   
` $ cd gazeo_ros_pkgs`   
` $ catkin_make -DCMAKE_INSTALL_PREFIX=/opt/ros/jade install`    
You may encounter problems such as 'cannot copu _setup_util.py to /opt/ros/...', if that is the case, you are supposed
to run this command as the root. For example,      
` $ sudo su`   
` $ source /opt/ros/jade/setup.bash`   
` $ rosdep install --from-paths gazebo_ros_pkgs --ignore-src --rosdistro=ROSDISTRO`   
` $ cd gazeo_ros_pkgs`   
` $ catkin_make -DCMAKE_INSTALL_PREFIX=/opt/ros/jade install`   

## Compile
1. ` $ sudo chmod +x configure`
2. ` $ ./configure`
3. ` $ catkin_make --pkg nubot_common`
4. ` $ catkin_make `

--------------------------
## Run
  You could run the robot_code, gazebo_visal and coach4sim in one computer. However, the simulation speed would be low. So you could also run them with several computers to decrease the burden of one single computer.
### With one computer
Assuming you are in the root directory of 'simatch',   
If you want to run those modules in one launch, you could   
1 `$ source devel/setup.bash` 

> You could write this line into ~/.bashrc file so that you don't have to source it every time you open a terminal. 

2 `$ roslaunch simatch_cyan.launch` or `$ roslaunch simatch_magenta.launch`   


If you want to run those modules seperatly, you could   
1 `$ source devel/setup.bash`   

> You could write this line into ~/.bashrc file so that you don't have to source it every time you open a terminal.   

2 To run gazebo_visual,    
`$ roslaunch nubot_gazebo game_ready.launch`    
3 To run robot_code for cyan or magenta robots:      
`$ rosrun nubot_common cyan_robot.sh` or `$ rosrun nubot_common magenta_robot.sh`   
4 To run coach4sim,      
`rosrun coach4sim cyan_coach.sh` or `rosrun coach4sim magenta_coach.sh`   

> **NOTE:** 
> You could change some parameters in sim_config file and relaunch all modules again.

### With several computers

Configuration of computer A and computer B
  The recommended way to run simulation is with two computers running nubot_ws and gazebo_visual seperately.   
For example,computer A runs gazebo_visual to display the movement of robots. Computer B runs nubot_ws to    
calculate and send  movement commands to robots. In addition, computer B should also run coach to send game    
command such as game start.    
  The communication between computer A and computer B is via ROS master. 
  The following is the configuration steps:   
> 1. In computer A, add computer B's IP address in /etc/hosts; and in computer B, add computer A's IP address in /etc/hosts >  
> e.g. In computer A, `$ sudo gedit /etc/hosts and add "Maggie 192.168.8.100"`   
>     In computer B, `$ sudo gedit /etc/hosts and add "Bart   192.168.8.101"`   
> 2. In computer A, run gazebo_visual; In computer B, before you run nubot_ws, you should export ROS_MASTER_URI.   
> e.g. In computer B, ` $ export ROS_MASTER_URI=http://Bart:11311`   
> 3. In computer B, run coach and send game command   

## Build error fix:
1. When catkin_make, if it shows "fatal error: Eigen/Eigen: No such file or directory"   
Solution 1: Change all 'Eigen3' to 'Eigen' in CMakeLists.txt of world_model package in robot_code module  
Solution 2: look at /usr/include/eigen3/Eigen, if this folder exists, it means you have already installed Eigen;    
Input this command: $ sudo ln -s /usr/include/eigen3/Eigen /usr/include/Eigen   
2. if you come across other problems, you could refer to doc/ folder. 



