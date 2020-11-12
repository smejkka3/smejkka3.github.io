---
layout: post
title:  "Introduction to Robotic Operating System (ROS)"
excerpt: "My first steps into F1Tenth. Mostly just ROS introduction."
---
## Introduction

To start with F1Tenth, the first lecture covers mostly overview of the whole curse and a brief introduction of autonomous racing. The point of the lecture was mostly to motivate students and set an outline of what to expect during the course.

After watching the first lecture the next content in line was the first tutorial. The slides on the website only briefly cover the F1Tenth simulator how to install it and how to manually drive the car inside of the simulator using keyboard. So I used lecture videos from University of Virginia by Prof. Madhur Behl available <a href="https://www.youtube.com/playlist?list=PL868twsx7OjddCq3az74hu6pVsuJJzXvP">here</a> and <a href="https://linklab-uva.github.io/autonomousracing/">here</a> as well as watched the first 5 videos all the way up to [F1/10 Lectures] Online ROS F1/10 Simulator to cover all the basics of ROS, which are later needed in the Lab1 Assignment. I'm gonna list bellow some of the basic points I should remember from these lectures.

## ROS basics

The <a href="http://wiki.ros.org/">ROS wiki</a> already has great tutorials to use when learning basics of ROS, however I'm gonna mention some of the main points from the first lecture. It was focused on the ROS as middleware which manages communication between different parts of the autonomous car. Here are the some of the basics components of ROS:

* <a href="http://wiki.ros.org/Nodes">Nodes</a> Programs with specific functionality, that runs as a single process and they communicate with other nodes using topics and messages. A node is written using the client library, the main ones are roscpp for C++ and rospy for Python. There are also some experimental client libraries such as R and Java.
  * <a href="http://wiki.ros.org/Master">ROS Master</a> is special node which runs always and it doesn't have to be written by user. It allows nodes to be able to exchange message among each other. ROS Master always has to be running. To start the ROS Master Node there is a command
```shell
>>> roscore
```

* <a href="http://wiki.ros.org/Topics">Topics</a> are channels over which nodes exchange messages. Nodes can subscribe to or publish to a topic. Topics has many-to-many relationship, so there can be multiple publishers and multiple subscribers on one topic and each topic has only one type of message.
* <a href="http://wiki.ros.org/Messages">Messages</a> are strongly-typed data structures for a topic.
* <a href="http://wiki.ros.org/Packages">Packages</a> are part of software, which contains one or more nodes.

The ROS Publishers and Subscribers, Messages and topic concept is very well described at
<a href="https://www.mathworks.com/help/ros/ug/exchange-data-with-ros-publishers-and-subscribers.html"> mathworks.com</a> with the following image:

<img src="https://www.mathworks.com/help/examples/ros/win64/ExchangeDataWithROSPublishersAndSubscribersExample_01.png">


The next thing to try for me was to follow the turtlesim tutorial, which is basically "hello world" of ROS. The tutorial is available at <a href="http://wiki.ros.org/turtlesim/Tutorials">ROS wiki</a>.

The next lecture was about ROS filesystem. Although very important topic to know to successfully work in ROS, it is better to try it on your own inside turtlesim or your own ROS project. So what is <a href="http://wiki.ros.org/ROS/Tutorials/NavigatingTheFilesystem">ROS filesystem</a>? It centralises the build process of a project, while at the same time provide enough flexibility and tooling to decentralise its dependencies. The main points of the lecture were about <a href="http://wiki.ros.org/catkin">catkin</a>, <a href="http://wiki.ros.org/catkin/commands/catkin_make">cakin_make</a> command and most importantly about <a href="http://wiki.ros.org/catkin/package.xml">package.xml</a> and <a href="http://wiki.ros.org/catkin/CMakeLists.txt">CMakeLists.txt</a> file inside ROS package.

