---
title:  "Constructing a Bluetooth Robotic Car with Directable Water Pump System"
mathjax: true
layout: post
categories: media
---

As this is my final project of the summer, I want to incorporate aspects that represent the culmination of what I have learned in terms of problem solving, circuitry, and embedded systems programming. In pursuit of a project that stands out from the norm and showcases my creativity and technical skills, I embarked on the endeavor to design and construct a "Fire Fighting Vehicle." This project even on the surface level has several complex components that should require a good amount ingenuity and creative workarounds.

![IMG_20230915_002505](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/dff18685-a617-455d-9dee-1e237d0e9014)

<sub>^ Completed Robotic Car </sub>



![IMG_20230915_002553](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/4ce3b496-166a-4b67-9284-b07a32e42cd8)

<sub>^ Car underside </sub>

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

#### Stable Frame:
First, the requirement of a stable frame probably implies that the vehicle must have at least 3 points touching the ground. Having 2 points would be challenging as the vehicle would have to be able to move while centering its mass and carrying/consuming water. This is not a reasonable idea for hobby electronics in my opinion since the motors that may be required to balance a liquid mass would have to be incredibly well made. I decided to settle for the least energy consuming option, a 2 wheeled vehicle with a single frontal pivot point. While 4+ wheeled vehicles are more comprehensible, the power supply required may be too much and the space of motors + batteries could be too much. This 3 point decision would eventually turn out to create some problems in the future though there is no guarantee any other wheel decision would be any better.


#### Wireless Communication:
Next is the wireless communication system. Ideally, communicating to a microcontroller using a small device would be great but how can I? I also have the game controller system I created in the last blog post which may be able to combine with this project. I found 2 good ways to perform wireless communication. Perhaps the easiest way for me to add bluetooth compatibility is to use the [HC-04 Bluetooth Module](https://www.smart-prototyping.com/HC-04-Bluetooth-Module-SPP2_1-BLE4_0). Using this module on 2 different arduino microcontrollers allows one module to send signals to another. I thought this strategy could be a bit too easy, since I just needed to purchase and attach modules and copy-paste the respective code. I did a bit more searching and found a lot of postive testimonals towards the ESP-32 microcontroller. While this controller is a little less friendly than the Arduino, it still uses the same language but likely uses completely different libraries. It also has built-in Wifi and Bluetooth connectivity, dual core processing, low power modes, and significantly more memory. It is also surpirisingly cheap ($3). I decided to use the ESP-32 to do this project. This led to a large amount of testing and research to solidify my understanding of the microcontroller.

## Bluetooth and App Development:
After deciding on using the ESP-32, I had to research the feasibility of a bluetooth system. While digging around for quick ways to transmit bluetooth signals, I found [this](https://ai2.appinventor.mit.edu/) website developed by MIT. It takes a simple approach to app development using scratch-like block programming and object creation with simple point and click UI customization to make it very easy and quick to create a phone app that can communicate wirelessly. [This](https://youtu.be/aM2ktMKAunw?si=7lePet71PU4cjZV3) youtube video taught me the general structure of how to program and transfer information between Android phone and ESP32. After some experimentation with sliders and troubleshooting regarding maximum byte sizes, it was much easier to create the app and make it functional than expected.

![MITAPPINVENTOR](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/2d8dca53-44fc-4e3c-a5b5-19429a5c4295)

<sub> ^ MIT app inventor programming screenshot </sub>
## Unstable Servos:
After creating the application, I verified the functionality of each hardware module. I noticed there was a lack of solid documentation of pin naming for the ESP-32 in the Arduino IDE. There were several different visuals I found that referred to pins in the same position as different names. I decided to manually verify the pins with a blink sketch and running through several pins for each visual. I found the visual below to match with the pinout of the ESP-32.

![ESP-32Pinout](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/10412b96-683e-4f7b-af77-1056950bf03c)

<sub> ^ Pinout of ESP-32</sub>

Afterwards I verified that the servos, pump, and gear motors all performed expectedly. Other than the servo library requiring an alternative for the ESP-32, the were no major problems. I created the below schematic for the car. The original schematic though had no flyback diodes.

![Espcar](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/ceedc2ba-0073-416c-b1c4-8ae4d1905021)
 <sub> ^ Final Bluetooth Car Schematic</sub>
 
 This oversight caused the motors of the servos to be forced into 0 degrees as long as the pump was turned on, which also would inconsistently toggle on and off. These problem caused me to look into the details of how a relay functions. I eventually figured out this was a culmination of 2 different problems. Firstly, my relay nomial voltage too high for be connected to the 3.3V of the ESP. Relays contain a coil that must be charged in order to cause the switch to change. While I did see the switch LED flicker and a small noise register when the voltage changed, I realized by the behavior of the relay that the switch was not actually functioning at all. I eventually found a relay with a more suitable nominal voltage. This fix causes the pump to activate very consistently though the servo motor problem still persisted. 

![IMG_20230910_225047](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/0030a41d-fe57-494e-906a-680c1c769fcc)

<sub> ^ Electronics before flyback and relay fix </sub>

After asking a few internet forums about my problem I recieved many pieces of advice. Including using capacitors to stabilize the pwm signal and pump, soldering all connections together instead of using breadboard, using better quality servos, selecting different power sources, adding flyback diodes, etc. I tested the easiest fixes first, finding that the flyback diodes stop the servos from reverting into the 0 degree position. After this fix, each individual piece of the car was functional without interrupting the others.

## Turning Problem
After constructing the vehicle with a glue gun to attach components to a wooden platform, I found there was not enough space for a caster wheel to glue at the front of the platform. I decided to find alternative ways to create a low friction frontal pivot point to minimize the resistance of the vehicle to its torque. I began by constructing wheels out of lego in an attempt to create a wheel system that took less space. This attempt was successful for forward movement though I found that turning was impossible due to the grip of the wheels despite their narrowness. Even after reducing the size of the wheels to increase the force applied to the body and taking away horizontal grip by pulling off the tires, the build still failed to turn. Realizing that I needed to find a method to minimize the overall directional friction of the frontal pivot point, I found a [table of frictional coefficients](https://www.engineeringtoolbox.com/friction-coefficients-d_778.html) for various materials. I found that plastics and ice were very good choices for a pivot point, of course, making a mold to attach ice at a fixed location is quite challenging but I realized that wet materials often have a lower friction coefficient than their dry variation. To make the final iteration of the pivot point, I found a hollow, smooth, dome-like
lego piece and froze water using it as a mold. The outcome was an extremely slippery pivot point in all directions at room temperature as a result of the condensation of melting ice.

![IMG_20230915_002723](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/d9a97a48-c2cc-43d1-aabb-81580ccca820)
<sub> ^ Dome pivot point </sub>

 Obviously, there is a safety drawback of using water in an electrical system and melting ice will eventually cause the condensation to run out but it works very well!. Another way to fix the turning problem is to increase the torque applied to the wheels by using new motors  or a higher supply voltage, though this would cause a rebuilding of the fundamental structure.

The final code and files are attached below:
[code](https://github.com/vincentkwok21/ESP32_Car/blob/main/Esp32BluetoothCar.ino)
[app apk](https://github.com/vincentkwok21/ESP32_Car/blob/main/Robotic_Car.apk)

{% include embed.html url="https://www.youtube.com/embed/HMhpJwU3rPM" %}

