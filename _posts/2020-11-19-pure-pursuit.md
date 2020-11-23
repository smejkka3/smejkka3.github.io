---
layout: post
title:  "Pure Pursuit"
excerpt: "First practical experience with Cartographer SLAM package and implementation of trajectory planner called the Pure Pursuit algorithm."
---

## Introduction
Very effective algorithm all the way from 1992 is based on very simple idea of following a waypoint. It is tracking algorithm with assumption that we already know where the set of waypoints is known both in global and local coordinate. This can be done by cartographer SLAM algorithm for example.  

### Geometric representation
Starting from car's local frame (base_link), we need to define so called Lookahead distance L, which is simply radius around the base_link where we are looking for waypoint. Because F1Tenth is nonholonomic robot, to get to the waypoint we need to turn along some arc, it is not possible to move to the waypoint in straight direction. However there can be indefinitely many arcs define to a single point as sown on the figure bellow (ref [f1tenth Module D, lecture 13](https://f1tenth.org/learn.html)).
![pp](/assets/pure_pursuit_geometric.png)
To solve this, we need to constrain the centre of the arc to be on y axes of the car. Using this we can start solving the equations bellow using basic trigonometry. Because we constrained the centre on y axis as radius of the arc, r can be computed as r = |y| + d. We also have right angle triangle therefore d<sup>2</sup> + x<sup>2</sup> = r<sup>2</sup>. Substituting d as the first equation r = |y| + d we get (r - |y|)<sup>2</sup> + x<sup>2</sup> = r<sup>2</sup> and solving this equation we get r<sup>2</sup> + L<sup>2</sup> - 2r|y| = r<sup>2</sup>. The L is again from simple Pythagorean theorem because we know x<sup>2</sup> and y<sup>2</sup> in the triangle we can express L as L<sup>2</sup> = x<sup>2</sup> + y<sup>2</sup> shown on the picture bellow (ref [f1tenth Module D, lecture 13](https://f1tenth.org/learn.html))
![pp2](/assets/pp_geometric2.png)

Solving r<sup>2</sup> + L<sup>2</sup> - 2ry = r<sup>2</sup> for r we have r = L<sup>2</sup>/2y. Since we know the radius, we can compute the curvature of arc as inverse of radius &gamma; = 1/r = 2y/L<sup>2</sup>

### Picking a goal point
There is many choices of how to pick a waypoint in the lookahead radius L, the simplest one is probably either choice the farthest waypoint the the L circle or closest outside of circle. Another example could be get both the closest point outside of circle and farthest point inside of circle, connect them and set a waypoint to the intersection of this connection and circle of lookahead distance. Visualised on figure bellow (ref [f1tenth Module D, lecture 13](https://f1tenth.org/learn.html))

![waypoint](/assets/pp_waypoint.png)

### Tuning
The main thing we need to is the lookahead distance L. Smaller L acts same as higher Kp in PID control where the car is gonna be more aggressive and do higher curvatures and oscillations, with higher L is the opposite, the trajectory will be smoother, but the tracking error will be higher.

### Pipeline
(ref [f1tenth Module D, lecture 13](https://f1tenth.org/learn.html))
1. Create a map using Cartographer
2. Create a set of waypoints using global planner (e.g. by driving by teleop)
3. Pick waypoints to track at each frame
4. Set steering angle to track current waypoint
5. Update the waypoint on the go

## F1Tenth Lab6 assignment

### Encountered difficulties
During this assignment for the first time I experienced some longer sessions on front of monitor to resolve some of the issues I had, as well as had problems with my programmings skills themselves.

1. Cartographer
I was able to make cartographer work locally as shown on examples on [https://google-cartographer-ros.readthedocs.io/en/latest/demos.html](https://google-cartographer-ros.readthedocs.io/en/latest/demos.html), after that I moved the files as mentioned in the [pure_pursuit lecture video](https://youtu.be/L51S2RVu-zc?t=4403), I had to change in file F110.xarco line 194 from
```shell
>>> <mesh filename="package://racecar_description/meshes/hokuyo.dae"/>
```   
to
```shell
>>> <mesh filename="package://f110_description/meshes/hokuyo.dae"/>
```  
From this I suppose that this should be only related to the real car, as I'm not able to make the model of the car work in the simulation of RVIZ, where RVIZ have problem transforming some parameters related to the car. So instead of running cartographer in unknown map I used one of the map that was saved in f1tenth_simulator package already and run waylogger in this map.

2. Switching from Python 3 to Python2.7
In order to run waylogger I needed to install package with Particle Filter. For this I used [MIT RACE CAR ROS PACKAGE](https://github.com/mit-racecar/particle_filter) as I didn't find this package anywhere in the f110. Up until this point I was using python3 without any problems in the assignments so far however this particle_filter and waylogger package both were using python2.7 so I decided to switch my ROS melodic to python2. However setting .bashrc file to python 2.7 didn't switch my ROS system to python2 and was still using python3. I spent long hours trying to figure this out because I think as I was using anaconda with python3 there were some settings which prevented the any system in my ubuntu from using python2 and even completely reinstalling whole ROS melodic didn't and removing anaconda reference from .bashrc file didn't work. I had to uninstall the anaconda completely as well as uninstall and install back ROS.
3. Waypoint Logger
After reinstalling my whole ROS the waylogger was finally working, however it needed running particle_filter topic otherwise it didn't log anything. In the above mentioned particle filter for MIT car I had to change [localize.launch](https://github.com/mit-racecar/particle_filter/blob/master/launch/localize.launch), concretely comment line 30
```shell
>>> 	<!-- <include file="$(find particle_filter)/launch/map_server.launch"/> -->
```
as I was using my own map server for the map from f1tenth_simulator package
and in line 33 use only /odom topic
```shell
>>> 	<arg name="odometry_topic" default="/odom"/>
``` 	
4. Launching waylogger and particle_filter
Strangely enough to get correct coordinates in logfile the particle_filter node needs to be launched as first, before the map and rviz are launched, otherwise the resulting coordinate system is different than the map launched.

After all of this above I was able to generate proper logfile with right coordinates.

Probably most difficulties during the programming of assignment was to correctly transform the global and car correctly otherwise the rest was very straight forward using the formulas from lecture. I expected to have more problems with the visualization, however the ROS Marker tutorial is easy to follow and the visualization was also quickly implement.

My final package as required in the task is available bellow.

[karel_lab6.zip](https://github.com/smejkka3/smejkka3.github.io/raw/master/assets/karel_lab6.zip)

txt with video link of the assignment included in the zipfile.
<iframe width="800" height="400" src="https://www.youtube.com/embed/sAk8qmBD_LI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="800" height="400" src="https://www.youtube.com/embed/aFKKos5si0Y" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
