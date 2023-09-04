---
title:  "Building a game controller and airmouse using accelerometer"
mathjax: true
layout: post
categories: media
---

My previous jumprope game has been a really fun and complicated project, I want to advance even more into the realm of interactability with my creations,
using more inputs that can be entered by the user. If you have read the introduction of myself in my blog post, you may remember that I enjoy videogames. I believe creating a videogame controller
follows this theme of user interactability and having the ability to play games with. I also believe this project could be a really good
segway into scripting and automation. These skills could be valuable in terms of productivity and job prospects as well as opening the door for 
genuinely helpful projects.

![Screenshot_20230904_161129_com huawei himovie overseas](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/ca13a519-6418-4d65-87ca-999774b6d8e4)


The idea here is to build a game controller that behaves as a keyboard and mouse. Initially, this controller was meant to be 
used to play Valorant as a multiple person team. One person controlling the mouse movement / buttons and the other controlling all other inputs. Most videogames though,
can be played as long as someone has a keyboard and mouse, so even though this code is meant for Valorant, really almost any game can be played,
especially by editing some key references in the code. Anyways, I wanted to have some silly input methods because it would be more entertaining to use. To do this,
the MPU-6050 will be used. It is a 6-axis sensor, meaning it can make 2 types of measurements (acceleration and gyro) on the X, Y, and Z axis. Gyro measurements are calculated by the rate of change in the orientation of the sensor while the
acceleration measures the force of gravity on the 3 dimensions. For this project, we will use acceleration measurements since it supports holding the sensor still at certain positions to turn while a gyro measurement requires constant orientation changing to turn. It is basically impossible for a human to
move their mouse with any amount of control with the gyro sensor. Thus, acceleration will be used. For the keyboard inputs, buttons and 
a joystick are used. The joystick is essentially a ball that can be rotated along the axis of 2 potentiometers. By using these potentiometer
readings, you can find the location that the user has moved the joystick by using 2 analogReads. To translate the user input to the keyboard and mouse, we will use a
SparkFun Pro Micro clone. The Pro Micro has an atmega32u4 which supports the Keyboard.h and Mouse.h libraries. While other arduino products can also perform these inputs with more roundabout ways such as using a bit of Python scripting,
I thought this would be a good time to use a Pro Micro for the first time as it has the smallest form factor I have seen of any Arduino equivalent and useful for simpler projects.

![JoystickMpu](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/b15800a5-acaa-476c-95bb-4e090a1920ef)

<sub>^Mpu6050 and Joystick</sub>

The components used for this project are listed below:

| Materials Needed:      |
| ----------- |
| Arduino microcontroller (Arduino Uno, Arduino Nano, etc.)      |
| MPU-6050 gyroscope and accelerometer module   |
| Joystick module   |
| Pushbuttons (for keybinds)   |
| Jumper wires   |
| Breadboard (optional, for prototyping)  |
| Micro USB data cable  |
| Computer with Arduino IDE installed  |

Using basic pinout details for each of the MPU-6050, joystick, and pushbuttons, the circuitry of the project is quite simple and most people should be able to figure it out by looking into the code.
I used a breadboard to construct this. The most difficult part of the circuitry is to wire and orient everything in an ergonomic fashion, as the MPU-6050 must be able to be held and not be constantly pulled by
wires. Figuring out where you want your buttons could also be a bit challenging.

### Future Adjustments:
 There are a couple of future adjustments that I may think about including in the future. Firstly, the acceleration readings of the MPU-6050 is not always the best way to move the mouse. There are other types of measurements that can be used for the mouse. To me, the first that comes to mind is the magnetometer, which is essentially a compass. Having vertical translation being the pitch of the MPU-6050 orientation is fine but having horizontal translation as the roll of the sensor is not as intuitive as the yaw. The problem with yaw is that the MPU-6050 physically cannot calculate or measure yaw but the 9-axis MPU-9250 with a magnetometer can find the magnetic poles of the Earth to make a measurement that can be translated into yaw. 
 
 ![PitchRollYaw](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/77172fa7-a8e7-477f-9cec-bdcbdb2606f8)

 <sub>^ Pitch, Roll, and Yaw</sub>

Another adjustment that can be done is the joystick movement calculation. For now, the code is only capable of  moving in 8 directions. Whenever the joysick is moved in a diagonal direction past the thesholds of movement in the code, the keyboard input is always alternating keys. This means the movements can only be in increments of 45 degrees. The code can be changed to be more accurate. By comparing the ratio of the 2 potentiometer albsolute readings from the central deadzone, we can calculate the slope that we want the user's character to move at and use this slope to decide the rate at which different keys are pressed to move at an angle that reflects the expected input more accurately.

