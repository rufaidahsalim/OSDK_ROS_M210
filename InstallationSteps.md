# DJI Onboard SDK ROS 4.0.0


###  Prerequisites
The system environment we have tested is in the table below.

|                            |                                             |
|----------------------------|-------------------------------------------- |  
| **system version**         | ubuntu 16.04                                |
| **processor architecture** | x86(mainfold2-c),armv8(mainfold2-c)           |
#### Firmware Compatibility
OSDK-ROS4.0's firmware compatibility depends on onboard-sdk4.0.0's. you can get more information [here](https://github.com/dji-sdk/Onboard-SDK);

#### Ros  
you need to install ros first.Install instruction can be found at: http://wiki.ros.org/ROS/Installation. We just tested ROS kinetic version.  
#### C++11 Compiler
We compile with C + + 11 Standard.

#### onboard-sdk
you need to download onboard-sdk4.0.0,and install it.
>$mkdir build  
>$cd build  
>$cmake..  
>$sudo make -j7 install


#### nema-comms
> $sudo apt install ros-{release}-nmea-comms  //ROS Kinetic is downloaded 

#### ffmpeg
> $sudo apt install ffmpeg  
#### libusb-1.0-0-dev
> $sudo apt install libusb-1.0-0-dev
#### opencv3.x
We use OpenCV to show images from camera stream. Download and install instructions can be found at: http://opencv.org. Tested with OpenCV 3.2.0.

### Permission
#### uart permission
You need to add your user to the dialout group to obtain read/write permissions for the uart communication.
>$sudo usermod -a -G dialout ${USER}  
>
Then log out of your user account and log in again for the permissions to take effect.

#### usb permission
You will need to add an udev file to allow your system to obtain permission and to identify DJI USB port.
>$cd /etc/udev/rules.d/  
>$sudo vi DJIDevice.rules

Then add these content into DJIDevice.rules.
>$SUBSYSTEM=="usb", ATTRS{idVendor}=="2ca3", MODE="0666"

At last,you need to reboot your computer to make sure it works.

### Building dji_osdk_ros pkg
#### create workspace
If you don't have a catkin workspace, create one as follows:
>$mkdir test1_ws
>$cd test1_ws  
>$mkdir src  
>$cd src  
>$catkin_init_workspace
#### add osdk-ros 4.0 
Download osdk-ros 4.0 and put it into src.
#### Build the dji_osdk_ros ROS package
>$cd ..  
>$catkin_make
#### Configuration
1.Remember to source your setup.bash.
>$source devel/setup.bash  

2.Edit the launch file and enter your App ID, Key, Baudrate and Port name in the designated places.  
(__note:there are two launch file.  
dji_sdk_node.launch is for dji_sdk_node.(3.8.1's interface)  
dji_vehicle_node is for dji_vehicle_node(4.0.0's interface)__)
> $rosed dji_osdk_ros dji_sdk_node.launch  
> $rosed dji_osdk_ros dji_vehicle_node.launch  

3.Remember to add UserConfig.txt to correct path.(in the current work directory)  
>If you want to run dji_sdk_node.launch, you need to put UserConfig.txt into /home/{user}/.ros.
>dji_vehicle_node.launch does not need UserConfig.txt.
#### Running the Samples
1.Start up the dji_osdk_ros ROS node.  
if you want to use OSDK ROS 4.0.0's services and topics:
>$roslaunch dji_osdk_ros dji_vehicle_node.launch  

if you want to adapt to OSDK ROS 3.8.1's services and topics:
>$roslaunch dji_osdk_ros dji_sdk_node.launch    
>
2.Open up another terminal and cd to your catkin_ws location, and start up a sample (e.g. flight control sample).
>$source devel/setup.bash  
>$rosrun dji_osdk_ros flight_control_node  
>
__note:if you want to rosrun dji_sdk_node,you need to put UserConfig.txt into current work directory.__  
3.Follow the prompt on screen to choose an action for the drone to do.


