# KTH_DD2410_Assign5_2025
Assignment 5 for the DD2410 IROB course at KTH in ROS2 Foxy



ROS2 Assignment 5: Goal-Oriented Robot Navigation
Getting Started
This assignment is built on the first assignment. So, follow the basic setup instructions. 
(You don’t need navigation launch yet)
Download the zip file, unzip it outside your ros-workspace. Place the 2 folders “irob_interfaces” and “irob_assingment_5” in your src. Build your workspace. 
(You can/should remove old “irob_assingment_1” from the workspace if you need to free space or to avoid conflicts. Don’t delete it however as you might need the assignment 1 solution in A level)
To launch the simulator, run:
(export turtlebot as burger in each terminal)
ros2 launch irob_assignment_5 simulator.launch.py

E-Level Tasks
Objective: Basic Integration, Goal Handling and State Machine based Navigation
Step-by-step Instructions
1. Acquire Services Information
•	Your task:
•	Go through the files provided in the 2 packages.
•	Understand what services you have been given.
•	You are provided with services for: Activating the robot, Deactivating the robot, Getting a Goal, verifying is the robot is at the goal.
•	Associated service nodes are also provided.

2. Launch File Integration for Services
Edit your project_launch.launch.py file.
•	Start with Getting a Goal service, Include the related node and launch it.
•	Make sure to:
•	Set correct package and executable names.
•	Load the parameter file containing goal_values_SM.yaml. This node needs this file! 
(Don’t load the file goal_values_BT.yaml, that is for C level)
•	Check [ros2 service list and info] to get information about this service.
Visualization Check in RViz
•	After the service server is running, test it in another terminal:
ros2 service call <service_name> <service_type>

•	A purple marker should appear in RViz, indicating the goal point.
•	Run the command again to get a different goal:
ros2 service call <service_name> <service_type>


3. Similarly, add the related nodes for other services in the launch file.
(Check ros2 service list for list of all running services now that you can use and verify that you have the 4 mentioned services available!)

4. Create the State Machine Node
•	Implement your state machine logic in SM_students.py. Here you will implement StateMachine with various service-clients and robot movement control commands.
The state machine should perform the following:
0.	Oh! I did not include these files correctly in the package, You need to include these files correctly as with any new python in a ros2 package.
1.	Activate the Robot
•	(Use the existing activation service, create a client).
(Hint: A topic is set True or False if robot is active or deactivated. You should subscribe to that topic)
The robot will not move if you don’t activate it at start OR if the service for deactivation is called, it will stop and don’t move unless activated. 
2.	Get a Goal
•	(Client for the related service) 
3.	Move to the Goal
•	Drive your robot—no need for sensors yet.
( You can use sensors if you want!)
(Hint; Drive = ideally using a topic that you have used before!)
•	Note: Robot must be activated to move. Check if proper service node is included in launch file
4.	Check if at Goal
•	(Use a service to determine if goal is reached) Check if proper service node is included in launch file
•	If the goal is inside an obstacle → request a new goal.
(Check ros2 srv list for list of all running services now that you can use!)
5.	Repeat Steps 1–4
•	Until a reachable goal is found and confirmed as reached
6.	Deactivate the Robot
•	Note: Robot must be de-activated at end, find a service. Check if proper service node is included in launch file
•	Robot should stop.
•	Your code should exit cleanly.

(It is your task to find relevant services for all above and include in the launch file.)
The robot should reach near all intermediate goals and should not skip them. In your evaluation, it will be seen that your solution works without skipping these goals and not just reached the final goal for E level.
✅ E-Level Complete!
You’ve built a basic autonomous robot using ROS2 and services. Great foundation work!

C-Level Tasks
Objective: Behavior Tree with Sensor Integration
Changes Needed:
•	Modify the launch file to use goal_values_BT.yaml instead of the previous parameter file.
New Requirements:
•	Integrate robot sensors (e.g., Odometry, laser scan) into your logic to command the robot, avoid obstacles etc.
•	Use a Behavior Tree (BT) to guide your robot through steps 1–6 (from E-Level).
•	The robot should now:
•	Use real sensor input
•	Avoid obstacles
•	Make decisions dynamically—no hardcoded movements.
A mischevious TA can “deactivate” the robot wile your code is running for the previous goal, make sure the BT handles that correctly.

(Hint: Call the deactivation service from terminal to check if the BT works fine)
Use: ros2 service call  .. deactivation service..
(Hint: A topic is set True or False if robot is active or deactivated. You should subscribe to that topic)
Another check is what if a new goal comes before you have reached the current goal?
A gumpy TA can use the related service to get a new goal while your code is running for the previous goal, make sure your code handles that correctly.

(Hint: Call the service for getting a goal from terminal to check if the BT works fine).

Use: ros2 service call {getting goal service}
Required: Draw your BT and present it to the TA in evaluation.
(Ideal: if you use a dedicated BT-ROS2 visualizer! This can help you in debugging and presentation)

(REMEMBER! NO hard coded movements, The goal_values_BT.yaml can be changed by a TA for evaluation to ensure robustness of your BT)
 C-Level Complete!


A-Level  Tasks
Objective: Full Autonomy with Behavior Tree Exploration

In this level, you should/can use your solutions from assignment 1. 
Start the navigation server (launch the file after modifying paths as per assignment 1 instructions).

In addition to the tasks from C level, this level has an update in the autonomous navigation:

Behavior Tree Logic for A-Level
0.	Activate the Robot
1.	Explore the Environment
•	(Refer to your work from Assignment 1)
•	Identify free space vs obstacles.
Can you Localize the Robot now?
•	Robot should know where it is in the arena.
(Hint: you are using nav stack., can you use amcl?)
2.	Get a Goal
3.	Validate the Goal
•	Check if it is in an obstacle, now you do not need to go to the Goal.
•	If so, get a new one
4.	Navigate to the Goal
5.	Check if Goal is Reached
•	Call service to verify
6.	Deactivate the Robot
A-Level Complete!
*(Ideally, the robot should start in a different position than the default, in the evaluation, the TA might ask you to move the robot to a different position by using the translation in Gazebo. You can also do this once you are confident about your implementation.) 

The grumpy TA would try all methods to try and fail your code AGAIN! 
The BT should handle deactivation, new goal while current goal is being processed etc.

(If your code handles all above, this might make the TA grumpier, leading to a last try to fail you): The TA kidnaps your robot! (move it in Gazebo). Do you think your method would still work? 
Do you think you really need localization or re-localization? How to achieve this?

(Hint: If you need a robot with RGB camera, export waffle instead of burger.)
