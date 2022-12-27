---
name: PLC Autonomous Cube Stacker
tools: [PLC, Ladder Logic and Structured Text, C++, Proportional control, Mechatronics]
image: https://marnonel6.github.io/assets/homepage_cube_stacker_4x_speed_indicator.gif
description: Built an autonomous cube stacker with a user interface capable of stacking 16 cubes.
---

# PLC Autonomous Cube Stacker
<br>
### Brief overview
Led a team of 3 mechatronic engineer student to program a PLC (Ladder Logic and Structured Text) and an Arduino (C++) to perform autonomous cube stacking. The pick and place robot was designed and built with a $50 budget and scrap materials from the lab. A GUI was designed were a user can input the stacking specifications for the tower in terms or stack height, up to 16 cubes, and stack pattern, silver or black cubes. After the user added the stack pattern the robot will start stacking the cubes based on the user defined pattern untill the pattern is completed. 

<br>
### Video demo

<video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/209709728-f2af391d-850f-4d6b-9443-56a8572f6afc.mp4" type="video/mp4">
</video>

<br>
### Graphical user interface
A GUI was designed to interface with the pick and place robot. On the left there is a vertical array of 16 siwtches to indicate the color of the cube this determines the pattern of the stack. The to the right of that array from top to bottom there is three arm location LEDs that dispays the postion of the arm either in the contre, left or right. Underneath that is a 'light' type switch to turn on the robot, two squre LEDs that displayed the grippers state and a time passed counter. The middle column from top down displays the height of the arm [cm], the stack heinght [cube count], black cubes left to stack, and silver cubes left to stack. A large red digital emergency stop is included and a physical one. The height of the stack can be set by changing the maximum height, this will also disable and enable the vertical array switches on the left accoring tho the value entered here. The Kp gain, absolute height error value is also displayed along with error, command signal, and actual location graphs.

![IMG_3949](https://user-images.githubusercontent.com/60977336/209710214-4d427c01-0b0a-4420-b75c-1668a01d0b2a.JPEG)

<br>
### Project objectives
- Stack up to 16 cubes autonomously
- User should specify cube stack heigth
- User should specify stack color pattern
- Cubes must be stack in a different orienatation that the pick up orientation
- Cubes must be picked up and placed by robot without human interaction
- GUI must display helpfull information
- $50 budget, 1 x 3D print part and scrap material in lab

### 16 Cubes stacked with an alternating pattern
![IMG_3846](https://user-images.githubusercontent.com/60977336/209712768-f1a2db69-328c-48cf-a503-2f378dcc6fe1.JPEG)

<br>
### Setup
![IMG_3828](https://user-images.githubusercontent.com/60977336/209712814-81e486a5-6907-417b-b5ad-373dc544d195.JPEG)

<br>
### Cube feeders
![IMG_3941](https://user-images.githubusercontent.com/60977336/209712713-c1e9ed80-acaf-4540-8564-4fd0e7d4ac67.JPEG)

<!-- <p class="text-center">
{% include elements/button.html link="https://github.com/JiasenZheng/Stereo_Visual_Odometry" text="GitHub" %}
</p> -->
