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

![DSC03642](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/582d1994-ba0f-4c92-823f-b5a3b7a017d8)

### Overview video


### High level program diagram
![AgTech Drone](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/9ab9a8d6-d4f8-4776-93f2-e3e19734c19d)

### Autonomous RPi maiden flight
<iframe width="960" height="640" src="https://www.youtube.com/embed/qZnLWsMiW1M?si=5xryIZkE3JAymTE5" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Position control
<iframe width="960" height="640" src="https://www.youtube.com/embed/RFKnYLMNUqU?si=yA36iYt479uSDvuw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

### Simulation Video (SITL Gazebo)

### Custom built drone
![Drone1](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/72938bc9-56bb-4918-a3cf-ca0e6f9d8850)





<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/ROS2_offboard_drone_control/tree/main" text="GitHub" %}
</p>

