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

* [Topics](http://wiki.ros.org/Topics): These are the channels facilitating message exchange among nodes. Nodes can either subscribe to or publish on a topic. Given their many-to-many relationship, a topic can have multiple publishers and subscribers, but only one type of message.
* [Messages](http://wiki.ros.org/Messages): These are strongly-typed data structures designated for a topic.
* [Packages](http://wiki.ros.org/Packages): They constitute a part of the software that may contain one or more nodes.

The interplay of ROS Publishers, Subscribers, Messages, and Topics is beautifully articulated at [mathworks.com](https://www.mathworks.com/help/ros/ug/exchange-data-with-ros-publishers-and-subscribers.html), elucidated with the following illustration:

![mathworks](https://www.mathworks.com/help/examples/ros/win64/ExchangeDataWithROSPublishersAndSubscribersExample_01.png)

Next on my agenda was the turtlesim tutorial, aptly referred to as the "Hello World" of ROS. You can find the tutorial at the [ROS wiki](http://wiki.ros.org/turtlesim/Tutorials).

Subsequent to this, I proceeded to the lecture on ROS filesystem. While this topic is indispensable for effective work in ROS, it's best understood through hands-on practice within turtlesim or your own ROS project. So, what exactly is [ROS filesystem](http://wiki.ros.org/ROS/Tutorials/NavigatingTheFilesystem)? It streamlines the build process of a project while furnishing the necessary tools and flexibility to decentralize its dependencies. The crux of the lecture hinged on [catkin](http://wiki.ros.org/catkin), the [cakin_make](http://wiki.ros.org/catkin/commands/catkin_make) command, and importantly, the [package.xml](http://wiki.ros.org/catkin/package.xml) and [CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt) files within the ROS package.

* [Catkin](http://wiki.ros.org/catkin): This is ROS's build system responsible for generating executables and libraries. It is an extension of CMake from the C programming language, augmented with ROS-specific features. Catkin lends more standard compliance to the package, enhancing its reusability among programmers. The figure below depicts the organization of the catkin workspace (sourced from [UV F1/10 lecture](https://linklab-uva.github.io/autonomousracing/assets/files/L03.pdf)).
<img src="/assets/catkin_ws.png">
* [Package.xml](http://wiki.ros.org/catkin/package.xml): This file houses the meta-information of a package such as its name, description, version, license, and dependencies.
* [CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt): As the main CMake file, it is instrumental in building the package and invoking catkin-specific functions. Below is an exemplification of how a rudimentary CMakeLists.txt should appear (sourced from [UV F1/10 lecture](https://linklab-uva.github.io/autonomousracing/assets/files/L03.pdf)):
CMakeLists.txt
![cmake](/assets/CMake.png)

I then transitioned to the subsequent video and Lecture 3 from the University of Virginia, highlighting one of the pivotal concepts in ROS: Publishers and Subscribers. This lecture opens with an illustration of the <a href="http://wiki.ros.org/rospy">rospy client library</a>, elucidating the initialization of a ROS Node with:

```shell
rospy.init_node('my_node_name')
```

and

```shell
rospy.init_node('my_node_name', anonymous=True)
```


The former informs ROS that this code is a ROS node named 'my_node_name', which must be unique. The latter parameter, anonymous=True, creates a unique name by appending a unique id to the node name. To prevent the node from being terminated immediately after a single run, we use:


```shell
rospy.spin()
```

This keeps the node active by utilizing some CPU cycles.

#### The Publisher
A [ROS Publisher](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29) broadcasts a message to a particular topic. The syntax of rospy's publisher is as follows:

![publisher](/assets/publisher.png)
Example of Publisher Node with explanation of what each line is doing, ref: [UV F1/10 lecture](https://linklab-uva.github.io/autonomousracing/assets/files/L04-compressed.pdf)

```python
pub = rospy.Publisher('topic_name', message_type, queue_size=size)
```


#### The Subscriber
A [ROS Subscriber](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28c%2B%2B%29) listens to the messages on a topic. Here is the syntax for rospy's subscriber:
![subscriber](/assets/subscriber.png)
  
```python
sub = rospy.Subscriber('topic_name', message_type, callback_function)
```

The callback function is executed every time a message is received on the topic. This function typically processes the data and carries out the necessary actions.

Example of Subscriber Node with explanation of what each line is doing, ref: UV F1/10 lecture
The next video is hands on ROS commands and exploring the workspace from terminal available here. It is great hands-on experience of all the lectures before and worth trying it and following along.

I delved deeper into the intricacies of [ROS Messages](http://wiki.ros.org/Messages). As I mentioned earlier, ROS messages are strongly typed, i.e., their structure is known and checked at compile time. ROS provides a lot of built-in message types for common data types like integers, floating-point numbers, strings, time and duration, etc., and complex data structures as well. But, you are free to define your own messages if the built-in ones don't suit your needs. ROS messages are defined in ".msg" files, and each file describes a single message. These ".msg" files are located inside the "msg" directory of a package. Once these files are created or modified, we have to modify the package.xml and CMakeLists.txt files and build the package using catkin_make.

The syntax of a ".msg" file is very simple. It is a text file where each line contains a type and a variable name, separated by a space. For example, the definition of a [Twist message](http://docs.ros.org/en/api/geometry_msgs/html/msg/Twist.html), which is frequently used to command robot velocities, is:

```shell
geometry_msgs/Vector3 linear
geometry_msgs/Vector3 angular
```

This message contains two fields: linear and angular, and each field is of type Vector3 (which itself is another message defined in geometry_msgs package).

The hands-on part of this lecture involved a couple of exercises where I had to write a publisher and a subscriber node in Python. The exercises were quite detailed and practical and required me to go back and forth between the ROS documentation, lecture videos, and the actual code. It was an excellent exercise to get comfortable with the ROS programming environment and tools.

I wrapped up this week by watching a couple of additional videos on ROS services and parameters, but I am yet to practice them. I hope to cover those topics in the upcoming week. In a nutshell, a [ROS Service](http://wiki.ros.org/Services) is another way for nodes to communicate with each other. It is a synchronous method, where one node sends a request to another, and the other node sends a response. A [ROS Parameter](http://wiki.ros.org/Parameter%20Server) is a way to store data that is shared among nodes and is not expected to change often. Parameters are stored on a Parameter Server and can be set, get, and deleted using ROS command line tools or programmatically using rospy or roscpp.

On a final note, I would like to say that this week was quite intense but also very satisfying. The learning curve is indeed steep, but the hands-on approach is very effective and engaging. I look forward to the upcoming weeks of this journey.


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
