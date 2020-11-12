---
layout: post
title:  "Wallfollowing"
---

## Introduction

Lecture 3 as prerequisite for wallfollowing algorithm covers one of the last ROS introductory topic as that are Rigid Body Transformation.
Coordinate frames in ros such as map frame, lidar frame and other sensors provide informations specific to that sensor. Coordinate frame is
set of 3 orthogonal axes for X,Y and Z direction with a position where this frame is placed. Any poistion of word, robot, etc. only makes sense
when we defined the frame in which we are describing the pose. The direction of X,Y, Z axes is defined by right-handed rule, where index finger points in
X direction, middle finger in y direction and thumb in Z direction. Let's look more into transformations and frames.

## Transfomations and Frames

Why do we need transformations between different sensors? Each time I get data in sensor frame (for specific sensor) which I want to transform in some unified way.
For example we can have frame of reference of lidar which we can transform into the frame of reference of robot, which is the center of the rear axle. As example in
the last assignment we only take in account the frame of reference of lidar and stopped the car based on position of the lidar. However car have certain legnth and lidar
can be place in the center of the car. Therefore the correct way of stopping before collision should be calculated from the front of the car (or edge of the car), not from the
position of the lidar. Between frames there will exist transformation that convert measuring from one frame to another.

## Reference Frames on F1Tenth car

#### map
The origin set by the user and every other transformation can be measured with respect to the map. Represent the environment where the car will be racing. Can be place arbitrary and typically never moves after its placed.

#### base_link
Positioned at center of the rear axle of the car. Moves with the car relative to the map frame.

#### lidar
The frame of reference where lidar scan measurements are taken. Moves with the car relative to the map frame.

#### odom
Typically required by ROS. Usually the initial position of the robot in the map before everything began. Fixed relative to the map.


### Rigid Body Transfomation

  How to transform data and postition between one frame and anohter. As on figure bellow, we want the coordinate of point p in frame 2, given the coordinate of point p in frame 1.

<img src="/assets/frames.png">
2 diffrent coordinate frames and point p from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/ROS-tf.pdf">UV F1/10 course</a>
  Steps:
  * Overlap both frames so their origins are at the same point. The reason is that we want to compute how is second frame of reference rotated. (apply rotation)
  * Describe the unit vectors of the second frame of reference in the terms of unit vectors of first frame of reference. As on picture bellow.

<img src="/assets/frames2.png">
Overlapped frames of reference from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/ROS-tf.pdf">UV F1/10 course</a>

x<sub>2</sub> = R<sub>11</sub>x<sub>1</sub> + R<sub>21</sub> y<sub>1</sub><br>
y<sub>2</sub> = R<sub>12</sub>x<sub>1</sub> + R<sub>22</sub> y<sub>1</sub><br>
The formula above describes the units vector of new frame of referece as a linear combination of the original frame of reference. (considering only X,Y axis - 2D problem, no Z).
The formula can be also rewriten as matrix:

<img src="/assets/rotation_matrix.png">
Rotation matrix from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/ROS-tf.pdf">UV F1/10 course</a>

Also we define &theta; as angle between x<sub>1</sub> and x<sub>2</sub> as shown on figure bellow. This angle tells us how rotated this frame is.

<img src="/assets/theta.png">
Overlapped frames of reference with angle theta from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/ROS-tf.pdf">UV F1/10 course</a>

To get the coefficients R<sub>11</sub>, R<sub>21</sub>, R<sub>12</sub>, R<sub>22</sub>, along x<sub>2</sub> unit vector direction, contains the cos unit component of x<sub>1</sub>
unit vector as well as sin component of y<sub>1</sub> unit vector. Thefore  x<sub>2</sub> and similary y<sub>2</sub> can be written as:


x<sub>2</sub> = cos(&theta;)x<sub>1</sub> + sin(&theta;)y<sub>1</sub><br>
y<sub>2</sub> = -sin(&theta;)x<sub>1</sub> + cos(&theta;)y<sub>1</sub><br>


Therefore I get:

<img src="/assets/rotation_matrix2.png">
Rotation matrix from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/ROS-tf.pdf">UV F1/10 course</a>

With this we are able to represent unit vectors in the new frame of reference in terms of unit vectors of original frame of reference. Similarly we can express the position of point P in the new frame of reference using the rotation matrix as shown on formulas bellow.

<img src="/assets/rotation_p.png">
Rotation matrix of point p from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/ROS-tf.pdf">UV F1/10 course</a>.
We can't forget to apply the translation as well.


#### Wallfollowing
The idea is we are trying to compute the error between the future position of the car instead of current error because of constant movement of the car.
By minimising the future distance from the wall with respect to the optimal trajectory we are able to follow the wall.
TODO DESCRIBE MY IMPLEMENTATION AS THE ALGORITHM.


## F1Tenth Lab3 assignment
My final package as required in the task is available bellow.

<a href="https://github.com/smejkka3/smejkka3.github.io/raw/master/assets/karel_lab3.zip">karel_lab3.zip</a>

txt with video links of the assignment included in the zipfile.

<iframe width="800" height="400" src="https://www.youtube.com/embed/5nLtlszkRvI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
