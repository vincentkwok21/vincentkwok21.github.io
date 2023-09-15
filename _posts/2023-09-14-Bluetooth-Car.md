---
title:  "Constructing a Bluetooth Robotic Car with Directable Water Pump System"
mathjax: true
layout: post
categories: media
---

As this is my final project of the summer, I want to incorporate aspects that represent the culmination of what I have learned in terms of problem solving, circuitry, and embedded systems programming. In pursuit of a project that stands out from the norm and showcases my creativity and technical skills, I embarked on the endeavor to design and construct a "Fire Fighting Vehicle." This project even on the surface level has several complex components that should require a good amount ingenuity and creative workarounds.

At its core, the fire fighting vehicle is the combination of a robotic arm and a driving motors to control the direction of the vehicle. These two ideas on the surface are quite simple, as I have already explored building a robotic arm previously and simply using a motor should be no challenge. Where this project may fall into complexity is the process of wireless signal transfer and communication. Before this project, I was unaware of how to implement this in any capacity or form. The process of researching and finding strategies to create bluetooth signals that can be controlled is an obvioius conflict point. Another point that should be noted is the appearance of problems that arise as a project becomes more complex. Even in the jump rope project which had many situations where code complexity created interesting workarounds, it is apparent that the robotic car will certainly have problems derived from complexity of the system, especially since hardware wise, this project will be more complicated than any previous project I have done.

## Choice of Mechanisms/Hardware:
  The first issue that must always be tackled before every project is the criteria that must be met with the selected hardware. The criteria I have chosen is as follows:
  | Criteria:      |
| ----------- |
| Turn and move forward/backward |
| Has Water pump functionality (no water leakage)  |
| Stable frame |
| Must have wireless communcation system that can be controlled by user |
| Uses robotic arm to direct water  |

Most of these criteria are quite simple to figure out though I would like to discuss a few criteria that influenced my planning of the project.

## Stable Frame
First, the requirement of a stable frame probably implies that the vehicle must have at least 3 points touching the ground. Having 2 points would be challenging as the vehicle would have to be able to move while centering its mass and carrying/consuming water. This is not a reasonable idea for hobby electronics in my opinion since the motors that may be required to balance a liquid mass would have to be incredibly well made. I decided to settle for the least energy consuming option, a 2 wheeled vehicle with a single frontal pivot point. While 4+ wheeled vehicles are more comprehensible, the power supply required may be too much and the space of motors + batteries could be too much. This 3 point decision would eventually turn out to create some problems in the future though there is no guarantee any other wheel decision would be any better.


## Wireless Communication
Next is the wireless communication system. Ideally, communicating to a microcontroller using a small device would be great but how can I? I also have the game controller system I created in the last blog post which may be able to combine with this project. I found 2 good ways to perform wireless communication. Perhaps the easiest way for me to add bluetooth compatibility is to use the [HC-04 Bluetooth Module] (https://www.smart-prototyping.com/HC-04-Bluetooth-Module-SPP2_1-BLE4_0). Using this module on 2 different arduino microcontrollers allows one module to send signals to another. I thought this strategy could be a bit too easy, since I just needed to purchase and attach modules and copy-paste the respective code. I did a bit more searching and found a lot of postive testimonals towards the ESP-32 microcontroller. While this controller is a little less friendly than the Arduino, it still uses the same language but likely uses completely different libraries. It also has built-in Wifi and Bluetooth connectivity, dual core processing, low power modes, and significantly more memory. It is also surpirisingly cheap ($3). I decided to use the ESP-32 to do this project. This led to a large amount of testing and research to solidify my understanding of the microcontroller.

## Bluetooth and App Development

## Unstable Servos

## Turning Problem
