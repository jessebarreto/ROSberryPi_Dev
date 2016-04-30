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
(MAKE SURE your /etc/apt/source.list line isnt commented)
$ cd ~/catkin_ws/external_src
$ sudo apt-get build-dep console-bridge
$ apt-get source -b console-bridge
$ sudo dpkg -i libconsole-bridge0.2*.deb libconsole-bridge-dev_*.deb

liblz4-dev
$ cd ~/catkin_ws/external_src
$ apt-get source -b lz4
$ sudo dpkg -i liblz4-*.deb

Resolving dependencies with rosdep

$ cd ~/catkin_ws
$ rosdep install --from-paths src --ignore-src --rosdistro indigo -y -r --os=debian:jessie

Building the catkin Workspace

Invoke catkin_make_isolated  to install ROS in /opt/ros/indigo like in Ubuntu

$ sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/indigo -j2

Setting the source of ROS in the bash

$ echo "source /opt/ros/indigo/setup.bash" >> ~/.bashrc
$ source /opt/ros/indigo/setup.bash

#Now ROS is Installed

To Update the workspace remember to 

Add relesead packages
$ cd ~/catkin_ws
$ rosinstall_generator ros_comm ros_control joystick_drivers --rosdistro indigo --deps --wet-only --exclude roslisp --tar > indigo-custom_ros.rosinstall

Update the workspace
$ wstool merge -t src indigo-custom_ros.rosinstall
$ wstool update -t src

Use rosdep to solve any new dependencies
$ rosdep install --from-paths src --ignore-src --rosdistro indigo -y -r --os=debian:jessie

With everything up to date rebuild the workspace
$ sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/indigo


#NOW ROS is Installed and Up2Date!

#For testing purposes: Install rosserial_embeddedlinux
(source: http://wiki.ros.org/rosserial_embeddedlinux/Tutorials)
(source: http://wiki.ros.org/rosserial_embeddedlinux/GenericInstall)
Install using catkin
$  cd ~/catkin_ws/src
$  git clone https://github.com/ros-drivers/rosserial.git
$  cd ~/catkin_ws/
$  catkin_make
$  catkin_make install
$  source <ws>/install/setup.bash

**If you find an error about a library called POCO follow this to install POCO C++ Project

Install it's dependencies
$ sudo apt-get install openssl libssl-dev

$ cd ~/catkin_ws/external_src/
$ wget https://github.com/andreasmuller/openframeWorks-Raspberry-PI/raw/master/LIBS/poco-1.4.3p1.tar.gz
$ gunzip poco-1.4.3p1.tar.gz
$ cd poco-1.4.3p1
$ ./configure --omit=Data/ODBC,Data/MySQL
$ make -s
$ sudo make -s install

