---
layout: post
title:  "Scan matching"
excerpt: "Deeper dive into localization with scan matching."
---

## Introduction
Last post was about SLAM, where I introduced approach to localization, here we are going to look at approach to lacalization using sets of range measurements collected over across time. Going back, localization is state of robot with respect to the environment it find self in. One approach to localization is to use odometry, basically estimating the current pose of the robot from knowing a start position and integration of control and motion measurements. However there is accumulation of very small errors in the position of the robot which becomes issue over some extended period time and the uncertainty of the pose increases. One way to solve this is to use range sensor measurements to localize, also known as scan matching.

## Setting a Problem
Having a scan from robot of measurements of distances to some landmarks A,B,C in some environment, we call this measurements at t=0 distances in local frame of reference. When we move in unknown direction at time t=1, the distances are obviously going to change. We can measure these distances at time t=1 and we want find transform R which transforms the two set of points to be the closest and this transform represents how much the robot moved. This is visualised on the picture bellow for better intuition ( ref [module C, lecture 9 F1Tenth website](https://f1tenth.org/learn.html))
![problem_scan_matching](/assets/problem_scan_matching.png)

However only from scan readings alone, we can't know the landmarks A,B,C exactly, therefore we have to assume that the closest points correspond to each other(correspondence match) and than iteratively find the best transform R.

### Iterative search for the best transform
1. Make a initial guess of R
2. For each point in new scan (t=k+1) find closest point in previous set (t=k) -> correspondence search
3. Make another better guess of R
4. Set next guess to the current guess (repeat 2-4 until converges)

For this algorithm (Iterative closest point or ICP) to converge, the choice of initial guess is important. Also ICP can be slow. For this reason PL-ICP was introduced (Point-to-Line Iterative Closest Point).

### Point-to-Line Iterative Closest Point
Instead of looking for point to point metric, PL-ICP is looking for point-to-line metric. Point-to-point measures the distances the distance to the nearest point on the segment, while point-to-line measures distance to nearest line containing the segment. Point-to-Line converges faster because the level sets the approximate surface better and therefore results in more accurate approximation or the error. This is achieved by the contours of the level being line and not concentric and centred around the projected point and thus giving algorithm quadratic convergence instead of linear convergence.

#### PL-ICL algorithm
at t=k
1. use previous guess q_k, transform coordinates of current scan into frame of previous scan
2. For each point, find closest line segment (correspondence search)
3. Update transform
  a. formulate point-to-line-error objective
  b. find transform q<sub>k+1</sub> that minimizes objective

PL-ICL also uses smart "tricks" in correspondence match search. It does local search with early termination and jump table for each scan point.

TODO DO MORE RESEARCH IN PL-ICL AND DESCRIBE BETTER POSSIBLY AFTER ASSIGNMENT.

To understand this algorithm better I recommend read trough original paper [2008 ICRA-PLICP](https://censi.science/pub/research/2008-icra-plicp.pdf) by Andrea Censi.

## F1Tenth Lab5 assignment
My final package as required in the task is available bellow.

TODO

pdf with theoretical part and txt with video links of the assignment included in the zipfile.

TODO VIDEO
