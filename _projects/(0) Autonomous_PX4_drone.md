---
name: Autonomous Drone built from scratch
tools: [ROS2, C++, Path Planning, Gazebo, SITL, PX4, Kinematics, Computer vision, YOLOv8]
image: https://marnonel6.github.io/assets/Drone.JPG
description: Developed an autonomous drone control and path planning package with ROS2(C++) for off-board (RPi) control of a Pixhawk over uXRCE-DDS (PX4). The drone is custom built from scratch.

---

# Autonomous Drone built from scratch

<br>
### Brief overview
<br>
Embarking on a mission to reshape the landscape of agriculture, I spearheaded the development of an UAV system, leveraging cutting-edge technologies to address the unique challenges faced by the livestock industry. Rooted in a commitment to innovation, this project represents a groundbreaking solution poised to redefine precision livestock farming. At the core of this endeavor is a seamless integration of ROS2, C++ and Python for autonomous missions. The Pixhawk 6X flight computer, coupled with PX4 for precise position control, forms the backbone of the system. Complemented by a Raspberry Pi, the drone's intelligence is harnessed to communicate with the Pixhawk via uXRCE-DDS, translating 3D world waypoints into dynamic, real-world actions. Fields2cover further enhances operational efficiency by optimizing field coverage path planning.

But this project isn't just about technological prowess; it's a testament to addressing a critical need in the agriculture sector. Recognizing the substantial economic impact of livestock loss on farms, a custom YOLOv8 model was meticulously trained for livestock detection and inventory management. This added layer of intelligence ensures not just efficiency but also a proactive approach to safeguarding the well-being of farm animals. More than a drone, this autonomous system stands as a cost-saving marvel for farms. Operating seamlessly from take-off to mission execution and landing, it dramatically reduces the need for human intervention. The autonomous charging feature ensures continuous operation, providing farms with an unparalleled level of vigilance over their livestock and fields.

This project was born out of a vision for a more technologically advanced and sustainable agriculture sector. The backstory underscores the urgency for innovation in livestock management, positioning this autonomous drone system as a beacon of progress in an industry thirsty for technological advancements. Farms embracing this technology aren't just adopting a solution; they are ushering in a new era of precision farming. The autonomous drone's ability to patrol fields, monitor livestock, and respond swiftly to anomalies makes it an indispensable tool for modern agriculture.

### Main componets
- T-Motor motors, ESCs and propellers
- Pixhawk 6X Flight computer (PX4 v1.14)
- Raspberry Pi 4B (Ubuntu 22.04, ROS2 HUMBLE)
- RTK + Base station
- Tarot 680 Pro Sport frame

![DSC03642](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/582d1994-ba0f-4c92-823f-b5a3b7a017d8)

### Overview video
<iframe width="960" height="640" src="https://www.youtube.com/embed/uN1cQExSy8g?si=MPC8V23dq6EN5UoV" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### High level program diagram
![AgTech Drone](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/9ab9a8d6-d4f8-4776-93f2-e3e19734c19d)

