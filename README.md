
# Jackal Robot Project Docker Setup Guide

## Introduction
This guide provides detailed instructions for setting up a Docker environment for the Jackal robot project, including RViz, Gazebo, and all necessary Jackal packages for ROS Noetic.

## 1. Dockerfile Setup
### Base Image with NVIDIA Support
```Dockerfile
FROM nvidia/cudagl:11.1.1-base-ubuntu20.04 as core
```
### Install ROS Noetic, Gazebo, and RViz
```Dockerfile
RUN apt-get update && apt-get install -y lsb-release gnupg2 && \
    sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list' && \
    apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 && \
    apt-get update && apt-get install -y ros-noetic-desktop-full
```

## 2. Install Jackal-Specific Packages
### Step 1: Include Jackal Packages
```Dockerfile
RUN apt-get update && apt-get install -y \
    ros-noetic-jackal-simulator \
    ros-noetic-jackal-desktop \
    ros-noetic-jackal-gazebo \
    ros-noetic-jackal-msgs \
    ros-noetic-jackal-navigation \
    ros-noetic-jackal-control \
    ros-noetic-jackal-description \
    ros-noetic-jackal-tutorials \
    ros-noetic-jackal-viz \
    ... # other dependencies
```
### Step 2: Environment Setup
```Dockerfile
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
```

## 3. Building and Running the Docker Image
### Build the Docker Image
```bash
docker build -t jackal_simulation .
```
### Run the Docker Container with GPU Support
```bash
docker run -it --gpus all --net=host --env="DISPLAY" --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" jackal_simulation
```

## 4. Testing the Installation
### Launch Gazebo with Jackal
```bash
roslaunch jackal_gazebo jackal_world.launch config:=front_laser
```

## 5. Using RViz
Launch RViz within the Docker container to visualize robot data and simulation environment.

---

This setup ensures a fully prepared environment with all the necessary tools and packages for the Jackal robot project, including Gazebo for simulation and RViz for visualization.
