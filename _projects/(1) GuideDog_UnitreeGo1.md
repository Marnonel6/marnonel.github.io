---
name: Guide dog - Unitree Go1
tools: [ROS2, C++, YOLOv7/Machine learning, NAV2/Navigation, Rtabmap/3D SLAM, Python, Computer vision]
image: https://marnonel6.github.io/assets/guide_dog_high_comp.gif
description: Programmed a quadruped robot dog to autonomously navigate its surroundings with the aid of voice recognition and a custom-trained object detection model.

---

# Guide dog - Unitree Go1
<br>
### Brief overview
<br>
<!-- The project aims to have a Franka Emika 7DOF robotic arm autonomously play airhockey againts an opponent. The project was inspired by the fast dynamics of the game and the variability in game play which does not allow any hard coding and decisions need to be made in real time. Our project used a Realsense camera with OpenCV to detect the puck and calculate the transformation of the puck relative to the robot. The puck coordinates are then used to calculate the predicted trajectory of the puck and estimate where the robot should hit the puck along the predicted trajectory line. Robot Operating System (ROS2) and the MoveIt2 motion planning framework are used to interface with the robot and control its movements. A ROS2 python API wrapper was developed as it was not currently avaible when this project was developed. The custom API is in the moveit_helper package. -->

Guide dogs are assistance dogs trained to lead visually impaired people through everyday life to different destinations around everyday obstacles. The goal of this project is to program a Unitree Go1 quadruped robot dog to be able to understand everyday obstacles and autonomously navigate and lead a human around them to their destination. Guide dogs cost between $20,000 - $40,000 and are red-green color blind and thus incapable of interpreting street signs whereas a camera on a robot dog can be programmed to interpret street signs and plan its path accordingly.

### Video

<!-- <video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/225454720-58e55235-0802-4ade-a01e-7cb94d5c653f.mp4" type="video/mp4">
</video> -->

<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226077809-9e7cc075-c3f5-4d00-9c90-167d8f858de7.mp4" type="video/mp4">
</video>

## Concepts and Overall System Architecture
The process loop of the robot is as follows:

### Object recognition - YOLOv7

<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226078995-964fbce5-dd42-4553-b531-df5996f69850.mp4" type="video/mp4">
</video>

* Upon startup, the robot follows a start-up sequence to reach its home position. The robot follows a series of waypoints 
to reach the home x- and y-coordinates with an offset in the z. It then reaches down to grasp the paddle (with an adapter) 
and moves back up slightly. This slight increase in height allows the robot flexibility while moving, so that if it pushes 
down during movement, it will not apply a force into the table while still keeping the paddle level with the table.

### Voice recognition

<video width="480" height="720" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226080702-4d36313b-e586-4ff9-bc0f-f7c119dfe256.mp4" type="video/mp4">
</video>

* Intel RealSense D435i is used at 480x270x90 allowing the puck to be tracked at 90 fps. As soon as the streaming has been enabled, this node detects the center of the table in pixel coordinates. Then with the help of the depth camera, the deproject function is used to convert pixel coordinates into real world coordinates with respect to the camera. Since the distance between the air hockey table and the Franka robot is known, points from the camera's frame of reference can be transformed to the robot's frame of reference. Next, with the help of OpenCV's HughCircles function the center of the puck is tracked in real time. For the calculation of the trajectory, the puck is only tracked when going towards the robot and up to the center of the table. In order to get rid of noise, before publishing the puck's position it is checked whether the point is close (with a prefixed tolerance) to the best fit line of the previous positions obtained. Note: The output video shows the tracked puck encircled with a black border, regardless of whether all these points are published (in other words, the video shows the contour for every direction of movement of the puck).

### Trajectory Calculations

<video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/206880201-e1849e50-2c71-4b56-8993-bf7feea20640.mp4" type="video/mp4">
</video>

Calculates the predicted trajectory of the puck and the play waypoints for the robot by using two
puck coordinates from computer vision. The node handles collisions by reflecting the impact angle about the normal line. The waypoints for the robot to hit the puck are constrained by the robot's workspace on the air hockey table. The most optimal waypoints are selected by considering all four sides of the robot's workspace. The robot will then move to the first waypoint that is on the predicted trajectory line of the puck and then move along the line to the second waypoint and hit the puck. A plot is dynamically generated and updated each time a new trajectory is calculated. The robot blocks if the trajectory is out of the workspace and unreachable.

### Hit the Puck

<video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60728026/206883262-3a7bd3e8-8259-4f35-b5ad-2bc805d5e52b.mp4" type="video/mp4">
</video>

* After receiving waypoint and goal positions, the robot receives service calls to move to those points, thereby meeting 
the puck along its trajectory and hitting it. If there is an edge case where the robot cannot successfully meet the puck 
given its trajectory, the robot will block instead.


<!-- <br>
### Manipulation
<br>
The manipulation package relies on several different `nodes` in order to function:
1. **manipulation_cap** provides low-level position and orientation sensing services, along with error recovery, movements, and gripper grasping
2. **manipulation_macro_a** provides position movement services for image captures using the RealSense
3. **manipulation_press** provides a pressing service to cap the markers
4. **manipulation_local** provides manipulation services for moving in between trays
5. **manipulation_pnp** provides pick and place services between the feed and assembly trays
6. **debug_manipulation** logs the external forces experienced by the robot
7. **plan_scene** provides a planning scene for simulation-based motion planning in Moveit
8. **limit_set** provides services to be used with the franka_control file launched prior to Moveit being launched. It allows the user to reconfigure the collision limits on the robot. 


Manipulation also relies on a python **manipulation** package with translational, array position, and verification utilities.
A scene. yaml file is used for specifying parameters in the plan_scene node and the main manipulation movements scene elsewhere in the project.

### Perception
<br>
* All the computer vision algorithms are embedded in the **vision** python package, functions in the package can be called in a node by 
```import <package name>.<script name>``` such as 
```shell
import vision.vision1
```
* **sample_capture.py**:  A helper python script to capture images using RealSense 435i RGB-D camera
    1. Connect the RealSense camera to your laptop
    2. Run the python script in a terminal:
    ```shell
    python3 sample_capture.py
    ```
    3. Press 'a' to capture and save an image and use 'q' to quit the image window
* **hsv_slider.py**: A helper python script to find the appropriate HSV range for color detection
    1. Add the path of the image to  `frame = cv. imread()` to read the image
    2. Run the python script in a terminal:
    ```shell
    python3 hsv_slider.py
    ```
    3. A window of the original image and a window of HSV image with slide bars will show up
    4. Test with HSV slide bars to find an appropriate range
* **vision.py** A python script to detect contours and return a list of hue values
    1. For testing purposes, an image can be loaded by setting the path to `image = cv. imread()`
    2. Run the python script in a terminal:
    ```shell
    python3 vision1.py
    ```
    3. A processed image with contours and a list of hue values will be returned

    The node that uses this library is called vision_bridge.
* **vision_bridge node**: Node that publishes a stream of ROS Images and implements a service **capture** that returns a list of H values of the detected markers and caps from an image.
    1. run `rosservice call /capture` and specify the **tray_location** to run the service.
        * tray_location 1 : Represents Assembly Location
        * tray_location 2 : Represents Markers Location
        * tray_location 3 : Represents Caps Location -->


<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/Franka_HockeyBot" text="GitHub" %}
</p>
