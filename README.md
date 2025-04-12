# ros2_holonomic_lego

Currently in development, hopeful for release mid March 2025.

Demos of ROS2 enabled Lego EV3 holonomic robots (on a Raspberry Pi with BrickPi3 interface)

## <B>Guest Starring:</B>

|Name|As seen on YouTube|That's Me|
|------------------|----|----|
[Terry the tri omniwheeled robot](./terry/README.md)|<a href="https://www.youtube.com/watch?v=IS1v4hBFn8c"><img src="https://img.youtube.com/vi/IS1v4hBFn8c/0.jpg" height=320></a>|<img src=./terry/images/final_assembly/step_5.jpg height=320>|
[Harry the holonomic robot](./harry/README.md)|<a href="https://www.youtube.com/watch?v=g4C4JOayAP4"><img src="https://img.youtube.com/vi/g4C4JOayAP4/0.jpg" height=320></a>|<img src=./harry/images/final_assembly/step_3.jpg height=320>|


## Installation

All these robots are based on BrickPi3 hardware so: follow instructions to build ROS2 BrickPi3 at (https://github.com/jfrancis71/ros2_brickpi3)

If environment not activated (eg you have logged in again since performing above step), ensure environment is activated:

```mamba activate ros2```

In the above step I am recommending Robostack ROS2, but if you are using you're own ROS2 install, you should replace the 'mamba install ros-humble-generate-parameter-library' install in the below instructions with what is appropriate for your ROS2 install.

```
cd ~/ros2_ws
git -C src clone https://github.com/jfrancis71/ros2_holonomic_lego.git
```

If building either Terry or Harry:
```
git -C src clone -b jazzy https://github.com/jfrancis71/omni_wheel_controller
```

If building Mike:
```
mamba install ros-jazzy-mecanum-drive-controller
```

For all robots:
```
colcon build --symlink-install
```

### Troubleshooting

I recommend having a seperate shell running htop so you can monitor progress. The Raspberry Pi 3B+ is quite memory limited which can cause problems installing some packages, particularly the omni_wheel_controller in the last step above. If you see process status 'D' in htop relating to the install processes that persists this can indicate difficulties due to low memory. In this case I suggest before running the colcon build step (the final step in the instructions), adding:

```
export MAKEFLAGS="-j 1" # recommended to reduce memory usage.
```

Also I suggest adding some temporary swap (I found 2GB perfectly sufficient). See discussion from Digital Ocean in the References section. Don't forget to remove the swapafter a succesful installation. (A swap file on an SD card will reduce card life significantly)

## Reinitialize Environment

```
source ./install/setup.bash
```

## Verify Install

Activate the motor controller (replacing PACKAGE_NAME with your robot, eg harry, terry or mike):
```
ros2 launch PACKAGE_NAME brickpi3_motors_launch.py
```

Note this command blocks the terminal, so for subsequent commands you will need another terminal.

This should cause the motors to rotate (briefly):
```
ros2 topic pub -t 5 /cmd_vel geometry_msgs/msg/TwistStamped "{twist: {linear: {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.0}}}"
```

## Remote Control

To control by keyboard:
```
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
```

To control by joystick:

Note the Omniwheel controller interprets twist messages in metric, so need to scale joystick (otherwise the speed commands will be far to fast for the robot to be safe with). The below config file is based off the teleop_twist_joy/confix/xbox.config.yaml, but modified to scale linear.x. Below I have used the joystick config file from the terry package, but this joystick config file can be used for all the robots here.

```ros2 launch teleop_twist_joy teleop-launch.py config_filepath:=./src/ros2_holonomic_lego/terry/config/xeox.config.yaml publish_stamped_twist:=true```

If the Coolie Hat mode is enabled (mode button illuminated red), left/right straffing
 (direct sideways movement) is controlled by the right analog joystick.
These settings are specific to a XEOX SL6556 joystick, so you may well need to alter 
for your own joystick. You can echo the twist topic to verify that the teleop_twist_j
oy is generating appropriate twist messages for your joystick controls.

## References:

#### Third Party Packages:
- Omni wheel controller: https://github.com/hijimasa/omni_wheel_controller
- ROS2 BrickPi3: https://github.com/jfrancis71/ros2_brickpi3


A Simple Introduction to Omni Roller Robots:
http://modwg.co.uk/wp-content/uploads/2015/06/OmniRoller-Holonomic-Drive-Tutorial.pdf


Northwestern Robotics, Modern Robotics Chapter 13.2
https://www.youtube.com/watch?v=NcOT9hOsceE


Kinematics of Mobile Robots with Omni Directional Wheels:
https://www.youtube.com/watch?v=-wzl8XJopgg&t=1756s


Useful discussion on swap file on Ubuntu:
https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04


Omniwheels:
https://uk.robotshop.com/products/48mm-omniwheel-compatible-servos-lego-mindstorms-nxt


Dexter Industries BrickPi3:
https://www.dexterindustries.com/brickpi-core/


Book:
The Unofficial Lego Technic Builder's Guide, Pawet Sariel Kmiec


Robot Cheat sheet:
https://www.theroboticsspace.com/assets/article3/ros2_humble_cheat_sheet2.pdf
