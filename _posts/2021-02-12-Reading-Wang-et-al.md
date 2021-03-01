---
layout: post
title:  "Reading notes: Model-free detection and tracking of dynamic objects with 2D lidar, Wang et al."
excerpt: "Notes from paper for Model-Free Detection and Tracking of Dynamic Objects with 2D LiDAR by Dominic Zeng Wang, Ingmar Posner and Paul Newman."
---

Mentioned in post before, the goal of the project is to implement algorithm mentioned in the paper from [Wang et. al](https://www.robots.ox.ac.uk/~mobile/Papers/2015IJRR_wang.pdf). The paper focuses on crating framework that estimates the pose of the sensor and determines static background as well as movement of dynamic objects.

 At the moment of writing we are in the stage of implementing the algorithm outside of simulation. The algorithm itself can be divided into several parts. Firstly the new laser and odom measurements coming from car need to be processed. The process of laser measurement consists of first cleaning the states, followed by forward propagation, then Association and update of dynamic and static objects finalised by merging tracks. The whole algorithm is outline in following figure ![alg](/assets/wangetalalg.png)
