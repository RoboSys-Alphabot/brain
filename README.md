# Alphabot
---

Repository for **Alphabot**, an autonomous ground-rover whose goal is to dynamically maneuver and manipulate the environment.

Robotic Systems Integration, Olin College of Engineering, SP2017

## Connectivity Diagram

![connectivity](images/graph.png)

## Starting Up

1. Make sure that the battery is reasonably charged.

1. Make sure that **no bare wires are exposed**.

1. Flip the green switch at the top of the robot so that the - side is depressed. When doing this, it is recommended to have the e-stop pressed. -- TODO : add pictures

1. Release the e-stop if it is engaged.

1. Check the odroid monitor to see if the booting is in procedure.

1. Turn on the USB hub by pressing on the button on its side. -- TODO : check this statement

1. ssh into the robot:

	```bash
	ifconfig eth0
	nmap 10.42.0.0/24
	ssh odroid@10.42.0.79
	```
	
	Here, beware that eth0 may not be the name of your ethernet interface connected to the odroid. Also, the IP address 10.42.0.0 is assumed to have its first three bytes consistent with the subnet address found by the previous command (ifconfig). After the nmap returns 2 hosts up, ssh into the address that is not your own. The default password for the odroid is *odroid*.

1. [Optional] Run a screen session so that you don't have to ssh for every terminal:

	```bash
	screen
	```
	<C-a> c opens a new screen window.

	<C-a> a allows you to rename your current window.

	<C-a> " shows the list of screen windows that are currently open.

	<C-a> ' allows you to navigate to a known window name.

1. Launch roscore:

	```bash
	roscore
	```

1. Make sure that the android is connected to your ros master.

1. Bring the Robot UP:

	```bash
	roslaunch alpha_main bringup.launch
	```

## Standard Procedure for Mission

### SLAM

Don't copy an paste the below lines in the terminal; run them sequentially.

```bash
roslaunch alpha_localization mapping.launch
roslaunch alpha_navigation move_base.launch slam:=true
roslaunch alpha_navigation frontier_exploration.launch
rosrun alpha_main states.py
```


## Running the Simulation

To run the fully integrated simulation for finding the can and heading towards it:

1. Launch the Simulator:

	```bash
	roslaunch alpha_gazebo gazebo.launch
	```

1. Launch the Navigation Stack:

	```bash
	roslaunch alpha_navigation move_base.launch slam:=true
	```

1. [Optional] In case you'd like to manually control the robot, launch keyboard based teleop:

	```bash
	#rosrun teleop_twist_keyboard teleop_twist_keyboard.py
	```

1. [Optional] For better viewing, launch rviz:

	```bash
	roslaunch alpha_main rviz.launch
	```

1. Launch the exploration node:

	```bash
	roslaunch alpha_navigation frontier_exploration.launch
	```

1. Launch the Can-Detection Module:

	```bash
	rosrun alpha_sensors distance_to_target.py
	```

1. Launch the State Machine:

	```bash
	rosrun alpha_main states.py
	```

1. [Optional] In case you want to see the camera feed:

	```bash
	#rosrun image_view image_view image:=/alpha/image_raw
	```

1. [Optional] To view the state-machine:

	```bash
	#rosrun smach_viewer smach_viewer.py
	```
Remember to run each of these individually!
