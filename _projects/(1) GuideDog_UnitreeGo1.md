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

YOLOv7 was the chosen object detection algorithm. It is a real-time object detection algorithm that is based on the You Only Look Once (YOLO) architecture and consists of convolutional neural networks (CNNs). A python ROS2 YOLOv7 package was developed with Rintaroh Shima for real-time object detection. Below is a sample video of the custom trained model. An Intel RealSense D435i was mounted on Go1 with its frame specifications at 620x480 and 30fps. The model was downsized and deployed on a Nvidia Jetson Nano on Go1.

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
The use of the Picovoice deep learning voice recognition library has enabled the creation of a custom wake word "Hey Willie" and commands like walk, stop, stand up, lay down, bark, and increase or decrease speed. By implementing this library, the user is able to control the movements of Go1 with their voice while navigating around. A ROS2 C++ and Python package was developed to hadle the voice recognition and translate the voice commands to desired controls. I utilized gTTS (Google Text-to-Speech), a Python library that generates text to speech audio files. I generate audio files that Go1 uses to effectively communicate with the user. The package was deployed on a Nvidia Jetson Nano on Go1.

<video width="480" height="720" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226080702-4d36313b-e586-4ff9-bc0f-f7c119dfe256.mp4" type="video/mp4">
</video>

### Autonomous navigation
(This whole section was done in collaboration with Nick Morales)

One of the key tasks for this project was enabling the Unitree Go1 robot to navigate autonomously around a mapped/unmapped environment while avoiding obstacles. To achieve this, we decided to leverage ROS 2's Nav2 package, a software stack specifically designed for this purpose.

We utilized the RoboSense RS-Helios-16P 3D LiDAR that comes with the Unitree Go1 EDU Plus model. RTAB-Map was our choice of processing software for the point cloud data, which offers 3D mapping capabilities. The package provides the Nav2 stack with ICP odometry and SLAM updates on the robot's position as well as objects in the environment. Nav2 then leveraged this information to generate local and global costmaps, which allowed it to plan collision-free paths. A ROS 2 node was developed in C++ that sends commands to Nav2 to plan and execute paths to a desired pose. The node was responsible for enabling the robot to follow a given path while avoiding obstacles in real-time, thus ensuring safe and efficient navigation.


<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226083481-d9013ea6-5fef-463e-a67c-af217bf6d331.mp4" type="video/mp4">
</video>


### Dynamic obstacle avoidance

The increased NAV2 footprint that encapsulates the user ensures dynamic obstacle avoidance not just for the robot, but also the user behind Go1. This adjustment helped minimize the risk of collisions and improved the overall user experience by allowing the robot to navigate in a safe and efficient manner.

<video width="960" height="640" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/226083479-69d3d70b-84c8-44f1-b8c8-3f050e961807.mp4" type="video/mp4">
</video>

## Future work
* Detect pedestrian road signs with computer vision and machine learning and lead the
  human in an appropriate way. Examples: Crosswalk signals, Crosswalk signs,
  Crosswalk, Stop sign, etc.
* Detect different types of everyday obstacle and lead the human in an appropriate way. Examples: Stairs, road   crossing, curb, closed door, etc.
* GPS utilization for navigating to a specified location and localization.
* Connect Go1 to the internet for an Alexa like experience.
* Update Go1's Jetson Xavier NX and preform all computations onboard Go1.
* Preform sensor fusion between the ICP odometry, Go1's odometry and the GPS.
* Utilize Chat GPT API for natural language processing and response to commands.

## Significant people who contributed to the project:
The guide dog project was my own individual project, but some subsets of this project had collaborations with Nick Morales, Katie Hughes, Ava Zahedi and Rintaroh Shima. Thank you all for you contributions.


<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/Franka_HockeyBot" text="GitHub" %}
</p>
