---
title:  "Beer Dispenser Halloween Costume"
mathjax: true
layout: post
categories: media
---

WORK IN PROGRESS:
I got invited to a Halloween party and realized that I have to show up with a costume. It turns out Halloween costumes are the biggest
scams. So I decided to make my own since it was pretty short notice. Queue the most intense 6 hours of my life, where I put this costume together
and barely made it to the party on time.

Last year, my costume was "Beer Pong" this costume uses a cardboard slab as its main body with velcro attached in a cone shape like how cups 
are oriented in beer pong. The idea is that when someone wants to play beer pong, I would just lay on the ground and pour some beer in the cups. I think people really enjoyed this
concept because I ended up winning a costume contest that year! Anyways I want to create a costume of similar caliber of coolness so here it goes.

{% include embed.html url="https://www.youtube.com/embed/DbM7yC2fTYg" %}


The inital idea planning was pretty straightforward, with the my current skill set, I wanted to do something with moving parts that is semi interactive but not insanely fragile.
I ended up deciding between being a human sized simon game and a beer dispenser. If you are unfamiliar with Simon, it is basically a memory game. The device will sequentially turn
on one light of the four at a time. Each light is also a different color, Red, Blue, Green, or Yellow. When the game performs a random sequence, the user is then prompted to push 
buttons corresponding to the lights in the same sequence as the lights. If they fail to remember the sequence, they lose. If the perform the sequence correctly, the game will
then play the same sequence again but append one more light to the end of the sequence and continue this until the game is over. I thought the logic of this and the gamification
could be very interesting but what I realized is that putting a Simon game on myself would be a bit awkward since I was not sure how I would put
The problem with the human sized Simon game was that I would need quite at large button and I was not sure where to put it. LED strips could handle making the buttons easy to see
but its quite awkard to build around the human body unless you are really prepared to do so. I was not and did not have that amount of time to build. So I decided on a beer dispenser.
The concept of the beer dispenser is quite simple. Every part of this project is alredy something that I have done before. It includes a pump, a speaker, a led strip, an arduino and that is about it.
The most difficult part of this build was that I had to fit the entire project inside a backpack and put it in a way that does not compromise the circuit or the safety of the build. Since beer and electronics are being in
the same backpack with only a plastic container dividing them, the margin of error for this project is very low. In fact, this project is challenging because of my need to simplify the circuits as much as possible
to avoid points of conflict.

The required criteria for this project are as follows:

| Criteria:      |
| ----------- |
| Some type of liquid dispensing action |
| Buzzer tone output  |
| FastLed strip compatibility |
| Must be able to fit inside a backpack |
| Not prone to leakage  |

I have attached the schematic below: I believe that the schematic is qute self explanatory compared to my previous ones from other projects. Similar to my bluetooth car, this project
will require an external power source since I cannot just connect the arduino to the laptop while talking around with it as my Halloween costume. There are 2 battery packs, one responsible for the
pump with a flyback diode to prevent anything upexpected from happening, and another for the Arduino power. I am using the isolated button to activate the speaker to play a premade tone
that sounds like a police siren. If you want to see the code regarding this section and the rest of the circuit it can be found here.

There were several problems and investigations I ran while doing this project that really slowed me down. First was that I originally built this circuit with a relay which I found to be unnecessary.
Without the pump, the flyback diode by itself seems good enough to keep any noise flyback to be muted. This increased the simplicity of my circuit as well. The first major problem I had was with my LED strip. The first iteration of
this project does not have and LED strip since I was unable to implement it in time for the first event I was going to. I found that the circuit would work for a bit but after a few minutes of the arduino turning on, the light activity would
become very unpredictable, sometimes flashing out of phase or in the wrong interval of timeframe. It would also sometimes just stop working or turn white at a very high brightness. I did not want this to occur
in a party environment and flashbang the entire room so I decided to play it safe and not incorporate the light. I had a real struggle compressing the entire build in the backpack as there were initially many loose parts/ breadboards that I ended
up adhering to my beer container with hotglue. I had to place each component into the backpack very carefully as making any loose connections would force me to take everything back out
to diagnose the problem. This happened several times while trying to load the backpack which made me quite nervous for the party. I also realized I had to be able to trigger the siren and pump from outside the backpack. 
To do this, I used alligator clips with a button connecting them to trigger circuit connections for the pump and siren.
This turned out to be successful and still garner a lot of attention. In order to pump liquid, I placed the pump and tubing inside a plastic container with a lid. This lid must be loosely placed on the container as I must be able to open this lid to refill the container. Not only that, the pump requires power which needs to be wired to batteries. Since the pump is placed inside the container, I wired the pump through a hole drilled through the lid, connecting it to batteries. If the lid is turned too much, the wire will be pulled based on the location of the hole when it rotates. This design flaw could be fixed with longer wires or a better hole placement, but I never found the need to fix this. This is a potential problem though, as a loose lid combined with the second hole required to path tubing from the pump to the outside of the backpack makes the container practically open at all times. If the backpack is ever significantly off balance, the outcome would be terrible. Leakage of over 2 liters of liquids and the damage of the electronics would be terrible.  I have attached videos and images of
the build below. There was no leakage in the first dispenser iteration which was good.

