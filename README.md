# installROSFoxyOnUbuntu1804

## Version
Ubuntu 18.04.5
ROS 2 Foxy Fitzroy (Originally bind with Ubuntu 20.04)

## Installation Guide

* __Install ROS 1 Melodic before install ROS 2__.

* Install ROS 1 Melodic by following official ROS 1 [Melodic installation guide](https://wiki.ros.org/melodic/Installation/Ubuntu).

* Follow the first few steps from [Official Installation](https://docs.ros.org/en/foxy/Installation/Ubuntu-Development-Setup.html).

    * Set locale
    * Add the ROS 2 apt repository
    * Install development tools and ROS tools
    * Get ROS 2 code
        ```
        # Note: vcs may fail, reinstall python3-vcstool
        sudo apt update && sudo apt install python3-vcstool 
        ```
    * Install dependencies using rosdep
    * Pause here-> Build the code in the workspace

* If start to colcon build, ros1_bridge will fail due to missing dependencies
    ```
        cd ~/ros2_foxy/
        colcon build --symlink-install --packages-skip ros1_bridge
    ```
    This may take a while about 20 mins...

* Next you need to source the ROS 1 environment
    ros1_bridge will be built with support for any message/service packages that are on your path and have an associated mapping between ROS 1 and ROS 2. Therefore you must add any ROS 1 or ROS 2 workspaces that have message/service packages that you want to be bridged to your path before building the bridge.
    ```
        # source ROS 2
        . ~/ros2_foxy/install/local_setup.bash
        # ignore the warning of ros1_bridge for now
        # source ROS 1 melodic
        . /opt/ros/melodic/setup.bash
    ```

* Then build just the ROS 1 bridge:
    ```
        colcon build --symlink-install --packages-select ros1_bridge --cmake-force-configure
    ```

## Sanity Check

Install terminator to have multi-terminal in one screen
```
    sudo apt install terminator
```
You will need four terminals to do this quick test

1. Terminal 1:
    ```
    # Start ROS 1 roscore
    . /opt/ros/melodic/setup.bash
    roscore
    ```

1. Terminal 2:
    ```
    # Source both (ROS 1 + ROS 2):
    # Source ROS 1 first:
    . /opt/ros/melodic/setup.bash
    # Source ROS 2 next:
    . ~/ros2_foxy/install/local_setup.bash

    # Start bridge
    export ROS_MASTER_URI=http://localhost:11311
    ros2 run ros1_bridge dynamic_bridge
    ```

1. Terminal 3:
    ```
    # Start a ros 1 python talker:
    . /opt/ros/melodic/setup.bash
    rosrun rospy_tutorials talker
    ```

1. Terminal 4:
    ```
    # Start a ros 2 cpp listener:
    . ~/ros2_foxy/install/local_setup.bash
    ros2 run demo_nodes_cpp listener
    ```

## TODO

- [ ] Check if it works with Jetson NANO JetPack 4.5.1
- [ ] Check if it works with Jetson Xavier NX JetPack 4.5.1
