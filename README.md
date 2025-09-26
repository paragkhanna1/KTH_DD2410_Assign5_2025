# KTH_DD2410_Assign5_2025 Assignment 5 for the DD2410 IROB course at KTH in ROS2 Foxy
contact: paragk@kth.se, paragkhanna1@gmail.com
(Leave a star if you liked this! A lot of effort went into this!)

---

# 🧠 ROS2 Autonomous Robot Navigation – Assignment 5

This project builds upon your solution from Assignment 1 to create an autonomous robot that navigates to goals using services and Behavior Trees. You'll begin with basic integration and progressively enhance your system toward full autonomy with exploration and recovery mechanisms.

---

## 📦 Getting Started

### Setup Instructions

1. Download and unzip the provided zip file **outside** your ROS workspace.
2. Move the two folders into your `src`:

   * `irob_interfaces`
   * `irob_assignment_5`
3. **Remove** the old `irob_assignment_1` from your workspace **if needed** to free space or avoid conflicts. *(Do NOT delete it or your other assignment-solutions completely as it/they will be required in A-Level)*
4. Build your workspace:

   ```bash
   colcon build --packages-select irob_interfaces irob_assignment_5
   source install/setup.bash
   ```

### Launching the Simulator

In each terminal, set the TurtleBot3 model:

```bash
export TURTLEBOT3_MODEL=burger
```

Then launch the simulation:

```bash
ros2 launch irob_assignment_5 simulator.launch.py
```

You will now see the arena in both RVIZ and Gazebo (like assignment 1)
---

## 🟩 E-Level Tasks

### 🎯 Objective
_Basic Integration, Goal Handling and State Machine based Navigation_

Implement a **basic autonomous robot** using:

* ROS2 Services for robot control
* A state machine for decision-making
* Nodes Integration into `project_launch.launch.py`

---

### ✅ Step-by-Step Instructions

#### 1. 🔍 Acquire Service Information

•	Your task:
•	Go through the files provided in the 2 packages.
•	Understand what services you have been given.
•	You are provided with services for: _Activating the robot, Deactivating the robot, Getting a Goal, verifying is the robot is at the goal_.

> Nodes for each of these services are already provided in the packages.

---

#### 2. 🧠 Launch File Integration

Edit `project_launch.launch.py`:

* Add the **Geting Goal** service node:

  * Set correct `package`, `executable`, and include parameters from `goal_params_SM.yaml`.
  * ❌ **Do NOT use** `goal_params_BT.yaml` yet (for C-Level).
l•	Check [ros2 service list and info] to get information about this service.

> Use `ros2 service list` and `ros2 service info` to verify everything is working.

---

#### 3. 👁️‍🗨️ Visualization Check (in RViz)

Test the service from another terminal:

```bash
ros2 service call <service_name> <service_type>
```
(Revise your ROS2 tutorials for correctly using above command)

* A **purple marker** should appear in RViz showing the current goal point.
* Search for an associated topic in ros2 topic list that provides information about this goal.
* Call again to get a **new goal** marker.

* Repeat the above 2,3 for the following service nodes:

  * **Activating Robot**
  * **Deactivating Robot**
  * **Check if at Goal**
> Use `ros2 service list` and `ros2 service info` to verify everything is working.

(Check ros2 service list for list of all running services now that you can use and verify that you have the 4 mentioned services available!)
---

#### 4. 🤖 Implement the State Machine

Edit your own `SM_students.py`:	Implement your state machine logic in SM_students.py. Here you will implement StateMachine with various service-clients and robot movement control commands.


* This script must:
  
  0. (Oh! I did not include these files correctly in the package, You need to include these files correctly like any new python script in a ros2 package, following all necessary steps.)

  1. **Activate** the robot
   
  	(Use the existing activation service, create a client).

(Hint: A topic is set True or False if robot is active or deactivated. You should subscribe to that topic)

The robot will not move if you don’t activate it at start OR if the service for deactivation is called, it will stop and don’t move unless activated. 

  2. **Get a Goal**

   (Client for the related service)

  3. **Move to Goal**

Drive your robot—no need for sensors yet.
( You can use sensors if you want!)
(Hint: Drive = ideally using a topic that you have used before!)

	Note: Robot must be activated to move. Check if proper service node is included in launch file

  
  4. **Check if at Goal**

(Use a service to determine if goal is reached) Check if proper service node is included in launch file

   If you goal is **inside an obstacle**, request a **new goal**

   
  5. Repeat Steps 1–4
   
	Until a reachable goal is found and confirmed as reached

  Ask for New goal, check if it is same as current goal, else move to New goal.
     
  6. **Deactivate** robot when done

•	Note: Robot must be de-activated at end, find a service. Check if proper service node is included in launch file
•	Robot should stop.
•	Your code should exit cleanly.


(It is your task to find relevant services for all above and include in the launch file.)
The robot should reach near all intermediate goals and should not skip them. In your evaluation, it will be seen that your solution works without skipping these goals and not just reached the final goal for E level.



> 🔄 The robot **stops moving** when deactivated, and only moves if activated again.!

---

#### ⚠️ Important Notes

* Ensure robot **does not skip goals**.
* All services must be correctly added to your launch file.
* Your code should **exit cleanly** after completing the mission.

---

✅ **E-Level Complete!**
You’ve built a basic autonomous robot using ROS2 and services. Great!

Congrats Padawan Young ROS-walker!
“A strong start, young one. You’ve taken your first step into a larger ROSverse. Stay curious, stay structured.”

---

## 🟨 C-Level Tasks

### 🎯 Objective
Behavior Tree with Sensor Integration

