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
To solve this, we need to constrain the center of the arc to be on y axes of the car. Using this we can start solving the equations bellow using basic trigonometry. Because we constrained the center on y axis as radius of the arc, r can be computed as r = |y| + d. We also have right angle triangle therefore d<sup>2</sup> + x<sup>2</sup> = r<sup>2</sup>. Substituting d as the first equation r = |y| + d we get (r - |y|)<sup>2</sup> + x<sup>2</sup> = r<sup>2</sup> and solving this equation we get r<sup>2</sup> + L<sup>2</sup> - 2r|y| = r<sup>2</sup>. The L is again from simple Pythagorean theorem because we know x<sup>2</sup> and y<sup>2</sup> in the triangle we can express L as L<sup>2</sup> = x<sup>2</sup> + y<sup>2</sup> shown on the picture bellow (ref [f1tenth Module D, lecture 13](https://f1tenth.org/learn.html))
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
My final package as required in the task is available bellow.

TODO

txt with video link of the assignment included in the zipfile.

TODO VIDEO
