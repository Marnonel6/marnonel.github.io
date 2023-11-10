---
name: Stereo Visual Odometry from scratch
tools: [COmputer vision, Kinematics, OpenCV, Python, KITTI]
image: https://marnonel6.github.io/assets/Drone.JPG
description: Developing a stereo visual odometry pipeline from scratch.

---

# Stereo Visual Odometry

<br>
### Brief overview
<br>
Visual odometry is the process of determining the position and orientation of a mobile robot by using camera images. A stereo camera setup and KITTI grayscale odometry dataset are used in this project.
KITTI dataset is one of the most popular datasets and benchmarks for testing visual odometry algorithms. This project aims to use OpenCV functions and apply basic cv principles to process the stereo 
camera images and build visual odometry using the KITTI dataset.

### SIFT Visual Odometry Demo Video [4x]
<iframe width="960" height="640" src="https://www.youtube.com/embed/XMeyRDLE4K4?si=ENV-ZfgACNpz-pgL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/a005471a-36ff-45d8-ba68-4eabfab59101

### Disparity Map
![disparity_map](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/29e18797-8492-4a89-a7e0-6c274891e3a2)

### Depth Map
![Depth_map_limit_50m](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/366a4a37-f0df-4a07-b14f-688988b5fdf7)

### SIFT Features
![SIFT_Features](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/010f0166-ff7e-4a0f-ac7c-ebf2c003bf30)

### SIFT Feature Matching
![Feature_Matching](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/e5bcdd1a-d7f8-4b06-924a-66b24eb8a502)

### Odometry path
- Red Visual odometry path
- Black ground truth path

#### SIFT
![sift](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/6079b188-07c5-4fce-ba07-d598dfbd67d5)

#### ORB
![orb](https://github.com/Marnonel6/marnonel6.github.io/assets/60977336/d17fe740-41fe-4e64-a29a-93425f3323f5)

### Presentation
[Stereo Visual Odometry Presentation.pdf](https://github.com/Marnonel6/marnonel6.github.io/files/13320831/Stereo.Visual.Odometry.Presentation.pdf)

### Write-up
[Stereo Visual Odometry - MJ Nel.pdf](https://github.com/Marnonel6/marnonel6.github.io/files/13320838/Stereo.Visual.Odometry.-.MJ.Nel.pdf)



<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/ROS2_offboard_drone_control" text="GitHub" %}
</p>

