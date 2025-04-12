# Harry (Holonomic Harry)

<img src=./images/final_assembly/step_3.jpg width=200>

## Summary

Harry is a demonstration four omniwheeled robot running ROS2 on a Raspberry Pi. He is built using Lego and EV3 motors with the Dexter Industries BrickPi3+ providing the hardware interface from the Raspberry Pi to the EV3 motors. The harry package demonstrates how to configure the following two packages together to create a working robot:

- A ROS2 omniwheel controller, available from (https://github.com/hijimasa/omni_wheel_controller), which listens to ROS2 twist messages and, using a configuration file specifying robot geometry, issues ROS2 controller motor velocity commands.

- The brickpi3_motors package, available from (https://github.com/jfrancis71/ros2_brickpi3), listens to these motor velocity commands and instructs the BrickPi3 hardware to rotate the motors correspondingly.
(Disclosure: I am the author of the ROS2 BrickPi3 package).


## Lego Assembly

The Omniwheels are not a Lego product, my recollection is that I purchased them from: https://uk.robotshop.com/products/48mm-omniwheel-compatible-servos-lego-mindstorms-nxt. I imagine there would be many other suppliers; check it supports a Lego technic axle. The wheel has diameter 48mm.

<img src=../terry/images/omni_wheel.jpg width=100>

[Lego Assembly Instructions](./lego_assembly/README.md)

