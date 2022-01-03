---
layout: post
title:  "Follow the Gap"
excerpt: "Addressing the shortcomings of wallfollowing and implementing Follow the Gap algorithm."
---

## Introduction
Let's describe alternative approach to wallfollowing, which is just following left or right hand-side boundary of track. The Follow the Gap algorithm instead tracing the boundary is navigating trough the track without colliding to any static obstacle and finding the best path to avoid these obstacles. The wallfollowing is not completely able to solve all challenges coming with having the obstacles in the track, which are different shape. Follow the gap is simple algorithm which doesn't need a lot fo information about the track before-hand, but using the data acquired right now, known as reactive method. It is able to avoid any obstacles without any prior knowledge about the map.

## Follow the Gap
#### Reactive navigation
Reactive navigation is using immediate sensory input to decide the driving steering and velocity commands. This works for statics and in some cases also dynamic obstacles.

### Gap finding intuition
As the name of algorithm suggests, the car should be able to find a widest gap in the presence of immediate obstacles and drive into this gap to avoid any immediate collision. Let's say my lidar is reporting following array of distances [0.2, 6.2, 6.0, 7.0, inf, 3.0, inf, 3.0, inf, 8.0, 1.0, 3.0] you could simply choose the farthest distance obstacle and drive towards it, however this may not be possible as the car would have to pass between too obstacles which range between them is not enough for car to fit in ( there is enough room in between than - this can be computed by computing length of the arc given the angular measurements), or the angle between these obstacles is not physically possible for car to turn - as in my array is case of 7th element. To approach this problem let's define what a gap is:
 * The gap is series of at least n consecutive hits that pass some distance threshold t.

If we would apply this definition to the array given above with n = 3 and t = 5.0 we would get only one gap: [6.2, 6.0, 7.0, inf]. So we would steer towards the center of this gap.

Problems with this approach is that purely following the deepest gap allows the car pass between the obstacles as the car might not fit in between them. To address this challenge we can use approach use in all of robotics.

#### Point Robot Approach
We can assume that the robot is circular. With this assumption we can inflate the  the obstacles with the radius of robot. So if I want the robot go between the obstacles I can always represent the robot as point object and still assure that the robot will fit between the two obstacles as I'm clearing the obstacles by at least the radius of robot itself. This is better dhown on figure bellow (source: [UV lecture follow the gap](https://linklab-uva.github.io/autonomousracing/assets/files/L14-Follow-the-gap.pdf)):
![PRA](/assets/PRA.png)

#### Disparity
Looking trough the lidar readings for consecutive readings that differ by an amount over some threshold, than we mask these readings  to appear closer to the lidar. The reminding points which are farther than the mask are the actual points where the car can fit and drive trough. This concept is nicely visualised on the following figure from [UV lecture follow the gap](https://linklab-uva.github.io/autonomousracing/assets/files/L14-Follow-the-gap.pdf)
<img src="/assets/ftg1.png" height="500">

### Follow the gap algorithm
* step 1:
  Find the nearest LIDAR point and pit safety bubble around it of radius rb
* step 2:
  Set all points inside bubble to distance 0. This can be achieved by computing the length of the arc and than determine which points falls into the radius of buble rb. All of these points are than set to 0.
* step 3:
  Find maximum sequence of consecutive non-zeros among the free-space points. This is the maximum gap where the car can drive.
* step 4:
  Find the best point among this maximum sequence from previous step.

To make this as fast as possible, I should be looking from farther ranges for these obstacles, because doing sharp turns results in slower velocity, which can be achieved by turning earlier in less sharp angle.

#### Disadvantages
There is risk of doing U turns if he car is going too fast and it has to do 90 degrees turn which it can't currently see as at particular point it will see the point on the opposite side of track, rather than the turn itself. This is shown on the figure bellow for better intuition.
<img src="/assets/ftg_fail.png" height="500">

It is also shown in the nice follow the gap practical demonstration in video bellow.
<iframe width="700" height="400" src="https://www.youtube.com/embed/ctTJHueaTcY" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



## F1Tenth Lab4 assignment
My implementation of FtG algorithm consists of these steps:
 1. Process each LiDAR scan by considering only field of view of 90 (45 to left and 45 to right as the 0th degree is exactly infront of the car) degrees, setting each value to the running mean of window 7 by convolution and clipping all values over 3m to 3m
 2. Find the closest obstacle and set all points in bubble around it in certain radius 0. This bubble is computed by computing arc of proportion of radius and closest point and computing all angles inside this bubble. Ranges corresponding to the indexes of angles which are in the bubble are than set to 0.
 3.  Finding the max gap by finding the longest non-zero consecutive values in the indexes of the processed lidar scan
 4.  Finding the deepest possible scan and steer in the direction of it.


<iframe width="800" height="400" src="https://www.youtube.com/embed/pKxiRvM4X6U" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
