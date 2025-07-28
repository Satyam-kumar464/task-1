**Prerequisites**
 
 Ensure ROS 2 Jazzy is installed and sourced:
```bash
source /opt/ros/jazzy/setup.bash
```
Create and build your workspace:
```
mkdir -p ~/turtlebot3_ws/src
cd ~/turtlebot3_ws/src
git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
cd ..
colcon build --symlink-install
source install/setup.bash
```
Add these to your .bashrc:
```
export TURTLEBOT3_MODEL=burger
export LDS_MODEL=none
source ~/turtlebot3_ws/install/setup.bash
```

**Clone Required Repositories**

Gazebo Worlds Dataset
```
git clone https://github.com/mlherd/Dataset-of-Gazebo-Worlds-Models-and-Maps.git
```
TurtleBot3
```
git clone https://github.com/mlherd/Dataset-of-Gazebo-Worlds-Models-and-Maps.git
```
**World Selection & Modification**

Browse the worlds/ folder in the dataset repo.

Choose one of the first five worlds (e.g., small_house.world, office.world, etc.).

Open the .world file and remove dynamic objects (e.g., humans, vehicles) if needed.

Place the world file in your Gazebo launch path or modify your launch file to load it.

{ I have choosen turtlebot3 houseworld for this task}

**Mapping the Environment**

Step 1: Launch Gazebo with the selected world

Terminal 1:
```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```
Step 2: Launch SLAM Toolbox

Terminal 2:
```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```
Step 3: Teleoperate the Robot

Terminal 3:
```
export TURTLEBOT3_MODEL=burger
ros2 run turtlebot3_teleop teleop_keyboard
```
Step 4: Save the Map

Terminal 4:
```
ros2 run nav2_map_server map_saver_cli -f ~/turtlebot3_map
```
**Launch Navigation2 with Saved Map**

Step 1: Launch Gazebo again

Terminal 1:
```
ros2 launch turtlebot3_gazebo turtlebot3_house.launch.py
```
Step 2: Launch Navigation2

Terminal 2:
```
ros2 launch turtlebot3_navigation2 navigation2.launch.py map:=~/turtlebot3_map.yaml
```
Set Navigation Goal in RViz

In RViz, click “2D Pose Estimate” to localize the robot.
Click ```Nav2 Goal``` and drag to set the goal.
Watch the robot navigate from point A to point B!