* <a href="http://wiki.ros.org/catkin">Catkin</a>: Is the build system of ROS which generates executables nad libraries. It is based on CMake from C programming language and it extends it with ROS specific features. It also makes the package more standard compliant, and thus reusable by other programmers. In the following figure is shown how the catkin workspace should be organised (taken from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/L03.pdf">UV F1/10 lecture</a>).
<img src="/assets/catkin_ws.png">
* <a href="http://wiki.ros.org/catkin/package.xml">Package.xml</a>: Contains the meta information of a package such as name, description, version, license and dependencies.
* <a href="http://wiki.ros.org/catkin/CMakeLists.txt">CMakeLists.txt</a>: The main CMake file to build the package and calls catkin-specific functions. Example of how should very basic  * CMakeLists.txt look look like is in the following figure (taken from <a href="https://linklab-uva.github.io/autonomousracing/assets/files/L03.pdf">UV F1/10 lecture</a>):
CMakeLists.txt
<img src="/assets/CMake.png">


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
<a href="http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29">ROS Publisher</a> is a node which will continually broadcast a message.
<img src="/assets/publisher.png">
Example of Publisher Node with explanation of what each line is doing, ref: <a href="https://linklab-uva.github.io/autonomousracing/assets/files/L04-compressed.pdf">UV F1/10 lecture</a>


#### Subscriber
<a href="http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29">ROS Subscriber</a> tells ROS that you want to receive messages on a given topic.
<img src="/assets/subscriber.png">
Example of Subscriber Node with explanation of what each line is doing, ref: UV F1/10 lecture
The next video is hands on ROS commands and exploring the workspace from terminal available here. It is great hands-on experience of all the lectures before and worth trying it and following along.

## Autoturtle assignment

Although I completed <a href="https://f1tenth-coursekit.readthedocs.io/en/stable/assignments/labs/lab1.html#lab-1-introduction-to-ros">Lab1 of F1Tenth</a> already, I decided to do the <a href="https://linklab-uva.github.io/autonomousracing/assets/files/A01.pdf">first assignment</a> from the F1/10 course from University of Virginia as well to gain very good base knowledge of ROS. There are 4 tasks in total. The first one is to create node swim_shool.py where the turtle from turtlesim tutorial draw figure 8 by doing movement specified by linear and angular velocity defined by user. I'm not uploading the code as it is university course credited assignment but my results are shown in gif bellow. My first idea was to create subscriber in the node which is getting the pose of the turtle and when the pose.theta is very close to 0 (means horizontal to X axis) multiply angular velocity of turtle by -1, which makes the turtle turn the other side to which is turtle currently rotating. However due the slight inaccuracies of the pose.theta and rospy.rate the <a href="http://docs.ros.org/en/melodic/api/turtlesim/html/msg/Pose.html"> pose.theta </a> never was exactly 0 or its absolute value was not close enough to accurately follow the 8 figure each round. I partially solved this by increasing queue size and rospy.rate to 1000hz to increase rate of loop which gave me more pose.theta values, however this solution still was not accurate enough. What worked great is to calculate the radius of the circle from the linear and angular velocities inputted by user. Using this radius I could calculate circumference of the circle the turtle is going to draw as well as at the same time computing the distance which turtle travelled at each run of the loop and once the turtle crossed the distance of circumference of the circle multiply angular velocity of turtle by -1 and combine this approach with the pose.theta checking (as the circumference check was not accurate enough and the turtle turned slightly before finishing the complete circle) mention above with slightly less accuracy. Combining these two condition made my turtle not skip the turns after finishing drawing the complete circle.

<img src="/assets/swim_school1.gif">

<img src="/assets/swim_school2.gif">

The second task random_swim_shool.py was very similar to the previous one. The only difference is that the initial position of the turtle should be random as well as linear and angular velocity of the turtle. My results shown again in gifs bellow.

<img src="/assets/random_swim_school1.gif">

<img src="/assets/random_swim_school2.gif">

In the third task back_to_sqaure_one.py the task was again to draw a shape by moving turtle, however this time it was square shape with the lower left corner at position (1,1). Side length of the square should be taken from user's input in the range between 1 to 5.
<img src="/assets/back_to_sqaure_one.gif">

The last problem was to swim to position defined by a user.
<img src="/assets/swim_to_goal.gif">


## F1Tenth Lab1 assignment

<a href="https://f1tenth-coursekit.readthedocs.io/en/stable/assignments/labs/lab1.html#lab-1-introduction-to-ros">First lab</a> of the F1Tenth was quite straight forward. The main goal is:
* to understand directory structure and framework of ROS
* to understand and be able to implement simple subscribers and publishers
* to understand and be able to implement messages
* to understand what exactly is in CMakeLists.txt and package.xml
* to understand package dependencies
* be able to write and to understand launch files
* to work with RViz
* to understand and work Bag files.

The goal is to implement ROS package with node (either in C++, Python or both) and launch file and custom message, which subscribes to the laser_scan topic and publishes distance to the closest obstacle, farthest obstacle and both of these in one topic. I decided to do the lab in both C++ and Python as I want to practice C++ for later when it will be useful for computer vision as python is known for being quite slow. I didn't encounter some major problems, only experienced little delay while coding the C++ part as I had to iterate trough the vector of laser_scan message ranges and as it was already more than a year sice I did major project in C++ I had to go back to basic tutorials on iterators in C++. My final package as required in the task is available bellow.

PDF with theory part of the assignment included in the <a href="https://github.com/smejkka3/smejkka3.github.io/raw/master/assets/karel_lab1.zip">zipfile</a>.
