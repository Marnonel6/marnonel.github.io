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

### Position control
<iframe width="960" height="640" src="https://www.youtube.com/embed/RFKnYLMNUqU?si=yA36iYt479uSDvuw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Gazebo Simulation (PX4 SITL)
<iframe width="960" height="640" src="https://www.youtube.com/embed/ahojRCN_Bg4?si=X4qH7ORprZA94zkb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Custom Built 3 DOF (Roll, Pitch, Yaw) PID Tuning Rig
<iframe width="960" height="640" src="https://www.youtube.com/embed/g6P4DaSF0XQ?si=g6WTKveviRPdK1Fb" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Custom built drone
![Drone1](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/72938bc9-56bb-4918-a3cf-ca0e6f9d8850)

### Future Work

### Reference:
- Mier_Fields2Cover_An_open-source_2023, Mier, Gonzalo and Valente, João and de Bruin, Sytze, IEEE Robotics and Automation Letters, Fields2Cover: An Open-Source Coverage Path Planning Library for Unmanned Agricultural Vehicles, 2023

<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/ROS2_offboard_drone_control/tree/main" text="GitHub" %}
</p>

