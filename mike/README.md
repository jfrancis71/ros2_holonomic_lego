# Mike (Mecanum Mike)

<img src=./images/final_assembly/step_5.jpg width=200>

## Summary

Mike (Mecanum Mike) is a demonstration tri-omniwheeled robot running ROS2 on a Raspberry Pi. He is built using Lego and EV3 motors with the Dexter Industries BrickPi3+ providing the hardware interface from the Raspberry Pi to the EV3 motors. The mike package demonstrates how to configure the following two packages together to create a working robot:

- ROS2 Control Mecanum Drive Controller (which is a recently released package available from ROS2 Jazzy)
- The brickpi3_motors package, available from (https://github.com/jfrancis71/ros2_brickpi3), listens to these motor velocity commands and instructs the BrickPi3 hardware to rotate the motors correspondingly.
(Disclosure: I am the author of the ROS2 BrickPi3 package).


## Lego Assembly

The Mecanum wheels are not a Lego product, my recollection is that I purchased them from: https://uk.robotshop.com/products/48mm-mecanum-omni-wheel-lego-4x. I imagine there would be many other suppliers; check it supports a Lego technic axle. The wheel has diameter 48mm.

<img src=./images/mecanum_wheel.jpg width=100>

[Lego Assembly Instructions](./lego_assembly/README.md)