Add **sensor integration** and convert logic to a **Behavior Tree (BT)**.

---

### 🛠 Changes from E-Level

* Modify launch file to use for getting goal service:

  ```bash
  goal_params_BT.yaml
  ```

### 🧠 New Requirements

* Integrate **sensor data** 

Integrate robot sensors (e.g., Odometry, laser scan) into your logic to command the robot, avoid obstacles etc.

* Build a **BT** that:

  * Guide your robot through steps 1–6 (from E-Level). Dynamically avoids obstacles.
  * Handles robot **deactivation** during navigation
  * Reacts to **new goals** being sent mid-execution
  * The robot should now:
      •	Use real sensor input
      •	Avoid obstacles
      •	Make decisions dynamically—no hardcoded movements.

---

### 🧪 Test Cases

A mischevious TA can “deactivate” the robot wile your code is running for the previous goal, make sure the BT handles that correctly.

(Hint: Call the deactivation service from terminal to check if the BT works fine)
(Hint: A topic is set True or False if robot is active or deactivated. You should subscribe to that topic)

Another check is what if a new goal comes before you have reached the current goal?
A gumpy TA can use the related service to get a new goal while your code is running for the previous goal, make sure your code handles that correctly.


From a separate terminal, try:

```bash
ros2 service call <deactivation_service>
ros2 service call <get_goal_service>
```

> Your robot **must respond** to these external calls without crashing or ignoring them.

---

### 🔍 Behavior Tree Debugging

* Required: Draw your BT and **present it** and present it to the TA during evaluation.
* Use a BT-ROS2 visualizer (like pytrees) if possible.

> **DO NOT hard-code any movement!** 
 The goal_values_BT.yaml can be changed by a TA for evaluation to ensure robustness of your BT.

---

✅ **C-Level Complete!**

Congrats! Knight Obi ROS-Kenobi
“You are now a Knight of the ROS Order. Your logic is strong, your callbacks cleaner. Trust in your tree, and let the sensors guide you!"
---

## 🟥 A-Level Tasks

### 🎯 Objective
Full Autonomy with Behavior Tree Exploration

Create a **fully autonomous robot** using:

* Navigation stack
* Localization
* Exploration
* Robust BT behavior

---
Change the params file in the launch file: "goal_params_BT_level_A.yaml"

### 📜 Behavior Tree Logic for A-level

0. **Activate Robot**
1. **Explore the Environment**
 (Refer to your work from Assignment 1)
   * Identify free vs. obstacles
   * Use exploration code from **Assignment 1**
2. **Localize the Robot**

   * Robot should know where it is in the arena. (Hint: you are using nav stack., can you use amcl?)

3. **Get a Goal**
4. **Validate the Goal**

   * Check if it is in an obstacle, now you do not need to go to the Goal. If so, get a new one

5. **Navigate to Goal**
6. **Check if Goal Reached** •	Call service to verify
7. **Deactivate Robot**

---


### 🧪 Evaluation Scenarios

*Ideally, the robot should start in a different position than the default, in the evaluation, the TA might ask you to move the robot to a different position by using the translation in Gazebo. You can also do this once you are confident about your implementation.
The grumpy TA would try all methods to try and fail your code AGAIN! 
The BT should handle deactivation, new goal while current goal is being processed etc.

(If your code handles all above, this might make the TA grumpier, leading to a last try to fail you):
The TA kidnaps your robot! (move it in Gazebo). Do you think your method would still work? 
Do you think you really need localization or re-localization? How to achieve this?

(Hint: If you need a robot with RGB camera, export waffle instead of burger.)

Grumpy TA may:

* 🔁 **Deactivate robot** during mission
* 🎯 **Send new goal** while robot is en route to current goal.
* 🕵️‍♂️ **Kidnap robot** (teleport it in Gazebo)

> Your code must handle these cases **robustly** using BT and localization.

---

### 🔁 Extra Hints

* Use:

  ```bash
  ros2 service call ...
  ```

  to test activation/deactivation dynamically.
* Switch to `waffle` robot model for RGB camera and other sensors if needed:

  ```bash
  export TURTLEBOT3_MODEL=waffle
  ```

---

## 🛠️ Troubleshooting

### 🌀 Robot Moves Strangely or Map is Distorted?

* You likely hit a wall or sent too high velocity.
* Restart the simulation.

### 🚫 Robot Not Moving?

* Check:

  * Is it **stuck in a wall**?
  * Is it **activated**?
  * Is a `robot state topic` set to `True`?

---
Level A complete!

Congrats! Master Yo-ROS-da
“Mastered the ROS force, you have. But beware... tempted by the Dark Side of shortcuts, you must not be. That way leads to debugging, segfaults... and late-night despair.” 
😈 “But maybe... you are already on the Dark Side.”
— Darth ROS-lord, Wielder of the Segfaults
---

## ✅ Summary of Completion Levels

| Level | Objective                   | Technologies          |
| ----- | --------------------------- | --------------------- |
| **E** | State Machine with Services | ROS2 Services, SM     |
| **C** | Behavior Tree with Sensors  | Sensors + BT          |
| **A** | Full Autonomy               | BT + AMCL + Nav Stack |

---

## 🧭 Useful Commands

```bash
ros2 launch irob_assignment_5 simulator.launch.py
ros2 service list
ros2 service info <service_name>
ros2 service call <service_name> <service_type>
```

---

## 💡 Pro Tips

* Keep a backup of Assignment 1.
* Use `rqt_graph` or `rqt` tools to visualize your system.
* Follow best practices for node creation and parameter loading.

