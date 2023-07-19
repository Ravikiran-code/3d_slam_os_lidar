# 3d_slam_os_lidar
The Simultaneous Localisation And Mapping algorithm to find accurate position and generates 3D Map. For this project we have used OS -128 LIDAR sensor. Could be used in Autonomous vehicles, terrain analysis, topography generation, GIS mapping and offroad analysis.

The HDL graph SLAM viewd in rviz
![Screenshot from 2023-07-18 21-02-17](https://github.com/Ravikiran-code/3d_slam_os_lidar/assets/58888116/1e63d24c-42eb-4e46-9f83-cfa16a8e41a6)

The 3D graph can be saved as .pcd and visualised in cloudcompare
![Screenshot from 2023-07-18 21-16-51](https://github.com/Ravikiran-code/3d_slam_os_lidar/assets/58888116/9e3aae06-36f5-47f6-b23e-aaf1b596d457)

Installation: 
1. Create dir hdl_grapgh_slam/src
2. cd hdl_grapgh_slam/src
3. Clone the repository
4. cd hdl_grapgh_slam
5. catkin_make

Use: 
source devel/setup.bash
roslaunch hdl_graph_slam hdl_graph_slam.launch
rosrun rviz rviz -d src/hdl_graph_slam/rviz/hdl_graph_slam.rviz
remap points data to /velodyne_points and imu data to /imu

Save 3d map: 
rosservice call /hdl_graph_slam/save_map "utm: false   
resolution: 0.5
destination: 'path/file_name.pcd'"
