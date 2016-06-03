# ROSberryPi_Dev

Installing the ROS on Raspberry Pi 3 and Raspberry Pi 2 B and in a FPGA SOC Kit Board   

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

libpoco-dev
$ cd ~/catkin_ws/external_src
$ sudo apt-get install libpoco-dev

curl
$ sudo apt-get install curl
$ sudo apt-get install libcurl4-openssl-dev

libboost
$ sudo apt-get install libboost-all-dev

libpython2.7-stdlib
$ sudo apt-get install libpython2.7-stdlib

urdfdom
$ sudo apt-get install liburdfdom-dev liburdfdom-tools 

Resolving dependencies with rosdep

$ cd ~/catkin_ws
$ rosdep install --from-paths src --ignore-src --rosdistro indigo -y -r --os=debian:jessie

Building the catkin Workspace

Invoke catkin_make_isolated  to install ROS in /opt/ros/indigo like in Ubuntu

$ sudo ./src/catkin/bin/catkin_make_isolated --install --DCMAKE_BUILD_TYPE=Release --install-space /opt/ros/indigo -j2

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
$  source ~/catkin_ws/install/setup.bash

**If you find an error about a library called POCO follow this to install POCO C++ Project

Install it's dependencies
$ sudo apt-get install openssl libssl-dev

$ cd ~/catkin_ws/external_src/
$ wget https://github.com/andreasmuller/openframeWorks-Raspberry-PI/raw/master/LIBS/poco-1.4.3p1.tar.gz
$ gunzip poco-1.4.3p1.tar.gz
$ tar -xf poco-1.4.3p1.tar
$ cd poco-1.4.3p1
$ ./configure --omit=Data/ODBC,Data/MySQL
#PAREI AQUI $ make -s
$ sudo make -s install

#Installing ROS on Ubuntu 14.04 on Raspberry with kernel 3.18

First it is needed to install ubuntu img on an SD card "big" enough
Following the tutorial on https://www.raspberrypi.org/documentation/installation/installing-images/linux.md 

Check which device is the SDcard with
$ df -h

(In my computer it was the device /dev/sdc)
Then unmount it
$ sudo umount /dev/sdc

Write the img on the card with
$ sudo ddrescue -d -D --force ubuntuXXXX.img /dev/sdX


#Done the Raspberry already has the Ubuntu but we need to configure it

Install the WiFi Adapter in the Raspberry Pi 2
MAC e8:4e:06:2b:9e:c4

Now on the Ubuntu make sure you can have access to the internet




# Install ROS on SoC Kit Arrow Terasic with FPGA Cyclone V

First of all, it's necessary to install the image of the linux on a SD Card. As before, the same steps has to be followed:

Check which device is the SD card with
$ df -h

(Once again, in my PC the device is in /dev/sdc)
Then we unmount it
$ sudo umount /dev/sdc

May 26, 2016
Sadly, this distro does not have any package manager (apt or dpkg)!
Download the image on the link:
https://s3.amazonaws.com/rocketboards/releases/2014.01/sockit-gsrd/bin/linux-sockit-gsrd-13.1-bin.tar.gz

Extract the files in tar
$ tar -xzf linux-sockit-gsrd-13.1-bin.tar.gz

Change the files permission with
$ cd ./linux-sockit-gsrd-13.1-bin
$ chmod -R 777 *

Extract the image
$ tar -xzf sd_image_sockit_20140126.tar.gz


Write the image with the command dd or ddrescue
$ sudo dd if=sockit_20140126.img of=/dev/sdc bs=512

Then to remove the usb not "kinda" ruining everything you've done
$ sudo sync

Then you have to insert the SD card on the board and set some jumpers
The jumper bootselection has to stay
bootselection0 = 1-2
bootselection1 = 2-3
bootselection2 = 1-2

The jumpers clk selection has to stay
clk_selection0 = 1-2
clk_selection1 = 1-2

And we also have to set the switches under the board to
MSEL[4:0] = 000001

Then connect the uart-usb cable between the FPGA and the computer
and the Power source on the board and turn on the board using the RED button.

Now to set the usb-serial communication between the PC and the board it is necessary to follow these steps:

Install the minicom program on linux
$ sudo apt-get install minicom

Check which USB serial device the board is connect on the computer with
$ls /dev/ttyUSB*

Now we need to configure the minicom
$ minicom
then select the help screen with CTRL+a z
then select the configuration screen with CTRL+a o

Now select "Serial port setup" and configure the minicom to connect over the serial port /dev/ttyUSB0 , and use 115,200 baud, 1 stop bit, no CRC and no flow control (usually referred to as "115200-N-1") and save it quick access with the option save as "FPGA_SOCKit"

close the minicom with CTRL+a x

After this all just reset the FPGA and open minicom again
$ minicom FPGA_SOCKit
	 
NOW WE ALREADY CAN ACCESS THE FPGA THROUGH UART COMMUNICATION

////////////////
Because the previous linux doesn't have a package manager, we prefered to download a second image to test

To download the second image use this link:
http://www.terasic.com/downloads/cd-rom/sockit/linux_BSP/SoCKit_RevD_LXDE.zip

After the download do the same procedure as before to write the image on the SD card

When booting we checked if this distro is good to go and that was the case!

To check about this distro
$ uname -a
Linux localhost.localdomain 3.13.0-00298-g3c7cbb9-dirty #4 SMP Thu Jul 10 15:01:
13 CST 2014 armv7l armv7l armv7l GNU/Linux 

$ lsb_release -a
No LSB modules are available.                                                   
Distributor ID: Linaro                                                          
Description:    Linaro 13.04                                                    
Release:        13.04                                                           
Codename:       quantal  

However, everything seems to be good, this distro is based in an outdate Ubuntu 12.10, a distro that doesn't ahve support anymore, and because of that that's some problems with the repository and with the apt-get.

To change that we started to update the Linaro 13.10 to a Linaro 14.01 and then Ubuntu 14.04. Following this tutorial: 
http://homecircuits.eu/blog/ubuntu-12-10-to-13-10-and-to-14-04/

Changing the source.list file and add the follow lines on nano
/#deb http://ports.ubuntu.com/ubuntu-ports/ quantal main universe
deb http://old-releases.ubuntu.com/ubuntu/ quantal main restricted universe multiverse
deb http://old-releases.ubuntu.com/ubuntu/ quantal-updates main restricted universe multiverse
deb http://old-releases.ubuntu.com/ubuntu/ quantal-security main restricted universe multiverse

/#deb-src http://ports.ubuntu.com/ubuntu-ports/ quantal main universe
deb-src http://old-releases.ubuntu.com/ubuntu/ quantal main universe

Now, it's possible to use update
$ sudo apt-get update

Now let's upgrade some old packages
$ sudo apt-get upgrade

and finally upgrade the distro
sudo apt-get dist-upgrade

First of all we realize that this distro is based on a linaro 13.10()