---
title:  "Creating a customizable jump rope game with a VL53l0X sensor"
mathjax: true
layout: post
categories: media
---

My previous jumprope game has been a really fun and complicated project, I want to advance even more into the realm of interactability with my creations,
using more inputs that can be entered by the user. If you have read the introduction of myself in my blog post, you may remember that I enjoy videogames. I believe creating a videogame controller
follows this theme of user interactability and having the ability to play games with. I also believe this project could be a really good
segway into scripting and automation. These skills could be valuable in terms of productivity and job prospects as well as opening the door for 
genuinely helpful projects.

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
