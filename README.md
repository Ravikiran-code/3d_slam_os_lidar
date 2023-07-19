# 3D SLAM using OS LIDAR
***3D SLAM using OS LIDAR*** is an open source ROS package for real-time 3D SLAM using a Ouster Sensor 128 LIDAR. It is based on 3D Graph SLAM with NDT scan matching-based odometry estimation and loop detection. Mainly used in Autonomous Vehicles, Terrain mapping and analysis, GIS Topography, Digital Elevation Map generation etc

The SLAM and estimated position viewd in Rviz:

![Screenshot from 2023-07-18 21-02-17](https://github.com/Ravikiran-code/3d_slam_os_lidar/assets/58888116/3d6dd3a7-e546-47e9-9fa5-3aaedf5d2266)

Then 3D map can be stored as .pcd file and aanalysed using cloudcompare

![Screenshot from 2023-07-18 21-16-51](https://github.com/Ravikiran-code/3d_slam_os_lidar/assets/58888116/ecd660d1-7274-458c-8c81-3db6602a8274)


## Requirements
***3D SLAM using OS LIDAR*** requires the following libraries:

- OpenMP
- PCL
- g2o
- suitesparse

The following ROS packages are required:

- geodesy
- nmea_msgs
- pcl_ros
- [ndt_omp](https://github.com/koide3/ndt_omp)
- [fast_gicp](https://github.com/SMRT-AIST/fast_gicp)

```bash
# for melodic
sudo apt-get install ros-melodic-geodesy ros-melodic-pcl-ros ros-melodic-nmea-msgs ros-melodic-libg2o
cd catkin_ws/src
git clone https://github.com/koide3/ndt_omp.git -b melodic
git clone https://github.com/SMRT-AIST/fast_gicp.git --recursive
git clone https://github.com/koide3/hdl_graph_slam

cd .. && catkin_make -DCMAKE_BUILD_TYPE=Release

# for noetic
sudo apt-get install ros-noetic-geodesy ros-noetic-pcl-ros ros-noetic-nmea-msgs ros-noetic-libg2o

cd catkin_ws/src
git clone https://github.com/koide3/ndt_omp.git
git clone https://github.com/SMRT-AIST/fast_gicp.git --recursive
git clone https://github.com/koide3/hdl_graph_slam

cd .. && catkin_make -DCMAKE_BUILD_TYPE=Release
```

**[optional]** *bag_player.py* script requires ProgressBar2.
```bash
sudo pip install ProgressBar2
```

## Example1 (Indoor)

Bag file (recorded in a small room):

- [hdl_501.bag.tar.gz](http://www.aisl.cs.tut.ac.jp/databases/hdl_graph_slam/hdl_501.bag.tar.gz) (raw data, 344MB)
- [hdl_501_filtered.bag.tar.gz](http://www.aisl.cs.tut.ac.jp/databases/hdl_graph_slam/hdl_501_filtered.bag.tar.gz) (downsampled data, 57MB, **Recommended!**)
- [Mirror link](https://zenodo.org/record/6960371)

```bash
rosparam set use_sim_time true
roslaunch hdl_graph_slam hdl_graph_slam_501.launch
```

```bash
roscd hdl_graph_slam/rviz
rviz -d hdl_graph_slam.rviz
```

```bash
rosbag play --clock hdl_501_filtered.bag
```

We also provide bag_player.py which automatically adjusts the playback speed and processes data as fast as possible.

```bash
rosrun hdl_graph_slam bag_player.py hdl_501_filtered.bag
```


You can save the generated map by:
```bash
rosservice call /hdl_graph_slam/save_map "utm: false   
resolution: 0.05
destination: 'path/file_name.pcd'"
```


## Use 3D SLAM using OS LIDAR in your system

1. Define the transformation between your sensors (LIDAR, IMU, GPS) and base_link of your system using static_transform_publisher (see line #11, hdl_graph_slam.launch). All the sensor data will be transformed into the common base_link frame, and then fed to the SLAM algorithm.

2. Remap the point cloud topic of ***prefiltering_nodelet***. Like:

```bash
  <node pkg="nodelet" type="nodelet" name="prefiltering_nodelet" ...
    <remap from="/velodyne_points" to="/rslidar_points"/>
  ...
```


