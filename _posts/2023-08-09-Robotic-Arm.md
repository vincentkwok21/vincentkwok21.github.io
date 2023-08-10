---
title:  "Creating a Robotic Arm Prototype"
mathjax: true
layout: post
categories: media
---


I have done a lot of projects that pertain to very electronic related things that do not have moving parts. I would like to have a project
that is capable of some type of movement and customizability. A robotic arm seems like a pretty simple idea. In essence, you must be able to control
a number servo motors at the same time with the arduino, connecting each servo together with the material of the arm. 


For this project, I opted to use popsicle sticks as they
are very cheap and wood, which is a useful building material in terms of being able to be used for many purposes. Almost any type of glue can be used as adhesive for wood, it is relatively light and
durable yet it is flexible. I also created a more civil engineering related project pertaining to a bridge building competition in the past which used elmers glue and popsicle sticks. This previous project opened my eyes to prototyping in wood and glue,
as it forces an engineer to deal with more physics related issues such as directions of force, stress, and sub optimal materials. These problems may not be as evident as prototyping in metal or 3d plastics which has it own pros and cons.

So my process of creating this arm was quite simple an I will list the process. First, I realized that the jump from being able to control multiple servos at the same time with
a solid degree of accuracy to making a pilotable arm is not much. So I focused on this problem initially. I found basic example code of how to contol a single servo with a potentiometer which was fairly easy to do. I planned to create an arm with 3 servos that control orientation as well as another servo
that controls the grabbing mechanism. This following idea required me to purchase potentiometers to control each servo with a level of tactility for the human hand that I did not have in a potentiometer. In additona
, I purchased additional servos and wires which I could strip and cut to any distance of my choosing in order to avoid any problems with the physical construction parameters of the arm.

After using these supplies, I constantly ran into a problem of the servos seeming to jitter. I realized this problem occured due to the 5V port of the arduino not supplying enough current suffucient enought to lead into each
analog pin that would read the status of the potentiometers and the 4 servos which consume a pretty consdierable amount of current. I spent a very long time understanding that this was a problem as I had actually never encountered an issue like this.
In my labs in class, I would always be given the correct amount of voltage or current needed for me to set in a function generator or DC power supply to get the results that should occur. I have only calculated power/current related problems but have never actually seen one in real life. Because of this,
I basically tried to figure out if my current problem was any other type of problem before realizing I just needed more power/current. I tested my arduino, my connections, the continuity of certain paths, I looked into the documentation of the servo, searched internet forums, changed the device powering the arduino, reexamined the original code and more until actually figuring out the problem.

This realization is shown in the schematic below, where you can see a 6V battery created with 4 AA batteries in series. By keeping the ground of the battery and the arduino connected, I can establish a common ground despite using different voltage/power sources for different sections of the schematic.
I figured the Analog pins could only read data if it can in someway, be related to the intial 5V or 3.3V pins of the Arduino and its own ground. I am not sure if this assumption is true,
but since I can avoid making this assumption, I decided to do that a minimize the risk of anything going wrong with voltage supplies. So I connect the potentiometers to the 5V supply from the Arduino. Since the 5V did not seem like it was enought to power the 4 servos consistently, I decided to continue raising the voltage source strength until all motors functioned which thurned out to be around 6V.
This voltage also is not led into the analog pins, which probably makes the current that is going into each servo be closer to the desirable amount.

![Roboticarm schematic](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/c69c501e-12e4-49ad-95f3-e6078866155f)
<sub> schematic, does not include construction details</sub>

So after this process of figuring out the power supply issues, I finally created the 4 servos working in unison:

The next step was much less technical though still interesting, I created the arm with a hot glue gun, which worked very well to anchor each servo to the popsicle stick wood. This method worked for basically every connection of the arm except the base. Due to the length of the arm from the center of mass, 
I found it was almost impossible to use hot glue to anchor it down. I would eventually opt to use ductape as a temporary solution though I really can only see this problem being fixed if I recreate te arm in a stronger, more anchorable material such as metal. I also wanted the arm to
be aligned so each servo could form a straight line. What I realized is that due to the ability for this particular servo to rotate 180 degress, having more than a single joint connected by a beam will cause the servos to be able to swing parts of the arm into each other, potentially enabling the arm to destroy its own structure assuming they are strong enough
This realization caused me to delve into the code parameters, to change the ranges of motion. This took longer than expected as I wanted to make sure the arm could never self-destruct no matter what input the user performed. And I made sure that these parameters will applied for massive voltage surges, which I simulated by 
intentionally making poor or missing connections that should hasve existed in my schematic. I also found an interesting problem in what the servo considers being at "0" degrees vesus "180" degrees. It is easy to find the limit of the range of motion since without power, the servo can be turned to its limits of range of motion.
But which side of the rotational limit is considered as 0 and which is 180? This problem was quite easy for me to test for though simple trial and error. I observed that the servo is asymetical at certain orientations and by relating clockwise or counter clockwise rotation to this asymeticality, you can tell which side is 0 and which is 180 degrees. Since the horn of the servo must be attached by a user and can be attached orignating at any rotational point on the servo, I could not use the direction of the horn
to determine the status of the servo. These little details caused the process of carefully changing my parameters to also be slower than expected. Not only was the self-destruction of the arm a problem, but also the potential daamage to the grabbing mechanism, which has only 40 degrees
of freedom in which the mechanism is guaranteed not to damage itself out of the potential 180.

![Img0226-1920x1280](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/cb4dfd0a-b1a6-4ec5-8b65-47dd26bd8ff3)
<sub>SG90 Servo, notice the symmeticality of where the horn must originate </sub>
]
After these adjustments and the construction process, I wired the circuit together using 2 perf boards, 1 for powering the potentiometers and the other for powering the servos. Combining these with the code, schematic and the consideration that I included in this report and this project is highly recreatable by other people.
I will include a video of the project below and perhaps an edited version of the arm constuction in the future.


{% include embed.html url="https://www.youtube.com/embed/16KMgaprCCc" %}
<sub> longer explanation of arm, schematic, and code </sub>
