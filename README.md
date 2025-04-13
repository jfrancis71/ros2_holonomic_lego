# ROS2 Holonomic Lego

Demos of ROS2 enabled Lego EV3 holonomic robots (on a Raspberry Pi with BrickPi3 interface)

This repository does not really contain any software as such (apart from configuration and launch files). It is primarily about demonstrating how to configure existing ROS2 control components to work together to create a working robot. It does contain examples of lego designs for corresponding robots.

## <B>Guest Starring:</B>

|Name|As seen on YouTube|That's Me|
|------------------|----|----|
[Terry the tri omniwheeled robot](./terry/README.md)|<a href="https://www.youtube.com/watch?v=IS1v4hBFn8c"><img src="https://img.youtube.com/vi/IS1v4hBFn8c/0.jpg" height=320></a>|<img src=./terry/images/final_assembly/step_3.jpg height=320>|
[Harry the holonomic robot](./harry/README.md)|<a href="https://www.youtube.com/watch?v=W5VVI1Zuzcs"><img src="https://img.youtube.com/vi/W5VVI1Zuzcs/0.jpg" height=320></a>|<img src=./harry/images/final_assembly/step_3.jpg height=320>|
[Mike (Mecanum Mike)](./mike/README.md)|<a href="https://www.youtube.com/watch?v=6CFftXhHokY"><img src="https://img.youtube.com/vi/6CFftXhHokY/0.jpg" height=320></a>|<img src=./mike/images/final_assembly/step_5.jpg height=320>|


### Tested Hardware

Raspberry Pi 3 Model B+, Dexter Industries BrickPi3

### Tested Software

Ubuntu 22.04, ROS2 Jazzy (RoboStack), BrickPi3


## Installation

All these robots are based on BrickPi3 hardware so: follow instructions to build ROS2 BrickPi3 at (https://github.com/jfrancis71/ros2_brickpi3)

If environment not activated (eg you have logged in again since performing above step), ensure environment is activated:

```mamba activate ros2```


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

Also I suggest adding some temporary swap (I found 2GB perfectly sufficient). See discussion from Digital Ocean in the References section. Don't forget to remove the swap after a succesful installation. (A swap file on an SD card will reduce card life significantly)


## Verify Install

To reinitialize environment:
```
source ./install/setup.bash
```


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
These settings are specific to a XEOX SL6556 joystick, so you may well need to alter for your own joystick. You can echo the twist topic to verify that the teleop_twist_joy is generating appropriate twist messages for your joystick controls.


## Discussion

All three robots work well. The holonomic aspect is super fun; however I expect they may struggle on rougher terrain. Holonomic Harry was probably the smoothest, however it is the one with the most complex design.

As the holonomic robots need more motors than their non holonomic counterparts, I think we would benefit from smaller motors and accept the lower power tradeoff (the Lego Technic EV3 motors supply plenty of drive force). However I decided to stick with EV3 motors due to convenient hardware and software support.


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


Mecanum Wheel kinematics:
https://ecam-eurobot.github.io/Tutorials/mechanical/mecanum.html


More on Mecanum kinematics:
https://invisibleart.pro/2023/08/30/mecanum-wheel/


Useful discussion on swap file on Ubuntu:
https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04


Omniwheels:
https://uk.robotshop.com/products/48mm-omniwheel-compatible-servos-lego-mindstorms-nxt


Dexter Industries BrickPi3:
https://www.dexterindustries.com/brickpi-core/


Book:
The Unofficial Lego Technic Builder's Guide, Pawet Sariel Kmiec


ROS2 Book: Robot Programming with ROS2, Francisco Martin Rico, 2023.


Robot Cheat sheet:
https://www.theroboticsspace.com/assets/article3/ros2_humble_cheat_sheet2.pdf
