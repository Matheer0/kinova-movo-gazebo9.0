
# Launch Kinova MOVO simulation on ROS Melodic (Ubuntu 18.04.4)


## 1. OS Requirement

Ubuntu 18.04.4, which can be downloaded [here](http://old-releases.ubuntu.com/releases/bionic/ubuntu-18.04.4-desktop-amd64.iso).

If you plan to use a virtual machine, you can download [VirtualBox 6.0.24 and Extension Pack](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0), and follow [these instructions](https://wiki.epfl.ch/roscontrol/virtualboxmac) or watch this [demo video](https://www.youtube.com/watch?v=JzSOIW2-6BE&list=PL2kQRtildZTblkiHUDwPQopv2q5E2vd7H&index=5&t=0s).



## 2. Required Software Installation

Because Kinova MOVO is not officially supported on Ubuntu 18.04.4 (and thus not on ROS Melodic), the official MOVO automated installation script does not work properly. Thus, we need to install ROS Melodic, MoveIt 1.0 and Gazebo 9.x manually.

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

	Note: when you are building the workspace, if you get error message ```catkin: command not found ```, simply run command ```sudo apt-get install ros-melodic-catkin python-catkin-tools ``` to re-install catkin, and then repeat the previous step.

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

## 3. Install and Launch Kinova MOVO

Now, let's install the Kinova MOVO packages.

* ***3.1 Download from the [Official Repository](https://github.com/Kinovarobotics/kinova-movo)***

	``` cd ~/catkin_ws/src ```
	
	``` git clone https://github.com/Kinovarobotics/kinova-movo ```
	
* ***3.2 Edit .bashrc File***
	
	* Add [following line](https://github.com/Kinovarobotics/kinova-movo/wiki/1.-Setup-Instructions#kinova-movo-package-install-on-development-computer) to .bashrc file, and source it
	
		```echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc ```

	* Re-source your .bashrc with ```source ~/.bashrc```

* ***3.3 Build the Workspace***

	Note: There are some packages (see next section) that are causing problems, so we will ignore them for now when we are building the workspace, and we will use command [catkin_make -DCATKIN_BLACKLIST_PACKAGES](https://answers.ros.org/question/54181/how-to-exclude-one-package-from-the-catkin_make-build/) to [blacklist them](https://github.com/catkin/catkin_tools/pull/152).

	* Go to ROS catkin workspace ``` cd ~/catkin_ws ```

	* Build the package using command:

		``` catkin_make -DCATKIN_BLACKLIST_PACKAGES="kinect2_bridge;kinect2_calibration;kinect2_viewer;kinect2_registration;gazebo_force_based_move;gazebo_grasp_plugin;range_sensor_filters;roboticsgroup_gazebo_plugins;eband_local_planner;movo_assisted_teleop;joint_trajectory_controller" ```

		(If you get any CMake Error like "Could not find a package configuration file provided by ... ", simply [do this](https://github.com/tu-darmstadt-ros-pkg/hector_quadrotor/issues/73#issuecomment-539649217); then repeat catkin_make command above.)

* ***3.4 Lauch Kinova MOVO***

	Now, Kinova MOVO simulation can be launched in Gazebo using command 

	```roslaunch movo_gazebo movo.launch ```

## 4. Problematic MOVO Packages

Currently, if we do command ```cd ~/catkin_ws && catkin_make``` directly, there are 11 packages that are causing error messages, either because of missing Freenect2 files or compilation errors from .cpp files in the sub src folder.

* Problematic packages due to Freenect2:

	```kinect2_bridge```, ```kinect2_calibration```, and ```kinect2_viewer```,  all in folder movo_common/movo_third_party/[iai_kinect2](https://github.com/Kinovarobotics/kinova-movo/tree/master/movo_common/movo_third_party/iai_kinect2)

* Problematic packages due to corresponding .cpp files:

	* ```kinect2_registration```, caused by [kinect2_registration.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_common/movo_third_party/iai_kinect2/kinect2_registration/src/kinect2_registration.cpp)

	* ```gazebo_force_based_move```, caused by [gazebo_ros_force_based_move.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_simulation/movo_gazebo_plugins/gazebo_force_based_move/src/gazebo_ros_force_based_move.cpp)

	* ```gazebo_grasp_plugin```, caused by [GazeboGraspFix.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_simulation/movo_gazebo_plugins/gazebo_grasp_plugin/src/GazeboGraspFix.cpp)

	* ```range_sensor_filters```, caused by [range_sensor_filters.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_robot/movo_sensor_filters/range_sensor_filters/src/range_sensor_filters.cpp)

	* ```roboticsgroup_gazebo_plugins```, caused by [mimic_joint_plugin.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_simulation/movo_gazebo_plugins/roboticsgroup_gazebo_plugins/src/mimic_joint_plugin.cpp)

	* ```eband_local_planner```, caused by [eband_local_planner_ros.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_common/movo_third_party/eband_local_planner/src/eband_local_planner_ros.cpp)

	* ```movo_assisted_teleop```, caused by [movo_assisted_teleop.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_common/movo_navigation/movo_assisted_teleop/src/movo_assisted_teleop.cpp)

	* ```joint_trajectory_controller```, caused by [joint_trajectory_controller.cpp](https://github.com/Kinovarobotics/kinova-movo/blob/master/movo_common/movo_third_party/joint_trajectory_controller/src/joint_trajectory_controller.cpp)

## 5. Helpful Links

*  With the help of following detailed articles, you can get a feel of launching and controlling simulations in Gazebo, by using other (well supported) robots
	* [TurtleBot3](https://automaticaddison.com/how-to-launch-the-turtlebot3-simulation-with-ros/)
	* [PR2 Robot](http://wiki.ros.org/pr2_simulator/Tutorials/StartingPR2Simulation), and [control](http://wiki.ros.org/pr2_simulator/Tutorials/WorkingWithGazeboOverRos)

* [Upgrade](https://www.theconstructsim.com/all-about-gazebo-9-with-ros/) Gazebo to version 9.x
 



	