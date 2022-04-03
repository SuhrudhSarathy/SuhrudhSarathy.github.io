---
title: Simulation of Drone using Gazebo and ROS
date: 2022-04-02
permalink: /posts/2020/08-path-planning
excerpt_separator: <!--more-->
toc: True
hidden: True
tags:
  - Simulation
  - Robotics
---
<!-- Introduction to the post -->

Simulation of robots plays an integral part in Algorithm development in Robotics. It allows you experiment, test and verify your algorithms before using them on real Robots. Simulation of Drones using Gazebo plays the same rule. This post aims to serve as a repository of steps that can be taken to simulate a Drone using Gazebo and communicate with it using ROS and PX4.
<!--more-->

This post talks about Installation of ROS, Gazebo and PX4 ecosystem for Drone Simulation. Other simulation platforms are linked below in the References section. In future (maybe), a ROS package to write controllers and use them might also be added.

# Installation
## ROS Installation
1. ROS can be installed from the Installation in [ROS Wiki](http://wiki.ros.org/melodic/Installation/Ubuntu). Make sure the correct version of ROS is installed as per your Ubuntu Installation. You can take a look at a quick guide below to know the correct version of ROS and Ubuntu distributions.

| Ubuntu Installation | ROS Version | Python Version |
| :---:               | :---:       | :---:          |
| 16.04               | Kinetic     | python2        |
| 18.04               | Melodic     | python2        |
| 20.04 (Recommended) | Noetic      | python3        |

__Note__: Make sure to follow instructions to the line. Most of the installation issues can be solved by doing this.

2. Install `catkin_tools` from their [official documentation](https://catkin-tools.readthedocs.io/en/latest/installing.html). `catkin_tools` provide useful tools to build ROS Workspaces and can be used instead of `catkin_make`

3. Make a catkin workspace to install all the packages. If you already have a catkin workspace (usually `catkin_ws`) this step can be ignored. (Make sure to replace `mav_ws` to your own workspace name in all the commands of this post).

bash```
mkdir -p mav_ws/src
cd mav_ws
catkin init
```

After the build process, you need to __source__

