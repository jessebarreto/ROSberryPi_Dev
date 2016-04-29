# ROSberryPi_Dev

Installing the ROS on Raspberry Pi 3 and Raspberry Pi 2 B   

#Installing ROS on Raspbian Jessie with kernel 4.1.9   

Following this tutorial: http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Indigo%20on%20Raspberry%20Pi   

We are following the instructions to install a ROS_comm.
   
Using the follow commands to setup the repositories   
   
$ sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu jessie main" > /etc/apt/sources.list.d/ros-latest.list'   
$ wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -   

$ sudo apt-get update   
$ sudo apt-get upgrade   

Install bootstrap dependencies   

$ sudo apt-get install python-pip python-setuptools python-yaml python-distribute python-docutils python-dateutil python-six   
$ sudo pip install rosdep rosinstall_generator wstool rosinstall   

Initialize rosdep   

$ sudo rosdep init   
$ rosdep update   

Install and create a catkin workspace   

$ mkdir ~/catkin_ws   
$ cd ~/catkin_ws   

$ rosinstall_generator ros_comm --rosdistro indigo --deps --wet-only --exclude roslisp --tar > indigo-ros_comm-wet.rosinstall   
$ wstool init src indigo-ros_comm-wet.rosinstall    

Installing some unsolve dependencies

$ mkdir ~/catkin_ws/external_src      
$ cd ~/catkin_ws/external_src   
$ sudo apt-get install checkinstall cmake   
$ sudo sh -c 'echo "deb-src http://mirrordirector.raspbian.org/raspbian/ testing main contrib non-free rpi" >> /etc/apt/sources.list'   
$ sudo apt-get update   

Collada-Dom   
(DO NOT FORGET TO CHANGE THE NAME OF THE PACKAGE TO collada-dom-dev)
$ cd ~/catkin_ws/external_src      
$ sudo apt-get install libboost-filesystem-dev libxml2-dev   
$ wget http://downloads.sourceforge.net/project/collada-dom/Collada%20DOM/Collada%20DOM%202.4/collada-dom-2.4.0.tgz   
$ tar -xzf collada-dom-2.4.0.tgz   
$ cd collada-dom-2.4.0   
$ cmake .   
$ sudo checkinstall make install   


libconsole-bridge-dev
(MAKE SURE your /etc/apt/soource.list line isnt commented)
$ cd ~/catkin_ws/external_src
$ sudo apt-get build-dep console-bridge
$ apt-get source -b console-bridge
$ sudo dpkg -i libconsole-bridge0.2*.deb libconsole-bridge-dev_*.deb


$ cd ~/ros_catkin_ws/external_src
$ apt-get source -b lz4
$ sudo dpkg -i liblz4-*.deb
