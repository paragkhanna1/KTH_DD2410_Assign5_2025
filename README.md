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
3. **Remove** the old `irob_assignment_1` from your workspace **if needed** to free space or avoid conflicts. *(Do NOT delete it completely as it will be required in A-Level)*
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

---

## 🟩 E-Level Tasks

### 🎯 Objective

Implement a **basic autonomous robot** using:

* ROS2 Services for robot control
* A state machine for decision-making
* Integration into `project_launch.launch.py`

---

### ✅ Step-by-Step Instructions

#### 1. 🔍 Acquire Service Information

Understand and identify the services provided:

* **Activate Robot**
* **Deactivate Robot**
* **Get Goal**
* **Check if at Goal**

> Nodes for each of these services are already provided in the packages.

---

#### 2. 🧠 Launch File Integration

Edit `project_launch.launch.py`:

* Add the **Get Goal** service node:

  * Set correct `package`, `executable`, and include parameters from `goal_values_SM.yaml`.
  * ❌ **Do NOT use** `goal_values_BT.yaml` yet (for C-Level).
* Repeat the above for the following service nodes:

  * **Activate Robot**
  * **Deactivate Robot**
  * **Check if at Goal**

> Use `ros2 service list` and `ros2 service info` to verify everything is working.

---

#### 3. 👁️‍🗨️ Visualization Check (in RViz)

Test the service from another terminal:

```bash
ros2 service call <service_name> <service_type>
```

* A **purple marker** should appear in RViz.
* Call again to get a **new goal** marker.

---

#### 4. 🤖 Implement the State Machine

Create your own `SM_students.py`:

* This script must:

  1. **Activate** the robot
  2. **Get a Goal**
  3. **Move to Goal** (basic movement via `/cmd_vel` or similar)
  4. **Check if at Goal**

     * If goal is **inside an obstacle**, request a **new goal**
  5. Repeat Steps 1–4
  6. **Deactivate** robot when done

> 🔄 The robot **must stop moving** when deactivated, and only move if activated again.

---

#### ⚠️ Important Notes

* Ensure robot **does not skip goals**.
* All services must be correctly added to your launch file.
* Your code should **exit cleanly** after completing the mission.

---

✅ **E-Level Complete!**

---

## 🟨 C-Level Tasks

### 🎯 Objective

Add **sensor integration** and convert logic to a **Behavior Tree (BT)**.

---

### 🛠 Changes from E-Level

* Modify launch file to use:

  ```bash
  goal_values_BT.yaml
  ```

### 🧠 New Requirements

* Integrate **sensor data** (Odometry, LaserScan)
* Build a **BT** that:

  * Dynamically avoids obstacles
  * Handles robot **deactivation** during navigation
  * Reacts to **new goals** being sent mid-execution

---

### 🧪 Test Cases

From a separate terminal, try:

```bash
ros2 service call <deactivation_service>
ros2 service call <get_goal_service>
```

> Your robot **must respond** to these external calls without crashing or ignoring them.

---

### 🔍 Behavior Tree Debugging

* Draw your BT and **present it** during evaluation.
* Use a BT-ROS2 visualizer (like **BehaviorTree.CPP** GUI) if possible.

> **DO NOT hard-code any movement!**

---

✅ **C-Level Complete!**

---

## 🟥 A-Level Tasks

### 🎯 Objective

Create a **fully autonomous robot** using:

* Navigation stack
* Localization
* Exploration
* Robust BT behavior

---

### 📜 Behavior Tree Requirements

0. **Activate Robot**
1. **Explore the Environment**

   * Identify free vs. occupied space
   * Use exploration code from **Assignment 1**
2. **Localize the Robot**

   * Use AMCL if needed
3. **Get a Goal**
4. **Validate the Goal**

   * Skip goal if inside an obstacle
5. **Navigate to Goal**
6. **Check if Goal Reached**
7. **Deactivate Robot**

---

### 🧪 Evaluation Scenarios

Grumpy TA may:

* 🔁 **Deactivate robot** during mission
* 🎯 **Send new goal** while robot is en route
* 🕵️‍♂️ **Kidnap robot** (teleport it in Gazebo)

> Your code must handle these cases **robustly** using BT and localization.

---

### 🔁 Extra Hints

* Use:

  ```bash
  ros2 service call ...
  ```

  to test activation/deactivation dynamically.
* Switch to `waffle` robot model for RGB camera:

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
  * Is `/robot_active` set to `True`?

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



Let me know if you want me to generate a visual Behavior Tree or help with `SM_students.py` or BT XML files.
