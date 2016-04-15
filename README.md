# ROSberryPi_Dev

Installing the ROS on Raspberry Pi 3 and Raspberry Pi 2 B

#Installing ROS on Raspbian Jessie with kernel 4.1.9

Following this tutorial: http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Indigo%20on%20Raspberry%20Pi

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

$ mkdir ~/ros_catkin_ws
$ cd ~/ros_catkin_ws

$ rosinstall_generator ros_comm --rosdistro indigo --deps --wet-only --exclude roslisp --tar > indigo-ros_comm-wet.rosinstall
$ wstool init src indigo-ros_comm-wet.rosinstall
    
