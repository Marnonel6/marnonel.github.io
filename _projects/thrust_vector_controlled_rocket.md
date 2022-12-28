---
name: Autonomous thurst vector controlled rocket
tools: [C++, PCB, PID, EAGLE, CAD, WiFi Microcontroller]
image: https://marnonel6.github.io/assets/Launch_Comp.gif
description: Invented, built, and tested a thrust vector controlled scaled rocket.
---

# Autonomous airhockey robot
<br>
### Brief overview
<br>
Invented, built, and tested a thrust vector controlled scaled rocket for my undergraduare thesis in mechatronic engineering. I implemented a PID controller and a state machine in C++ for autonomous active orientation control in-flight and recovery. I formulated and constructed a custom flight controller PCB using EAGLE. Designed the rocket body, thrust vector control mount and parachute deploy system in Inventor/Fusion360. A autonomous launchpad and App was built to launch the rocket from a safe distance. A ESP32 was used for wireless protocols between the launchpad and the android app created with MIT AppInventor.

(Final thesis is attatched below.)

<br>
### Video demo

<video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/209871829-384275b5-b4c0-40c1-b5ab-553239c13085.mp4" type="video/mp4">
</video>

<br>
### Parachute system
The parachute system utilizes a spring to eject the parachute and the door when a servo is turned that is blocking the door. The servo is opened at 90% of the apogee altitude. Altitude of the rocket is calculated by using a barometer, a temperature sensor and knowing the height above sea level.

<video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/209882755-b34d157f-3427-46ed-bdff-1a86099825ec.mp4" type="video/mp4">
</video>

<br>
### Flight controller PCB
![FlightContoller](https://user-images.githubusercontent.com/60977336/209883891-9514522a-a0f9-4447-b11e-be9a7cf886ee.png)

<br>
### Mission control android APP
<video width="720" height="480" controls="controls">
  <source src="https://user-images.githubusercontent.com/60977336/209884377-9483c9a4-38a3-41ef-9fac-24a6cccb6972.mp4" type="video/mp4">
</video>

<br>
### Launchpad motor ignition connection
![Launchpad](https://user-images.githubusercontent.com/60977336/209884498-70993136-09b6-47f6-b52f-d9abc897dd7f.png)


<br>
<li class="inline-block">
  <a
    target="_blank"
    class="align-middle link-primary mr-2 mr-lg-0 ml-lg-2"
    href="/assets/20852657-Nel.pdf"
    >Thesis_TVC.pdf</a
  >
</li>
<br>
<object data="{{ site.url }}{{ site.baseurl }}/assets/20852657-Nel.pdf" width="1200" height="1200" type="application/pdf"></object>

