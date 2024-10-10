# Husky_with_Freespace_Segmentation
![Nvidia](https://camo.githubusercontent.com/782b1fbf320f714fc832dd80f02b0db9b8765e3f7577aaa478cf7d6ba235bb33/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76313f7374796c653d666f722d7468652d6261646765266d6573736167653d4e564944494126636f6c6f723d323232323232266c6f676f3d4e5649444941266c6f676f436f6c6f723d373642393030266c6162656c3d)![Docker](https://camo.githubusercontent.com/4ec342876a40b53ffc6230a41196528690f9f42b1098fd354df46c649720b4c6/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76313f7374796c653d666f722d7468652d6261646765266d6573736167653d446f636b657226636f6c6f723d323439364544266c6f676f3d446f636b6572266c6f676f436f6c6f723d464646464646266c6162656c3d)![OpenCV](https://camo.githubusercontent.com/83d8a90be61c85c17da1e70a56d4e9fc5943ec1b91ddc47726b9485689a8c3b2/68747470733a2f2f696d672e736869656c64732e696f2f7374617469632f76313f7374796c653d666f722d7468652d6261646765266d6573736167653d4f70656e435626636f6c6f723d354333454538266c6f676f3d4f70656e4356266c6f676f436f6c6f723d464646464646266c6162656c3d)


Reseach Project using Nvidia Isaac ROS Bi3D Freespace Segmentation

## I. Installations

Step 1. Install or update your NVIDIA Drivers: https://ubuntu.com/server/docs/nvidia-drivers-installation .

Step 2. Install the Omniverse Laucher: https://www.nvidia.com/en-us/omniverse/download/ .
You will also have to create a Omniverse account during this step.

Step 3. Install Docker: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository .

Step 4. Install the NVIDIA-container-toolkit: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-apt .

Step 5. Install Isaac for workstation: https://docs.omniverse.nvidia.com/isaacsim/latest/installation/install_workstation.html .
If you are using a Linux system please follow the extra instructions that NVIDIA has provided.


Step 6. The comptue setup needed to use Isaac ROS: https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html

## II. Setup of Isaac Sim with Husky

Step 1. Clone or Download this GitHub repository 

Step 2. Lauch Isaac Sim from the Omniverse Laucher

Step 3. Isaac Sim App Selector window will appear. We want to select the option with open in terminal. Then paste the following commands into this terminal.

```bash
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp && LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PWD/exts/omni.isaac.ros2_bridge/humble/lib
```

```bash
./isaac-sim.sh 
```

These commands above allows us to build the ROS2 Humble brigde for the Isaac Sim.

Note: If you try to put ```export RMW_IMPLEMENTATION=rmw_fastrtps_cpp && LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PWD/exts/omni.isaac.ros2_bridge/humble/lib``` into your ```~/.bashrc``` , then Isaac Sim will crash on startup.
Also. if you try to put these commands into ```Extra Args:```, then Isaac Sim will crash on startup.

Step 4. Click the ```File``` tab and click on ```Open```. Then open the file: ```husky_ros2.usd```. This file has all of the ROS2 topics neeed to do the Freespace Segmentation. [So do not edit the Husky action graphs] 

Note: Please do not drag and drop the ```husky_ros2.usd``` into Isaac Sim. This will not build the ROS2 action graphs.

Step 5. The ```husky_ros2.usd``` will apper as empty this is due to the error of the ```husky_clem24.usd``` needing a prim path.

![prim path](https://github.com/jkcolem/Husky_with_Freespace_Segmentation/blob/main/Screenshot%20from%202023-12-14%2016-05-31.png)

Click the Folder button go exactly where ```husky_clem24.usd``` file is. [If you have multipe copies of this please choose the one that's in the repo]

Step 6. Once this error code is fixed and you change lighting to ```Stage Lights```. Your Isaac Sim should look similar to the following:

![husky_in_isacc_sim](https://github.com/jkcolem/Husky_with_Freespace_Segmentation/blob/main/Screenshot%20from%202023-12-15%2016-01-10.png)

Note: If you click play in this state the husky will drop down to infinity. To test to see if the ROS2 action graphs are working, you can create a physic scene flat grid for the husky to interact with. Then create a shape that has collider preset so the lidar will be able to dectect it. Run ```RVIZ2``` in a sepreate terminal (requires you have ROS2 Humble. But in Part IV we will create a ROS2 docker container where you will able to do this command.).

## II. Moving the Husky in Isaac Sim

### A) Publishing Twist Message

```bash
ros2 topic pub /cmd_vel geometry_msgs/Twist '{linear:  {x: 1.0, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.0}}'
```

### B) Teleop

Step 1. Follow Turtlebot3 Gazebo Simulation Packages: https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation/ . This allow us to have the teleoporation for the Husky.

Note: You will need ROS2 Humble

Step 2. There is a file where you can change the max velocity and angular velocity of the teleoperation [ Will update how to accesses this in a future update to readme]

Step 3. Open a new terminal and run the following commands in your Turtlebot3 workspace

```bash
source install/setup.bash 

export TURTLEBOT3_MODEL=waffle_pi 

export TURTLEBOT3_MODEL=burger 

ros2 run turtlebot3_teleop teleop_keyboard
```
## IV. Setup of Freespace Segmentation

Step 1. Drag and drop ```world.usd``` or your own custom enviroment into Isaac Sim with the husky (from Part II)

Step 2. Follow steps 1 -7 through "isaac_ros_bi3d_freespace quickstart guide": https://nvidia-isaac-ros.github.io/repositories_and_packages/isaac_ros_freespace_segmentation/isaac_ros_bi3d_freespace/index.html#quickstart . These steps will create all of the packages and onxx models needed in a docker container to run NVIDIA's Isaac ROS freespace segmentation.

Step 3. Run the following in a new terminal attached to the docker container

```bash
cd ${ISAAC_ROS_WS}/src/isaac_ros_common && \
./scripts/run_dev.sh
```
Note: After everytime we create a new terminal attached to the docker container, we must run the following:

```bash
cd /workspaces/isaac_ros-dev && \
  colcon build --symlink-install && \
  source install/setup.bash
```
This builds and source the workspace

Step 4. In the new terminal, run the following launch files to start the Isaac ROS BI3D Freespace segmetation:

```bash
ros2 launch isaac_ros_bi3d_freespace isaac_ros_bi3d_freespace_isaac_sim.launch.py \
featnet_engine_file_path:=/tmp/models/bi3d/bi3dnet_featnet.plan \
segnet_engine_file_path:=/tmp/models/bi3d/bi3dnet_segnet.plan \
max_disparity_values:=64
```

Note: The code defualts to the left side of the camera to change this to the right added:

``` bash
camera_frame:= front_stereo_camera:right_rgb \
```

Step 5. Open a thrid terminal attached to the docker container and run the following:

``` bash
ros2 run isaac_ros_bi3d isaac_ros_bi3d_visualizer.py --disparity_topic bi3d_mask
```
This visualize the disparity mask on the live camera feed.

Step 6 (optional). Open a fourth terminal attached to the docker container and run the following:

For Left Camerea View:
```bash
ros2 run image_view image_view --ros-args --remap /image:=/front_stereo_camera/left_rgb/image_resize
```
For Right Camera View:
```bash
ros2 run image_view image_view --ros-args --remap /image:=/front_stereo_camera/right_rgb/image_resize 
```
This allows us to see the live video feed that is being provided to the disparity mask.

### V. Code Demostration

<img src=https://github.com/jkcolem/Husky_with_Freespace_Segmentation/blob/main/2023-12-06%2000-34-19.gif width="500" height="500"/>

Note: This video includes optional Step 6 from Part IV

Note: You can change the mask color from GrayScale to a different colormap (more details in future update)

### VI. Link to YouTube Video of Outdoor Autonomy Oriented Digital Twin for Wheeled Mobile Robots

Link: https://youtu.be/FAJQwg3lfDY?si=H31s5fX3eKVDIj6b
