# Launching Kinova Movo simulation on ROS Melodic (Ubuntu 18.04.4)


## 1. System Requirement

Ubuntu 18.04.4, which can be downloaded [here](http://old-releases.ubuntu.com/releases/bionic/ubuntu-18.04.4-desktop-amd64.iso).

If you plan to use a virtual machine, you can download [VirtualBox 6.0.24 and Extension Pack](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0), and follow [instructions](https://wiki.epfl.ch/roscontrol/virtualboxmac) or
watch this [demo video](https://www.youtube.com/watch?v=JzSOIW2-6BE&list=PL2kQRtildZTblkiHUDwPQopv2q5E2vd7H&index=5&t=0s).



## 2. Manual installation

Because Kinova Movo is not officially supported on Ubuntu 18.04.4 (and thus not on ROS Melodic), the official automated installation script does not work. Thus, we need to install manually.

### 2.2 ROS

#### 2.1.1 Install ROS Melodic

Follow instructions [here](http://wiki.ros.org/melodic/Installation/Ubuntu), be sure to download **Desktop-Full Install: (Recommended)** at step **1.4**, and **modify and source** .bashrc file at  **1.5**.

[This video](https://www.youtube.com/watch?v=WKlk_2EGfM4&list=PL2kQRtildZTblkiHUDwPQopv2q5E2vd7H&index=12&t=0s) can be referenced.

#### 2.1.2 Create a ROS Workspace

Follow instructions [here](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment).


### 2.2 MoveIt

#### 2.2.1 Install on Ubuntu 18.04
``` 
sudo apt install ros-melodic-moveit
```

#### 2.2.2 