# Simple Arm Description - ROS2 Package

A ROS2 package for a simple 2-joint robotic arm with visualization and simulation support.

## Package Structure

```
simple_arm_description/
├── CMakeLists.txt
├── package.xml
├── README.md
├── config/
│   └── controllers.yaml          # ROS2 control configuration
├── launch/
│   └── display.launch.py         # Launch file for RViz visualization
├── meshes/                        # (Empty - for 3D mesh files)
├── rviz/
│   └── simple_arm.rviz           # RViz configuration
└── urdf/
    ├── simple_arm.urdf           # Main robot description
    └── simple_arm_ros2_control.xacro  # ROS2 control configuration
```

## Robot Description

The simple arm consists of:
- **Base Link**: Fixed base (grey cylinder)
- **Joint 1**: Revolute joint rotating around Z-axis (±180°)
- **Link 1**: First arm segment (blue, 0.5m long)
- **Joint 2**: Revolute joint rotating around Y-axis (±115°)
- **Link 2**: Second arm segment (red, 0.4m long)
- **End Effector**: Green sphere at the tip

## Prerequisites

Make sure you have ROS2 Jazzy installed with the following packages:

```bash
sudo apt install ros-jazzy-robot-state-publisher
sudo apt install ros-jazzy-joint-state-publisher
sudo apt install ros-jazzy-joint-state-publisher-gui
sudo apt install ros-jazzy-xacro
sudo apt install ros-jazzy-rviz2
```

For Gazebo simulation (optional):
```bash
sudo apt install ros-jazzy-ros2-control
sudo apt install ros-jazzy-ros2-controllers
sudo apt install ros-jazzy-gz-ros2-control
sudo apt install ros-jazzy-joint-trajectory-controller
```

## Building the Package

1. Navigate to your ROS2 workspace:
```bash
cd ~/ros2_ws
```

2. Build the package:
```bash
colcon build --packages-select simple_arm_description
```

3. Source the workspace:
```bash
source install/setup.bash
```

## Usage

### Visualize in RViz

Launch the robot visualization with the joint state publisher GUI:

```bash
ros2 launch simple_arm_description display.launch.py
```

This will:
- Open RViz with the robot model
- Display a GUI window to control joint positions
- Show TF frames and the robot structure

You can move the sliders in the Joint State Publisher GUI to control the arm's joints.

### Launch Arguments

The launch file supports several arguments:

```bash
# Launch without GUI (no joint control sliders)
ros2 launch simple_arm_description display.launch.py use_gui:=false

# Launch without RViz
ros2 launch simple_arm_description display.launch.py use_rviz:=false

# Use simulation time
ros2 launch simple_arm_description display.launch.py use_sim_time:=true
```

### Check the URDF

Verify the URDF is valid:

```bash
check_urdf ~/ros2_ws/src/simple_arm_description/urdf/simple_arm.urdf
```

View the TF tree:
```bash
ros2 run rqt_tf_tree rqt_tf_tree
```

## ROS2 Topics

When running, the following topics are available:

- `/joint_states` - Current joint positions and velocities
- `/robot_description` - The robot URDF
- `/tf` - Transform tree

## Future Enhancements

To add Gazebo simulation support:
1. Include the `simple_arm_ros2_control.xacro` in your main URDF
2. Create a Gazebo launch file
3. Use the controllers defined in `config/controllers.yaml`

To control via code:
```bash
ros2 topic pub /arm_controller/joint_trajectory trajectory_msgs/msg/JointTrajectory ...
```

## License

BSD-3-Clause

## Author

ROS2 Simple Arm Package
