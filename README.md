# asymmetric-self-play-for-ur5e-tool-use
## UR5e Operation Information

### Activate the Robot:

To begin using the robot, one must first ensure the controller is plugged in, and the connections to the controller are valid.

To turn on the UR5e, you hit the power button located on the front of the teach pendant next to the emergency stop button.

![Image](https://user-images.githubusercontent.com/112212389/237012193-939fdd81-f112-470f-8c1f-38d38bcab9bc.jpeg)

Once the system has turned on, you still must activate the robot by selecting the button located at the bottom left corner of the screen. 

Once clicked, you will be taken to this screen.

Press the “ON” button to activate the robot.

After providing power to the robot, click the “START” button to unlock the robot’s joints.

![startup-ur5e](https://user-images.githubusercontent.com/112212389/237008486-84df1ae5-3686-42e9-af9b-09d46aaf64fe.png)

After being unlocked, if everything went well, your screen should look as it does below.

![startup-ur5e-2](https://user-images.githubusercontent.com/112212389/237008586-0e02f395-eae2-427d-bc8c-ffe172e39c77.png)

If your screen looks like the one above, hit the “Exit” button.

With the robot active, you can begin moving it easily using the “Move” tab. An example of what your screen should look like is below.

![move-tab](https://user-images.githubusercontent.com/112212389/237008677-bdbb41fe-6d1c-497f-afaa-edfdb8ab85ed.png)

### Activating the Gripper

With the robot on and activated, you can now activate the gripper supplied by RobotIQ.

Gripper Model: RobotIQ 2F-85
Product Website: https://robotiq.com/products/2f85-140-adaptive-robot-gripper?ref=nav_product_new_button 

![gripperspecs](https://user-images.githubusercontent.com/112212389/237008725-6a023ee3-17fa-42d8-a654-f83bc5f7799e.png)

The gripper connects to the UR5e via a UR Cap, which is a software package installed on the teach pendant of the UR5e. To activate the gripper, first ensure that the gripper is connected to the UR5e via the provided cable. Then, navigate to the "Installation" tab on the teach pendant, and select the "Universal Robots" option. From there, select "Installation" again, and then choose "UR Caps". Finally, choose the "RobotIQ 2F-85" option to activate the gripper.

![URCAP](https://user-images.githubusercontent.com/112212389/237008807-d9e24552-2938-45d8-91d4-7f51c01aeebc.png)

### Activating the Wrist Camera

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

```
conda deactivate

$ roslaunch ur_calibration calibration_correction.launch
robot_ip:=192.168.1.17
target_filename:="${HOME}/my_robot_calibration.yaml”
```

Once you’ve done this, you should find a file named my_robot_calibration.yaml in your home directory. Just leave that there.

Next, open another terminal, and input the following to set-up a connection with the robot:

```
$ roslaunch ur_robot_driver ur5e_bringup.launch robot_ip:=192.168.1.17 kinematics_config:=${HOME}/my_robot_calibration.yaml
```

If you’re getting an error, it is likely the driver was installed incorrectly or you have not properly set-up your root file. To alter your root file, use:

```
gedit ~/.bashrc
```

This is what my root file looks like:

![bashrc](https://user-images.githubusercontent.com/112212389/237008889-c43f836b-1b3b-4640-b2fd-b13bf9283406.png)

### Using RVIZ and MoveIt!

This website has great information on how to get rviz & MoveIt! up and running: https://github.com/UniversalRobots/Universal_Robots_ROS_Driver/blob/master/ur_robot_driver/doc/usage_example.md 

I was able to connect the UR5e to my computer using the instructions followed in their tutorial, but I was having issues once I started trying to implement MoveIt!. It is likely I need to look at my current installation and make adjustments.

The UR External Control must be playing for the robot to be able to accept commands. You should be able to connect to the robot and see its position in rviz, but when you try to move the robot using the:

```
rosrun ur_robot_driver test_move
```

You’ll run into issues. If you are having issues with getting the external control to run, be sure that the external control has the Host IP setting as your own computer’s IP.

To find your own IP address, go to Setting > Network > Ethernet > Settings. Use the IPv4 Address in External Control, and then run the program.


https://user-images.githubusercontent.com/112212389/237010498-ea772614-535c-492f-a0ca-d5ede49789a1.mp4


## Future Work

Look into this GitHub repository to look at set-up
https://github.com/o2ac/o2ac-ur

Set-up a real-time kernal for UR5e.

Create a real-world simulation environment.
