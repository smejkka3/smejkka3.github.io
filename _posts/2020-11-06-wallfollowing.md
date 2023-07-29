---
layout: post
title:  "Taking The Brake: The Art of Automatic Emergency Braking"
excerpt: "Navigating the intriguing journey of bringing a vehicle to a halt, all by itself, before it kisses an obstacle."
---

## Introduction

Having recently delved into the fascinating world of ROS, the next challenge in line was to devise a node that harnesses `laser_scan` data to put a leash on a car that's all set to crash into an unsuspecting object. This necessitated mastering the Time To Collision (TTC) calculation, using the aforementioned LaserScan message, to predict any upcoming catastrophe. This task pushed me further into the labyrinth of publishers and subscribers, while also nudging me to unravel the secrets of the `Odometry` message and `AckermannDriveStamped` message. But before we plunge into the deep end, let's paddle through some theory.

## AEB (Automatic Emergency Braking)

Imagine a vigilant guardian angel that takes the wheel to halt your vehicle just before it slams into an object. That's AEB for you! It is a safety net that operates based on the data fed by the vehicle's sensors. At its core, AEB is a binary classifier that is faced with a life-saving decision: to brake or not to brake.

#### Slip-ups

The Achilles' heel of AEB is false negatives: situations where the car doesn't hit the brakes when it's seconds away from a collision. It's not hard to see why this is a no-go for a road-ready car. Then, there are false positives, which are less detrimental but still annoying. This is when your car turns overprotective and hits the brakes when there's no threat in sight.

#### TTC (Time to Collision)

  Now, let's dive into the nitty-gritty of TTC calculation:

  ![Time to Collision formula from lecture 2 of F1/10 course](/assets/TTC_formula.png)

  The denominator in the equation above, known as the "range rate," is the time derivative of the range between the vehicle and a given obstacle:

  ![Formula for range-rate from lecture 2 of F1/10 course](/assets/range_rate.png)


#### Lidar

Even though Lidar and I aren't strangers, I'll walk you through the [LaserScan message data fields](http://docs.ros.org/en/api/sensor_msgs/html/msg/LaserScan.htmlLaserScan) that are crucial for this task. Given the TTC formula, I need to employ the <i>float32[] ranges</i> to estimate the distance between the obstacle and the car (which is our numerator). For the denominator (range-rate), I'll have to engage <i>float32 angle_min, float32 angle_max</i>, and <i>float32 angle_increment</i>. This is because the range-rate calculation requires the cosine of each beam angle. In scenarios where the range-rate of a specific beam is less than zero, I set it to zero to avoid division by zero. I also explored the possibility of eliminating the zeros from the denominator and the corresponding numerator (at the same LaserScan angle), but this led to a surge in false negatives.

Another vital message is the <i>Odometry</i>, specifically <i>geometry_msgs/TwistWithCovariance twist.linear.x</i>, which represents the linear speed of the car and is used in the range-rate computation.

## F1Tenth Lab2 assignment

After a series of trial and error in the RViz simulator, I found the sweet spot for the TTC threshold for braking: 0.3. The moment the TTC slips below this threshold, the brake is applied, making for some dramatic last-minute rescues.

<iframe width="800" height="400" src="https://www.youtube.com/embed/LXWpBoFb4nk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<iframe width="800" height="400" src="https://www.youtube.com/embed/zna-dPAIdUQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<p>
</p>
