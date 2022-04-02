---
title: Simulation of Drone using Gazebo and ROS
date: 2022-04-02
permalink: /posts/2020/08-path-planning
excerpt_separator: <!--more-->
toc: True
tags:
  - Simulation
  - Robotics
---
<!-- Introduction to the post -->

Simulation of robots plays an integral part in Algorithm development in Robotics. It allows you experiment, test and verify your algorithms before using them on real Robots. Simulation of Drones using Gazebo plays the same rule. This post aims to serve as a repository of steps that can be taken to simulate a Drone using Gazebo and communicate with it using ROS and PX4.
<!--more -->

This post talks about Installation of ROS, Gazebo and PX4 ecosystem for Drone Simulation. Other simulation platforms are linked below in the References section. In future (maybe), a ROS package to write controllers and use them might also be added.

# Installation
## ROS Installation
1. ROS can be installed from the Installation in [ROS Wiki](http://wiki.ros.org/melodic/Installation/Ubuntu). Make sure the correct version of ROS is installed as per your Ubuntu Installation. You can take a look at a quick guide below to know the correct version of ROS and Ubuntu distributions.

| Ubuntu Installation | ROS Version | Python Version |
| :---:               | :---:       | :---:          |
| 16.04               | Kinetic     | python2        |
| 18.04               | Melodic     | python2        |
| 20.04 (Recommended) | Noetic      | python3        |