#### Path Planning ([Fields2Cover](https://fields2cover.github.io/))
The path planning node is responsible for planning the mission path for the UAV. The node leverages the Fields2Cover package for efficient field coverage path planning. The significance of this node lies in its ability to calculate an optimal path, considering the working width of the UAV and its minimum turning radius. The Fields2Cover package serves as the backbone of the path planning strategy. A pull request was opened and merged to the package to add the ability to [discretize the swaths of the planned path](https://github.com/Fields2Cover/Fields2Cover/pull/93). This addition empowers users with finer control over the waypoints, optimizing the precision and maneuverability of the UAV during its mission execution. The node intelligently processes input field data, accounting for UAV specifications, and generates a visually intuitive output path displayed below. The resulting path, depicted in green and the input field depicted in blue. The shading gradient, from light to dark green, delineates the start to end sides of the path, offering a clear visual representation of the coverage sequence.

To integrate this path into the overall UAV mission, the node converts the Fields2Cover output path into a standardized NAV2 messages path. This conversion is displayed in the high-level flow diagram presented in the previous section. From the UAV's home position, the node orchestrates the planning of a comprehensive mission path. This involves charting a course from the home position to the start of the Fields2Cover path and then retracing the path back to the home position. The fully assembled mission path is then published to the /f2c_path topic, ensuring that the UAV executes its mission with precision.

Beyond path planning, the node also plays a pivotal role in ensuring accurate spatial awareness. It performs the crucial task of converting UAV odometry from PX4 coordinates to ROS2 coordinates. This transformed data is then published to the /vehicle_pose topic, providing real-time information about the UAV's odometry within the ROS2 ecosystem.

![Screenshot from 2023-11-10 12-01-03](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/9e8270a2-01b6-4322-a1e8-34714fdf9920)

#### UAV Control (State Machine)
In the heart of the UAV control system lies a state machine (As seen in the High level program diagram section), ensuring seamless transitions between distinct operational states. This critical component, encapsulated in the timer_callback function, governs the UAV's behavior, ensuring optimal performance and responsiveness throughout its mission.

1. PREFLIGHT State:

    Objective: Conduct preflight checklist.
   
    Conditions: Waits for vehicle odometry and verifies the drone's startup position.
   
    Actions: Sets take-off hover waypoint, transitions to IDLE state on successful startup position validation and safety checks.

3. IDLE State:

    Objective: Arm the drone, set offboard mode, and initialize take-off.
   
    Conditions: Publishes offboard control mode, take-off hover waypoint, and vehicle command.
   
    Actions: Transitions to OFFBOARD state upon reaching offboard mode and armed state.

4. OFFBOARD State:

    Objective: Execute take-off, hover, and prepare for mission.
   
    Conditions: Continuously publishes offboard control mode and take-off hover waypoint.
   
    Actions: Transitions to MISSION state after reaching a specified altitude above the home position.

5. MISSION State:

    Objective: Execute the mission path generated by the path planning node.
   
    Conditions: Loops through path waypoints, publishing trajectory waypoints and waypoint velocity vectors. Verifying that the drone has reached the waypoint within a tolerance before publishing the next waypoint.
   
    Actions: Transitions to Return-to-Launch (RTL) upon completing the mission path or when the drone battery is low to recharge at the home position.

6. LAND/RTL States:

    Objective: Initiate landing or return-to-launch procedures.
   
    Conditions: Monitors the drone's mode transition.
   
    Actions: Transitions to LIMBO upon mode switch completion.

7. LIMBO State:

    Objective: A placeholder state with no specific actions.
   
    Conditions: Remains in this state until a transition is triggered.

8. FAIL State:

    Objective: Handle emergency errors by transitioning to LAND.
   
    Conditions: Triggered when unexpected errors occur.
   
    Actions: Transitions to LAND state for safety.

9. Default State:

    Objective: Default state for logging and debugging purposes.
   
    Conditions: Logs information when none of the predefined states are matched.

This intricate state machine encapsulates the intelligence required for our UAV to navigate through its mission with precision, resilience, and adaptability. It seamlessly integrates with various functionalities, ensuring a dynamic response to the evolving requirements of the mission environment.

### Gazebo Simulation (PX4 SITL)
In the quest for reliable and safe UAV operations, I employed the PX4 SITL (Software-In-The-Loop) Gazebo simulation environment as a crucial pre-flight testing ground. This strategic decision aimed to validate code changes and mission plans comprehensively before deploying the UAV in real-world scenarios. By doing so, I created a robust development pipeline that substantially minimized risks, ensuring a flawless execution during actual flight operations.

Key Benefits of PX4 SITL Gazebo Simulation:

1. Code Validation and Testing:

    The PX4 SITL simulation environment provided a realistic virtual platform for testing code changes. This enabled me to identify and rectify bugs or issues in the control software before deploying it to the physical UAV. The simulation served as a proactive measure to catch potential problems, contributing to the seamless integration of new functionalities.

2. Mission Plan Validation:

    Mission changes, crucial for executing specific tasks, were meticulously tested within the PX4 SITL simulation environment. This allowed me to validate and fine-tune mission plans, ensuring that the UAV would navigate its designated path accurately and efficiently during actual flights.

3. Risk Mitigation:

   By thoroughly testing the UAV control software in the simulation environment, it significantly mitigated risks associated with unforeseen issues or unsafe behavior during real-world flights. This proactive approach translated to a 100% success rate in take-offs, landings, and overall flight operations, minimizing the chances of crashes or erratic behavior.

The adoption of PX4 SITL not only served as a risk mitigation strategy but also streamlined the development process. Code changes and mission modifications underwent rigorous testing in the simulation, ensuring that the UAV was well-prepared for its outdoor missions. In essence, the utilization of PX4 SITL simulation was pivotal in achieving a 100% success rate in UAV flight operations. 

<iframe width="960" height="640" src="https://www.youtube.com/embed/ahojRCN_Bg4?si=X4qH7ORprZA94zkb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Custom YOLOv8 models for cattle counting

![cattle_yolov8](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/386fbf74-6555-42e5-afde-543830e1fa2c)

### Custom Built 3 DOF (Roll, Pitch, Yaw) PID Tuning Rig
A purpose-built 3DOF rig was designed and built and played a crucial role in refining the Roll, Pitch, and Yaw rate and attitude PID controllers, providing a controlled environment for meticulous tuning.

- Key Features of the Custom 3DOF Rig:
    - Individual Axis Isolation:
    
        The rig's design incorporated a thoughtful approach by allowing individual axis isolation. This strategic feature proved instrumental in dissecting and fine-tuning each controller independently, ensuring an optimized performance profile for every axis before combining them.

    - Dynamic PID Controller Tuning:

        The 3DOF rig served as a dynamic testing ground, enabling manual application of step inputs through the radio controller. This approach facilitated real-time adjustments and tuning of the PID controllers for Roll, Pitch, and Yaw rates, as well as attitude control. This live feedback,            displayed through the QGround Control Station, allowed for precise calibration of the PID gains.

    - Bridging the Gap to Outdoor Flight:

The 3DOF rig played a pivotal role in bridging the gap between indoor PID tuning and outdoor flight dynamics. By achieving an optimal state in the controlled environment provided by the rig, it set the stage for a smoother transition to the complexities of real-world flight. Fine-tuning         was performed outdoor and incorperated the velocity and position PID controllers.

<iframe width="960" height="640" src="https://www.youtube.com/embed/g6P4DaSF0XQ?si=g6WTKveviRPdK1Fb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Position control Demo
<iframe width="960" height="640" src="https://www.youtube.com/embed/RFKnYLMNUqU?si=yA36iYt479uSDvuw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Custom built drone
![Drone1](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/72938bc9-56bb-4918-a3cf-ca0e6f9d8850)

### Future Work

### Reference:
- Mier_Fields2Cover_An_open-source_2023, Mier, Gonzalo and Valente, João and de Bruin, Sytze, IEEE Robotics and Automation Letters, Fields2Cover: An Open-Source Coverage Path Planning Library for Unmanned Agricultural Vehicles, 2023

<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/ROS2_offboard_drone_control/tree/main" text="GitHub" %}
</p>

