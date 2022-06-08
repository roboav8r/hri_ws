# hri_ws
Workspace for my Ph.D. research in human-robot interaction.

# Setup
git clone --recursive this repo

## Install dependencies
sudo apt-get install python3-rosdep -y
sudo apt-get install ros-foxy-depth-image-proc # needed for depthai-ros
sudo apt-get install ros-foxy-realsense2-camera
sudo rosdep init # "sudo rosdep init --include-eol-distros" for Dashing
rosdep update
rosdep install -i --from-path src

Python dependencies:
empy, lark


### Depthai
Follow install instructions here
https://github.com/luxonis/depthai-ros

## LibRealSense
If using realsense cameras, install the librealsense driver here:
https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md


## Miscellaneous

### Useful commands for configuring the lidar device
cd ~/hri_ws/src/rplidar_ros/
sudo bash scripts/create_udev_rules.sh 

ls -l /dev |grep ttyUSB
sudo chmod 666 /dev/ttyUSB0 

sudo wget -qO- https://raw.githubusercontent.com/luxonis/depthai-ros/main/install_dependencies.sh | sudo bash

## Build

colcon build --symlink-install

# Usage
Source the Workspace
cd ~/hri_ws
conda deactivate
soros2
. install/local_setup.bash

## Launch sensors
ros2 launch rplidar_ros2 view_rplidar_launch.py serial_port:="/dev/rplidar" frame_id:="laser" serial_baudrate:=115200
ros2 launch realsense2_camera rs_launch.py # Launches depth and RGB image

## Launch Skeletal pose tracking
ros2 run hri_skeletons run --ros-args --remap image:=/camera/color/image_raw -p debug:=true -p model:=$HOME/openvino_models/public/human-pose-estimation-3d-0001/FP16/human-pose-estimation-3d-0001.xml

# Misc fixes