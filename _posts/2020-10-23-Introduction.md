---
layout: post
title:  "Delving into the Robotic Operating System (ROS)"
excerpt: "Embarking on the F1Tenth journey: An encompassing introduction to ROS."
---

## An Aerial View

Initiating my journey with F1Tenth, the preliminary lecture provides an overarching perspective of the entire course along with a concise introduction to autonomous racing. The essence of this lecture revolves around stirring up students' interest and laying down a roadmap of what awaits them throughout the course.

Post the inaugural lecture, my next stop was the first tutorial. The slides available on the website give a cursory glance at the F1Tenth simulator, its installation process, and the manual operation of the car within the simulator via a keyboard. To delve deeper, I turned to lecture videos by Prof. Madhur Behl from the University of Virginia, available [here](https://www.youtube.com/playlist?list=PL868twsx7OjddCq3az74hu6pVsuJJzXvP) and [here](https://linklab-uva.github.io/autonomousracing/). I devoted my time to the first five videos up until [F1/10 Lectures] Online ROS F1/10 Simulator, to master the rudiments of ROS, a prerequisite for Lab1 Assignment. Below, I have enumerated some salient points from these lectures that merit recollection.

## Fundamentals of ROS

Although the [ROS wiki](http://wiki.ros.org/) is a treasure trove of tutorials for learning the basics of ROS, I will highlight the key takeaways from the initial lecture. The focus was primarily on ROS as a middleware, adept at managing communication among various segments of the autonomous car. Here are some fundamental components of ROS:

* [Nodes](http://wiki.ros.org/Nodes): These are programs designed for specific functionality. Operating as independent processes, nodes interact with each other using topics and messages. Nodes are written using the client library, with roscpp and rospy being the main ones for C++ and Python respectively. There also exist experimental client libraries such as R and Java.
* The [ROS Master](http://wiki.ros.org/Master) is a unique node that is always running and doesn't necessitate user-defined codes. It enables nodes to interchange messages. To launch the ROS Master Node, use the command:

```shell
>>> roscore
```

* [Topics](http://wiki.ros.org/Topics) are channels over which nodes exchange messages. Nodes can subscribe to or publish to a topic. Topics has many-to-many relationship, so there can be multiple publishers and multiple subscribers on one topic and each topic has only one type of message.
* [Messages](http://wiki.ros.org/Messages) are strongly-typed data structures for a topic.
* [Packages](http://wiki.ros.org/Packages) are part of software, which contains one or more nodes.

The ROS Publishers and Subscribers, Messages and topic concept is very well described at [mathworks.com](https://www.mathworks.com/help/ros/ug/exchange-data-with-ros-publishers-and-subscribers.html) with the following image:

![mathworks](https://www.mathworks.com/help/examples/ros/win64/ExchangeDataWithROSPublishersAndSubscribersExample_01.png)


The next thing to try for me was to follow the turtlesim tutorial, which is basically "hello world" of ROS. The tutorial is available at [ROS wiki](http://wiki.ros.org/turtlesim/Tutorials).

The next lecture was about ROS filesystem. Although very important topic to know to successfully work in ROS, it is better to try it on your own inside turtlesim or your own ROS project. So what is [ROS filesystem](http://wiki.ros.org/ROS/Tutorials/NavigatingTheFilesystem)? It centralises the build process of a project, while at the same time provide enough flexibility and tooling to decentralise its dependencies. The main points of the lecture were about [catkin](http://wiki.ros.org/catkin), [cakin_make](http://wiki.ros.org/catkin/commands/catkin_make) command and most importantly about [package.xml](http://wiki.ros.org/catkin/package.xml) and [CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt) file inside ROS package.

* [Catkin](http://wiki.ros.org/catkin): Is the build system of ROS which generates executables nad libraries. It is based on CMake from C programming language and it extends it with ROS specific features. It also makes the package more standard compliant, and thus reusable by other programmers. In the following figure is shown how the catkin workspace should be organised (taken from [UV F1/10 lecture](https://linklab-uva.github.io/autonomousracing/assets/files/L03.pdf)).
<img src="/assets/catkin_ws.png">
* [Package.xml](http://wiki.ros.org/catkin/package.xml): Contains the meta information of a package such as name, description, version, license and dependencies.
* [CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt): The main CMake file to build the package and calls catkin-specific functions. Example of how should very basic  * CMakeLists.txt look look like is in the following figure (taken from [UV F1/10 lecture](https://linklab-uva.github.io/autonomousracing/assets/files/L03.pdf)):
CMakeLists.txt
![cmake](/assets/CMake.png")


Moving on to the next video and Lecture 3 from the University of Virginia, which talks about one of the most important concepts in ROS and that's Publishers and Subscribers. This lecture starts with example of <a href="http://wiki.ros.org/rospy">rospy client library</a>, explaining initialisation of ROS Node with

```shell
rospy.init_node('my_node_name')
```

and

```shell
rospy.init_node('my_node_name', anonymous=True)
```

This tells ROS that this code is ROS node with the name my_node_name and this name must be unique. The anonymous=True parameter create unique name for you adding unique id to the end of the node name. To not kill the node immediately after one run using
```shell
rospy.spin()
```
to achieve this by consuming some CPU cycles.


#### Publisher
[ROS Publisher](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29) is a node which will continually broadcast a message.
![publisher](/assets/publisher.png)
Example of Publisher Node with explanation of what each line is doing, ref: [UV F1/10 lecture](https://linklab-uva.github.io/autonomousracing/assets/files/L04-compressed.pdf)


#### Subscriber
[ROS Subscriber](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29) tells ROS that you want to receive messages on a given topic.
![subscriber](/assets/subscriber.png)
Example of Subscriber Node with explanation of what each line is doing, ref: UV F1/10 lecture
The next video is hands on ROS commands and exploring the workspace from terminal available here. It is great hands-on experience of all the lectures before and worth trying it and following along.

## Autoturtle assignment

Although I completed [Lab1 of F1Tenth](https://f1tenth-coursekit.readthedocs.io/en/stable/assignments/labs/lab1.html#lab-1-introduction-to-ros) already, I decided to do the [first assignment](https://linklab-uva.github.io/autonomousracing/assets/files/A01.pdf) from the F1/10 course from University of Virginia as well to gain very good base knowledge of ROS. There are 4 tasks in total. The first one is to create node swim_shool.py where the turtle from turtlesim tutorial draw figure 8 by doing movement specified by linear and angular velocity defined by user. I'm not uploading the code as it is university course credited assignment but my results are shown in gif bellow. My first idea was to create subscriber in the node which is getting the pose of the turtle and when the pose.theta is very close to 0 (means horizontal to X axis) multiply angular velocity of turtle by -1, which makes the turtle turn the other side to which is turtle currently rotating. However due the slight inaccuracies of the pose.theta and rospy.rate the [pose.theta](http://docs.ros.org/en/melodic/api/turtlesim/html/msg/Pose.html) never was exactly 0 or its absolute value was not close enough to accurately follow the 8 figure each round. I partially solved this by increasing queue size and rospy.rate to 1000hz to increase rate of loop which gave me more pose.theta values, however this solution still was not accurate enough. What worked great is to calculate the radius of the circle from the linear and angular velocities inputted by user. Using this radius I could calculate circumference of the circle the turtle is going to draw as well as at the same time computing the distance which turtle travelled at each run of the loop and once the turtle crossed the distance of circumference of the circle multiply angular velocity of turtle by -1 and combine this approach with the pose.theta checking (as the circumference check was not accurate enough and the turtle turned slightly before finishing the complete circle) mention above with slightly less accuracy. Combining these two condition made my turtle not skip the turns after finishing drawing the complete circle.

![swim_school1](/assets/swim_school1.gif)

![swim_school2](/assets/swim_school2.gif)

The second task random_swim_shool.py was very similar to the previous one. The only difference is that the initial position of the turtle should be random as well as linear and angular velocity of the turtle. My results shown again in gifs bellow.

![random_swim_school1](/assets/random_swim_school1.gif)

![random_swim_school2](/assets/random_swim_school2.gif)

In the third task back_to_sqaure_one.py the task was again to draw a shape by moving turtle, however this time it was square shape with the lower left corner at position (1,1). Side length of the square should be taken from user's input in the range between 1 to 5.
![back_to_sqaure_one](/assets/back_to_sqaure_one.gif)

The last problem was to swim to position defined by a user.
![swim_to_goal](/assets/swim_to_goal.gif)


## F1Tenth Lab1 assignment

[First Lab](https://f1tenth-coursekit.readthedocs.io/en/stable/assignments/labs/lab1.html#lab-1-introduction-to-ros) of the F1Tenth was quite straight forward. The main goal is:
* to understand directory structure and framework of ROS
* to understand and be able to implement simple subscribers and publishers
* to understand and be able to implement messages
* to understand what exactly is in CMakeLists.txt and package.xml
* to understand package dependencies
* be able to write and to understand launch files
* to work with RViz
* to understand and work Bag files.

The goal is to implement ROS package with node (either in C++, Python or both) and launch file and custom message, which subscribes to the laser_scan topic and publishes distance to the closest obstacle, farthest obstacle and both of these in one topic. I decided to do the lab in both C++ and Python as I want to practice C++ for later when it will be useful for computer vision as python is known for being quite slow. I didn't encounter some major problems, only experienced little delay while coding the C++ part as I had to iterate trough the vector of laser_scan message ranges and as it was already more than a year sice I did major project in C++ I had to go back to basic tutorials on iterators in C++. My final package as required in the task is available bellow.
