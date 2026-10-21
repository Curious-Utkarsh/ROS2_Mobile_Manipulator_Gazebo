# Aura — Autonomous Mobile Manipulator (ROS 2, Gazebo)

[![ROS 2 Humble](https://img.shields.io/badge/ROS%202-Humble-22314E?logo=ros)](https://docs.ros.org/en/humble/)
[![Nav2](https://img.shields.io/badge/Nav2-navigation-1F6FEB)](https://docs.nav2.org/)
[![MoveIt 2](https://img.shields.io/badge/MoveIt-2-1F6FEB)](https://moveit.picknik.ai/humble/)
[![Simulation](https://img.shields.io/badge/runs%20in-Gazebo-F58113)](#demo)

Aura is a differential-drive robot with a 5-DOF arm and a two-finger gripper that I modelled in Fusion 360 and built up in Gazebo. It explores and maps an unknown house on its own, navigates with Nav2, finds coloured targets with its camera, manipulates them with MoveIt 2, and returns to an ArUco-marked dock. It started as an autonomous floor-cleaning robot concept and was my first project combining navigation and manipulation in one system.

## Demo

<table>
<tr>
<td align="center"><a href="https://youtu.be/NHeu9y64Ovw"><img src="https://img.youtube.com/vi/NHeu9y64Ovw/hqdefault.jpg" width="320" alt="Aura: self driving with robotic arm manipulation"></a><br><sub>Navigation + arm manipulation</sub></td>
<td align="center"><a href="https://youtu.be/6KSx5C4jXmc"><img src="https://img.youtube.com/vi/6KSx5C4jXmc/hqdefault.jpg" width="320" alt="Aura joystick teleoperation"></a><br><sub>Joystick teleoperation</sub></td>
</tr>
</table>

Playlist: [ROS 2 Projects](https://www.youtube.com/playlist?list=PLVjFk3hP8Wm4)

Build log on LinkedIn (Jan–Feb 2025): [meet Aura](https://www.linkedin.com/feed/update/urn:li:activity:7280264641157414912/) · [URDF from Fusion 360](https://www.linkedin.com/feed/update/urn:li:activity:7280832984570572801/) · [Gazebo simulation](https://www.linkedin.com/feed/update/urn:li:activity:7282055581220196352/) · [autonomous mapping](https://www.linkedin.com/feed/update/urn:li:activity:7282472150022692864/) · [MoveIt on the arm](https://www.linkedin.com/feed/update/urn:li:activity:7285260305759182848/) · [full build](https://www.linkedin.com/feed/update/urn:li:activity:7286963156571013120/) · [Alexa voice control](https://www.linkedin.com/feed/update/urn:li:activity:7297102489537982464/)

## Key features

- **Autonomous exploration** — frontier detection on the occupancy grid, A\* paths smoothed with B-splines and followed with pure pursuit, while slam_toolbox builds the map.
- **Localization and navigation** — AMCL on the saved map, Nav2 goals and waypoint following.
- **Manipulation** — 5-DOF arm + gripper configured in MoveIt 2; a C++ action server (`AuraTask`) executes named arm/gripper motions on request.
- **Vision** — HSV colour detection of red and blue targets drives the task sequence; ArUco detection homes the robot back onto its dock.
- **Task orchestration** — a Python node combines Nav2, the arm action server, LiDAR, IMU and camera into one mission.
- **Teleoperation** — joystick through `twist_mux`, plus a slider tool for the arm.

## Software stack

ROS 2 Humble · Nav2 · slam_toolbox · AMCL · MoveIt 2 (C++ MoveGroupInterface) · ros2_control · Gazebo Fortress · OpenCV (HSV, ArUco) · Fusion 360 · Python · C++

## Packages

`aura_description` URDF (base + arm), worlds · `aura_controller` diff-drive + arm controllers, joystick · `aura_mapping` slam_toolbox + frontier exploration · `aura_localization` AMCL · `aura_navigation` Nav2, waypoints · `aura_moveit` MoveIt config + C++ task server · `aura_msgs` `AuraTask` action · `aura_vision` ArUco homing · `aura_script` mission node · `aura_bringup` simulation launch

## Run

```bash
mkdir -p ~/aura_ws && cd ~/aura_ws
git clone https://github.com/Curious-Utkarsh/aura-mobile-manipulator.git .
rosdep install --from-paths src --ignore-src -r -y
colcon build --symlink-install && source install/setup.bash

ros2 launch aura_bringup simulation.launch.py      # Gazebo + AMCL + Nav2 + MoveIt + task server
ros2 run aura_script task                           # run the mission
```

## Related

- [Mobi](https://github.com/Curious-Utkarsh/mobi-service-robot) — the real-hardware service robot that followed Aura
- [MechaBot](https://github.com/MechaMind-Labs/AMR-Mastery) — docking taken to real hardware
- [TurtleBot3 ArUco docking](https://github.com/Curious-Utkarsh/turtlebot3-aruco-docking) — the docking logic Aura builds on
- [SCARA conveyor cell](https://github.com/Curious-Utkarsh/scara-pick-and-place) — manipulation on real hardware

## Credits

The frontier-exploration node is adapted from an open-source ROS 2 frontier-based exploration project (comments in the original's language are kept). The MoveIt task-server pattern follows Antonio Brandi's *Robotics and ROS 2 — Manipulators* course. House and warehouse worlds use AWS RoboMaker models.

---
[Kumar Utkarsh](https://github.com/Curious-Utkarsh) · [LinkedIn](https://www.linkedin.com/in/kumar-utkarsh-124aab227/) · [MechaMind Labs](https://www.mechamindlabs.com/)