![Circuit Schematic](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/713db6d3-5f2d-4480-9b1f-1003973929a2)

<sub>Circuit Schematic</sub>

{% include embed.html url="https://www.youtube.com/embed/yZJo5mE5fsI" %}



Before the second event, I wanted to incorporate the lights especially since there was a costume contest this time around. I realized that the current demand on the 6V AA battery source was probably
too much as the LED strip uses 150 diodes in series. Just some basic circuit knowledge tells me that diodes have a voltage drop and while it may not be as high as the 0.7V for a normal diode or 0.3 for a schottky, there
must still be a drop/load which is likely causing problems in my strip and Arduino. I decided to change my led segment from 150 leds to 50 leds which still did not fix the problem. At the start of this second iteration I also replaced all the
batteries used with what I thought to be better batteries. I initially used [dollar store batteries](https://www.dollartree.com/ecircuit-8pk-aa-shd/363460?traffic_source=google&traffic_medium=cpc&utm_content=gpla&gad_source=1&gclid=Cj0KCQjwqP2pBhDMARIsAJQ0CzroKCOagAig0VRgbrctMrRg17_oqPNvV1ybzMwLjDrYmj4LUZYmXx0aAq32EALw_wcB)
and switched to [kodak batteries.](https://www.dollartree.com/ecircuit-8pk-aa-shd/363460?traffic_source=google&traffic_medium=cpc&utm_content=gpla&gad_source=1&gclid=Cj0KCQjwqP2pBhDMARIsAJQ0CzroKCOagAig0VRgbrctMrRg17_oqPNvV1ybzMwLjDrYmj4LUZYmXx0aAq32EALw_wcB)
This was a poor decision. I realized after a while that my pump was really quiet and thought that maybe one of the kodak batteries was dead. I started shuffling only kodak batteries around and found no success
in fixing the problem. After a while I decided to compare this sound with the original pump sound from the first iteration and realized that the dollar store batterys must be suppling much more power (meaning current too).
So after 2 hours, I learned that kodak is a terrible battery brand and that dollar store is the way to go. With this increased current now supplying both voltage sources in the schematic, everything was working as intended including the LED strip. This version of the project proved to work quite well. I noticed a small leakage though after the event that caused dripping out of the backpack. Somehow, this leakage did not cause the circuit to fail. I believe that the lid of the container dampens any splashes stemming from movement of the backpack. This dampened wave will leak past the lid and drip down the container. Since all my important circuit components are elevated by breadboard along the surface of the container, the dripping beer/juice probably does not contact the key parts of the breadboard enough to cause unintended conductivity.

After completing this project, there are a few problems I ran into that I would improve in the future.

## Condensation:
I have never ran into this problem before since none of my projects include a large amount of water. I realized that the beer I purchased in the first iteration of this project caused my backpack to be damp from from condensation. I was worried that this would cause the circuit to have poor conductivity and therefore little/no power output especially on the pump. I found this to luckily not be the case but definitely potentially dangerous. I think either warming up the beer or having some type of termal insulation that can hold a lot of moisture to prevent it from spreading to the circuit would be a solid fix. The presence of liquid inherently will increase the air humidity of the bag which may not be fixable unless the circuit is isolated in a completely different bag which would likely not be very practical.

## Water/ Atmospheric Pressure:
There is some condition in which the pump stops going but the liquid continues its momentum and keeps flowing out of the tubing. This is obvioiusly not great as leakage means there is less drink to pour everyone and it makes a bit of a mess. I found that lifting/ pointing the tip of the tubing upwards prevents the liquid from exiting the tube. This likely has something to do with the water/ air pressure difference which is something that im a bit rusty with. Im thinking just having a stopper or something that can hold pressure would be the simplest way to fix this. This fix may have a problem with pressure buildup which can either be solved by modifying the stopped to gradually release pressure, or lifting the pump to ensure equalization of pressure. I am not exactly sure what the easiest/best way to incorporate these ideas are but that is something I will deal with if another iteration is made.
## General Design Inefficiencies:
There are a few design inefficiencies in this project. The biggest problem is the risk of spillage which can be fixed with some longer wire lengths where required as well as some water proofing which could be done with something as simple as duct tape or hotglue. I also think it would be nice to use a lid that can snap on instead of screw on which would be easier to fill. The second biggest problem is that no connections are prefectly secure, soldering the most important joints is one of the first things I wouls have done if given more time with the first iteration. Finally, using a safer container is something that would be great as well. Im sure I could find a way to create an unleakable container with whatever hospitals use with their iv bags. A bag also has higher volume and flexibility than a rectangular plastic container.


Project Github with code:

{% include embed.html url="https://www.youtube.com/embed/DbM7yC2fTYg" %}
