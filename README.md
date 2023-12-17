# Husky_with_Freespace_Segmentation
Reseach Project using Nvidia Isaac ROS Bi3D Freespace Segmentation

## 1. Installations

Step 1. Install or update your NVIDIA Drivers: https://ubuntu.com/server/docs/nvidia-drivers-installation .

Step 2. Install the Omniverse Laucher: https://www.nvidia.com/en-us/omniverse/download/ .
You will also have to create a Omniverse account during this step.

Step 3. Install Docker: https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository .

Step 4. Install the NVIDIA-container-toolkit: https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-apt .

Step 5. Install Isaac for workstation: https://docs.omniverse.nvidia.com/isaacsim/latest/installation/install_workstation.html .
If you are using a Linux system please follow the extra instructions that NVIDIA has provided.


Step 6. The comptue setup needed to use Isaac ROS: https://nvidia-isaac-ros.github.io/getting_started/hardware_setup/compute/index.html
## 2. Setup of Isaac Sim with Husky

```bash
export RMW_IMPLEMENTATION=rmw_fastrtps_cpp && LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$PWD/exts/omni.isaac.ros2_bridge/humble/lib
```

```bash
./isaac-sim.sh 
```

## 3. Setup of Freespace Segmentation
