
# Launch Kinova Movo simulation on ROS Melodic (Ubuntu 18.04.4)


## 1. OS Requirement

Ubuntu 18.04.4, which can be downloaded [here](http://old-releases.ubuntu.com/releases/bionic/ubuntu-18.04.4-desktop-amd64.iso).

If you plan to use a virtual machine, you can download [VirtualBox 6.0.24 and Extension Pack](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0), and follow [these instructions](https://wiki.epfl.ch/roscontrol/virtualboxmac) or watch this [demo video](https://www.youtube.com/watch?v=JzSOIW2-6BE&list=PL2kQRtildZTblkiHUDwPQopv2q5E2vd7H&index=5&t=0s).



## 2. Required Software Installation

Because Kinova Movo is not officially supported on Ubuntu 18.04.4 (and thus not on ROS Melodic), the official Kinova Movo automated installation script does not work properly. Thus, we need to install ROS Melodic, MoveIt 1.0 and Gazebo 9.x manually.

### 2.1 ROS

* ***2.1.1 Install ROS Melodic***

	Follow instructions [here](http://wiki.ros.org/melodic/Installation/Ubuntu), be sure to download **Desktop-Full Install: (Recommended)** at step **1.4**; and **modify and source** .bashrc file at step **1.5**.

	You can also follow [this video](https://www.youtube.com/watch?v=WKlk_2EGfM4&list=PL2kQRtildZTblkiHUDwPQopv2q5E2vd7H&index=12&t=0s).

* ***2.1.2 Create ROS Workspace***

	Follow instructions [here](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment).


### 2.2 MoveIt 1.0

* ***2.2.1 Install on Ubuntu 18.04***

	``` sudo apt install ros-melodic-moveit```

* ***2.2.2 [Create](https://ros-planning.github.io/moveit_tutorials/doc/getting_started/getting_started.html#create-a-catkin-workspace-and-download-moveit-source) & [Build](https://ros-planning.github.io/moveit_tutorials/doc/getting_started/getting_started.html#build-your-catkin-workspace) Catkin Workspace***

	Note: when you are building the workspace, if you get error message ```catkin: command not found ```, simply run command ```sudo apt-get install ros-melodic-catkin python-catkin-tools ``` to install catkin, and then repeat the previous step.

	You can also follow [this video](https://www.youtube.com/watch?v=Ki5wL6RHiqs).

### 2.3 Gazebo 
Note: Gazebo 9.x is the official version fully integrated and supported by ROS Melodic.

* ***2.3.1 Gazebo 9.x Installation  (from debian)***

	``` curl -sSL http://get.gazebosim.org | sh ```
	
* ***2.3.2 Make Sure Gazebo Installed Properly***

	1. [Test that stand-alone Gazebo works](http://gazebosim.org/tutorials?tut=ros_installing&cat=connect_ros#Testthatstand-aloneGazeboworks)
	2. [Test that you have the right version of Gazebo](http://gazebosim.org/tutorials?tut=ros_installing&cat=connect_ros#TestthatyouhavetherightversionofGazebo)
	
		&nbsp; &nbsp; &nbsp;The outputs should be
		
			 /usr/bin/gzserver 
			 /usr/bin/gzclient

* ***2.3.3 Install gazebo_ros_pkgs***

	```sudo apt-get install ros-melodic-gazebo-ros-pkgs ros-melodic-gazebo-ros-control ```
	
* ***2.3.4 Test Gazebo with ROS Integration***

	Follow instructions [here](http://gazebosim.org/tutorials?tut=ros_installing&cat=connect_ros#TestingGazebowithROSIntegration).

## 3. Kinova Movo Installation
Now, let's install the Kinova Movo simulator.

* ***2.4.1 Download from the [Official Repository](https://github.com/Kinovarobotics/kinova-movo)***

	``` cd ~/catkin_ws/src ```
	
	``` git clone https://github.com/Kinovarobotics/kinova-movo ```
	
* ***2.4.2 Build the Workspace***

	``` cd ~/catkin_ws && catkin_make ```
	
	