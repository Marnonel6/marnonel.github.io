---
name: Guide dog - Unitree Go1
tools: [ROS2, C++, YOLOv7/Machine learning, NAV2/Navigation, Rtabmap/3D SLAM, Python, Computer vision]
image: https://marnonel6.github.io/assets/guide_dog_high_comp.gif
description: Programmed a quadruped robot dog to autonomously navigate its surroundings with the aid of voice recognition and a custom-trained object detection model.

---

# Guide dog - Unitree Go1 

<p class="text-center">
{% include elements/button.html link="https://www.trossenrobotics.com/unitree-overview?utm_source=google&utm_medium=cpc&utm_campaign=&utm_term=&utm_adgroup=&gclid=Cj0KCQjwn9CgBhDjARIsAD15h0AKNGop2arLaFG7QrKSj08WFxTdRc5kYTbyQ8zrEiDIc3FSZB_ygzIaAuF9EALw_wcB" text="Unitree Go1" %}
</p>

### Brief overview

Guide dogs have been used for decades to assist visually impaired individuals in navigating their surroundings. However, these dogs come at a high cost, ranging from $20,000 to $40,000. Additionally, they are red-green color blind, which limits their ability to interpret street signs and other visual cues.

To address these limitations, a I Programmed the Unitree Go1 quadruped robot dog to become an autonomous guide dog. By incorporating advanced technologies such as speech recognition, object recognition, and autonomous navigation, the robot dog will be able to help visually impaired individuals navigate their surroundings and avoid obstacles. And because a robot dog can be updated through machine learning and software updates, it is possible to continually improve its abilities over time.

Overall, this project represents an exciting advancement in assistive technology, providing a cost-effective and long-lasting alternative to traditional guide dogs. By combining the latest in robotics and machine learning, the Unitree Go1 robot dog has the potential to revolutionize the way visually impaired individuals navigate the world around them.

<br>
### Introduction video
<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226077809-9e7cc075-c3f5-4d00-9c90-167d8f858de7.mp4" type="video/mp4">
</video>

### Object recognition - YOLOv7

YOLOv7 was the chosen object detection algorithm. It is a real-time object detection algorithm that is based on the You Only Look Once (YOLO) architecture and consists of convolutional neural networks (CNNs). A python ROS2 YOLOv7 package was developed with Rintaroh Shima for real-time object detection. Below is a sample video of the custom trained model. An Intel RealSense D435i was mounted on Go1 with its frame specifications at 620x480 and 30fps.

<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226078995-964fbce5-dd42-4553-b531-df5996f69850.mp4" type="video/mp4">
</video>

<p class="text-center">
{% include elements/button.html link="https://universe.roboflow.com/guidedogunitreego1/guide_dog-fhaac" text="Custom guide dog dataset" %}
</p>

A custom dataset was created (Link above) and hand labeled. In order to ensure that the model was as general as possible the photos where augmented in several different ways (Contrast, stretched, flipped, blurred, etc). Below is the labeled test data and the final models predictions.

![yolov7_test_data](https://user-images.githubusercontent.com/60977336/226084555-74ed7a13-bb35-461a-8069-ade6f4d9e4a5.jpg)

![yolov7_test_data_2](https://user-images.githubusercontent.com/60977336/226084557-815a6129-ed7d-4d68-bfa1-9e4a4e94a4f4.jpg)

The confusion matrix indecates the the model achieved a average true positive rate of 75% and a false negative rate of 25% over all the classes, indicating that the model is able to accurately detect and classify objects in images with a relatively high degree of precision and recall. Furthermore, the model achieved a false positive rate of 0% and a true negative rate of 100% on average, suggesting that it is highly accurate in distinguishing between positive and negative instances. These results are promising and indicate that the model is a reliable and accurate solution for object detection tasks within the classes and in the setting which the data set was taken.

![confusion_matrix](https://user-images.githubusercontent.com/60977336/226084551-653490a5-0fa7-47d6-ba16-141a5a0a84f8.png)

My custom object detection model achieved a mean Average Precision (mAP) score of 0.85 when the intersection over union (IoU) threshold was set at 0.5, indicating good performance in localizing objects. The model also achieved a mAP score of 0.58 across a range of IoU thresholds from 0.5 to 0.95, indicating reasonable performance in detecting objects with varying levels of overlap. The model achieved a precision of 0.88 and a recall of 0.82, indicating that the model is able to accurately identify a high percentage of relevant objects while minimizing the number of false positives. These metrics are important indicators of the overall performance of an object detection model and suggest that the model is well-suited for a variety of applications that require accurate detection and localization of objects.

![results](https://user-images.githubusercontent.com/60977336/226084553-42ea7893-e17f-4351-ab5f-288e6e5c72d3.png)

The model achieved an average F1 score of 0.77 across a confidence range of 0.1 to 0.8. This suggests that the model performs well at a range of confidence levels and is able to accurately detect and classify objects with a high degree of precision and recall.

![yolov7_guide_dog](https://user-images.githubusercontent.com/60977336/226084554-6955c8d6-c14c-4048-9177-05a4b6df1fac.jpg)


### Voice recognition

<video width="480" height="720" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226080702-4d36313b-e586-4ff9-bc0f-f7c119dfe256.mp4" type="video/mp4">
</video>

* Intel RealSense D435i is used at 480x270x90 allowing the puck to be tracked at 90 fps. As soon as the streaming has been enabled, this node detects the center of the table in pixel coordinates. Then with the help of the depth camera, the deproject function is used to convert pixel coordinates into real world coordinates with respect to the camera. Since the distance between the air hockey table and the Franka robot is known, points from the camera's frame of reference can be transformed to the robot's frame of reference. Next, with the help of OpenCV's HughCircles function the center of the puck is tracked in real time. For the calculation of the trajectory, the puck is only tracked when going towards the robot and up to the center of the table. In order to get rid of noise, before publishing the puck's position it is checked whether the point is close (with a prefixed tolerance) to the best fit line of the previous positions obtained. Note: The output video shows the tracked puck encircled with a black border, regardless of whether all these points are published (in other words, the video shows the contour for every direction of movement of the puck).

### Autonomous navigation

<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226083481-d9013ea6-5fef-463e-a67c-af217bf6d331.mp4" type="video/mp4">
</video>

Calculates the predicted trajectory of the puck and the play waypoints for the robot by using two
puck coordinates from computer vision. The node handles collisions by reflecting the impact angle about the normal line. The waypoints for the robot to hit the puck are constrained by the robot's workspace on the air hockey table. The most optimal waypoints are selected by considering all four sides of the robot's workspace. The robot will then move to the first waypoint that is on the predicted trajectory line of the puck and then move along the line to the second waypoint and hit the puck. A plot is dynamically generated and updated each time a new trajectory is calculated. The robot blocks if the trajectory is out of the workspace and unreachable.

### Dynamic obstacle avoidance

<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226083479-69d3d70b-84c8-44f1-b8c8-3f050e961807.mp4" type="video/mp4">
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
