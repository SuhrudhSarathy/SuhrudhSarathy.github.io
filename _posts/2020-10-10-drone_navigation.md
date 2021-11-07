---
title: Autonomous Drone Navigation
date: 2020-08-08
permalink: /posts/2020/10-auto-drone-nav
excerpt_separator: <!--more-->
toc: false
tags:
  - Drone
  - Robotics
---
<!-- Introduction to the Project-->
During the pandemic, I was at home without any access to any physical robots. It was the perfect time to put into practise the concepts of Robotics that I learnt in the summer. I got quite intriguied by Autonomous drones. My greatest inspirations came after watching [this](https://www.youtube.com/watch?v=8RILnqPxo1s) video on __Deep Drone Racing__ by the _Robotics and Perception group_ at UZH and a bunch of really cool [work](https://www.youtube.com/playlist?list=PLu70ME0whad8hdnmNtTlttJrQcGk95udj) by the _Autonomous Robots Lab_ at NTNU. Taking inspiration, I wanted to try out working with Indoor Navigation of Autonomous Drones.

# Simulator
I was in search of a simulator to work on Drones. I found [rotors_simulator](https://github.com/ethz-asl/rotors_simulator) from ETHZ-ASL. Unfortunately, it was hard to install it on Ubuntu 18.04 with Gazebo 9. So I installed [CrazyS](https://github.com/gsilano/CrazyS), an extension of RotorS. This repository has proper instructions for installation in Ubuntu 18.04 and with Gazebo 9.

## Benifits of using a Simulator
The obvious benifit of using a simulator is the absence of need for the actual drone. However, some other benifits that I feel were important for my project were:
1. __Odometry__ : The ground truth plugin from Gazebo made it really easy to obtain accurate odometry. This allowed me to concentrate on specific parts of the project that I was interested on.
2. __VI Sensor__ : A Visual Inertial Sensor was simulated. This allowed me to access the depth image from the sensor to further use it in perception.
3. __Controller__ : The [launch](https://github.com/gsilano/CrazyS/blob/master/rotors_gazebo/launch/mav_hovering_example_with_vi_sensor.launch) file launched `lee_position_controller_node` which had a [controller](https://github.com/gsilano/CrazyS/blob/master/rotors_control/src/nodes/lee_position_controller_node.cpp) implemented. This made the job really easy for me to concentrate on planning and perception only.

## Launching the drone simulation
1. Launch `firefly` hovering (without VI Sensor)
```bash
roslaunch rotors_gazebo mav_hovering_example.launch
```
2. Launch `firefly` hovering (with VI Sensor)
```bash
roslaunch rotors_gazebo mav_hovering_example_with_vi_sensor.launch
```
This launches the drone in an empty world. To change the world, you can pass the world as an argument or take a modify the launch [file](https://github.com/gsilano/CrazyS/blob/master/rotors_gazebo/launch/mav_hovering_example_with_vi_sensor.launch#L4)

# Perception
The VI Sensor on board publishes Point Cloud Data as `sensor_msgs/PointCloud2` message type. To perform collision checking, I tried two methods:
1. ## Point Cloud Library (PCL)
I used the Point Cloud Library to `filter`, `transform` and perform `search` of the Point Cloud. \
### Filtering
The Point Cloud data that was published was very dense and noisy. So I applied `Pass Through Filter` and `Voxel Grid Filter` to reduce the distance and density of the incoming Point Cloud.
```c++
// Pass Through Filter
pcl::PassThrough<pcl::PointXYZ> pass;
pass.setInputCloud(cloud);
pass.setFilterFieldName("z");
pass.setFilterLimits(0.0, 3.0);

pass.filter(*cloud_pt);

// Voxel Grid Filter
pcl::VoxelGrid<pcl::PointXYZ> sor;
sor.setInputCloud(cloud_pt);
sor.setLeafSize(0.05, 0.05, 0.05);
sor.filter(*cloud_voxel);
```
### Transformation
The incoming Point Cloud was in the `vi_sensor` frame. To perform collision checking in the `world` frame, the Point Cloud had to be transformed.
```c++
try
{
    geometry_msgs::TransformStamped transform;
    transform = tfBuffer.lookupTransform("world", msg->header.frame_id ,ros::Time(0), ros::Duration(3.0));
    tf::Transform tftrans;
    tf::transformMsgToTF(transform.transform, tftrans);
    pcl_ros::transformPointCloud(*cloud_voxel, cloud_out, tftrans);
}
catch(tf2::TransformException &ex)
{
    ROS_WARN("%s", ex.what());
}
```
### Search
To check for collision of any point in space given the Point Cloud data, I used the inbuilt `Octree` representation of Point Cloud to check if the queried point was surrounded by any Points. 
```c++
pcl::octree::OctreePointCloudSearch<pcl::PointXYZ> octree(resolution);
octree.setInputCloud(cloud);
octree.addPointsFromInputCloud();

if(octree.radiusSearch(searchPoint, radius, idx, distances) > 0){
		resp.collision = true;
		// Collsion
	}
	else{
		resp.collision = false;
		// No collision
	}
``` 
### PCL References
1. [Voxel Grid Filter](https://pointclouds.org/documentation/tutorials/voxel_grid.html)
2. [Pass Through Filter](https://pointclouds.org/documentation/tutorials/passthrough.html)
3. [Octree Point Cloud Search](https://pointclouds.org/documentation/classpcl_1_1octree_1_1_octree_point_cloud_search.html)

2. ## Voxblox 
<!-- Simulator -->
<!--Perception pipeline-->
<!--Planner-->
<!--Post Processing-->