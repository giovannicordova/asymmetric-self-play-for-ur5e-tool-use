# asymmetric-self-play-for-ur5e-tool-use
## Activating the Robot:

To begin using the robot, one must first ensure the controller is plugged in, and the connections to the controller are valid.

To turn on the UR5e, you hit the power button located on the front of the teach pendant next to the emergency stop button.

Once the system has turned on, you still must activate the robot by selecting the button located at the bottom left corner of the screen. 

Once clicked, you will be taken to this screen. Press the “ON” button to activate the robot.

After providing power to the robot, click the “START” button to unlock the robot’s joints.

After being unlocked, if everything went well, your screen should look as it does below.

![Robot Activated](https://i.imgur.com/NLVhjKk.png)

If your screen looks like the one above, hit the “Exit” button.

With the robot active, you can begin moving it easily using the “Move” tab. An example of what your screen should look like is below.

![Move Tab](https://i.imgur.com/77pyZKV.png)

## Activating the Gripper

With the robot on and activated, you can now activate the gripper supplied by RobotIQ.

Gripper Model: RobotIQ 2F-85

The gripper connects to the UR5e via a UR Cap, which is a software package installed on the teach pendant of the UR5e. To activate the gripper, first ensure that the gripper is connected to the UR5e via the provided cable. Then, navigate to the "Installation" tab on the teach pendant, and select the "Universal Robots" option. From there, select "Installation" again, and then choose "UR Caps". Finally, choose the "RobotIQ 2F-85" option to activate the gripper.

Product Website: https://robotiq.com/products/2f85-140-adaptive-robot-gripper?ref=nav_product_new_button 

## Activating the Wrist Camera

To activate the wrist camera, you first need to connect it to the UR5e controller. Once connected, you can access the camera through the "Installation" tab on the teach pendant. From there, select "Universal Robots", followed by "Installation", and then "UR Caps". Finally, choose the "Robotiq Camera URCap" option to activate the camera.

Product Website: https://robotiq.com/products/wrist-camera?ref=nav_product_new_button

## Programming the UR5e via Python

You will need either Ubuntu 18.04 with ROS melodic, or Ubuntu 20.04 with ROS noetic. We are all using ROS Noetic with Ubuntu 20.04 at our lab.

### Drivers & Prerequisites:

First, begin by installing the Universal Robots ROS Driver [here](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver). 

For the UR5e to function properly, you must install the External Control URcap, which has already been installed on our UR5e, but installation information can be found [here](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/install_urcap_e_series.md). 

You will also need to set up the tool communication with the UR5e if you would like to access the RobotIQ gripper. Information on how to install this URcap can be found [here](https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/setup_tool_communication.md), which has been installed on the robot already

Each UR robot is calibrated inside the factory giving exact forward and inverse kinematics. To also make use of this in ROS, you first have to extract the calibration information from the robot. Information on how to do so can be found in the Universal Robots ROS Driver in the first link. If we ever get another robot, it would be a good idea to follow the details on this page: https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_calibration/README.md 

Once you have the drivers setup, use this script to collect the calibration information from the robot. First, make sure the robot is turned on and connected to the network.

conda deactivate

$ roslaunch ur_calibration calibration_correction.launch
robot_ip:=192.168.1.17
target_filename:="${HOME}/my_robot_calibration.yaml”

Once you’ve done this, you should find a file named my_robot_calibration.yaml in your home directory. Just leave that there.

Next, open another terminal, and input the following to set-up a connection with the robot:

$ roslaunch ur_robot_driver ur5e_bringup.launch robot_ip:=192.168.1.17 kinematics_config:=${HOME}/my_robot_calibration.yaml

If you’re getting an error, it is likely the driver was installed incorrectly or you have not properly set-up your root file. To alter your root file, use:

gedit ~/.bashrc

This is what my root file looks like:
