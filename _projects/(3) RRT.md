---
name:  Rapidly-exploring Random Tree
tools: [RRT, Python, Path Planning]
image: https://marnonel6.github.io/assets/RRT_m.gif
description: Implementation of the RRT algorithm
---

# Rapidly-exploring Random Tree <br><br>

### Brief overview
A rapidly exploring random tree (RRT) is an algorithm designed to efficiently search nonconvex, high-dimensional spaces by randomly building a space-filling tree. The tree is constructed incrementally from samples drawn randomly from the search space and is inherently biased to grow towards large unsearched areas of the problem. RRTs were developed by Steven M. LaValle and James J. Kuffner Jr. They easily handle problems with obstacles and differential constraints (nonholonomic and kinodynamic) and have been widely used in autonomous robotic motion planning.

### Results
* RRT with no obstacles (100 & 1000 iterations)
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT0.gif" />
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT1.gif" />
* RRT with circular obstacles
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT_a.gif" />
<!-- * RRT with arbitary obstacles
<img src="{{ site.url }}{{ site.baseurl }}/assets/RRT3.gif" /> -->

### Algorithm descriptions
* pseudocode for the basic algorithm with no obstacles:
<!-- <img src="{{ site.url }}{{ site.baseurl }}/assets/RRT4.png" /><br> -->

![Screenshot from 2022-12-12 22-01-34](https://user-images.githubusercontent.com/60977336/207223788-311ce13e-b75d-4a80-a017-762573dfe067.png)

* **CHECK_POINT_COLLISION** Checks if the point collides with the obstacles
* **CHECK_EDGE_COLLISION** Checks if the edge collides with the obstacles
* **CHECK_COLLISION_FREE_PATH** Check if there is a collision-free path to the goal position <br><br>

<p class="text-center">
{% include elements/button.html link="https://github.com/Marnonel6/Rapidly_Exploring_Random_Tree" text="GitHub" %}
</p>
