---
title:  "Making Logic Gates from scratch on PCB"
mathjax: true
layout: post
categories: media
---

I have been doing Arduino projects for a while and have been continuing to do so but I don't just want to be a 1 trick pony so I have decided to make something using just the electrical engineering fundamental knowledge I have. For
this project, I will not be using Arduino as it seems a bit too easy to do some things. Arduino is often considered a tool of hobbiests to tinker with. While this is often very convienient, I want to do a more time consuming project that focuses a bit deeper into the bread and butter of electrical engineering. I began this project by drawing some schematics of logic gates without using the internet as a challenge.

![PCBlogicgate](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/b886fcfe-f470-4131-a99b-8d6d98420e76)
<sub> Final Draft of PCB</sub>



Using the concepts learned in my devices classes, I deployed the concept of transistors being able to be used as switches to create logic gates. These gates follow the assumption that no current from the base pin of the BJT will reach the emitter pin. This assumption, I later found out, was completely false, causing my circuits to not behave exactly as planned. This incorrect assumption is from me forgetting that BJT's and MOSFETs are different things (lol) but without the internet I completely did not realize until I tried building these circuits on breadboard. If you take all my sketches and replace every BJT with an NMOSFET, I believe they would work. I found some very interesting insights into how you could manipulate truth tables of a given logical circuit in this pursuit though. While my XOR and NAND gates may not be the most optimized choices, they show how you can "add" circuits together or invert a circuit by simply adding a not gate into another logic gate. The entire PDF of my sketches for this entire post can be found [here.](https://github.com/vincentkwok21/Logic-Gate/blob/main/Transistor%20Logic%20Gates.pdf)

![image](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/5627d12d-1f57-4eda-87e2-7f6e15c61382)

<sub> My iteration of XOR gate. Notice how the XOR gate uses AND (left side) and OR (right side). The upper drawing shows my thought process of using a NOT gate to create the correct output to feed into the BJT (which should be an NMOSFET). Bottom drawing is my final idea.</sub>

![image](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/c393f516-317f-44ad-96cd-c058c7b27362)

<sub> My iteration of NAND gate. Notice how I implement an NOT gate with the AND gate to create NAND. </sub>

Though after realizing that BJT's did not work as I previously expected, I revisited all my circuits, using the concept of voltage dividers to get the correct voltage drop for the LED's to glow, combining this with some internet access and I created the AND, OR,
NAND, and NOR gates. I omitted the XOR and XNOR gates because they seemed a bit more complicated to create since I had only NPN BJT transistors. I realized here that changing the location of the parallel resistor and LED beetween the front and the back of the circuit
resulted in and inverse input that did not require the construction of a NOT gate, which I found pretty neat. I tested each of these schematics on a breadboard and had success with all of them.

My original intention of this project was to take all these circuits and put them on a PCB for soldering practice while also ordering my own PCB which is something I have never done before. Before modelling it on Kicad and ordering the PCB, I wanted to make sure that
no anomalies would take place. I have never wired something with this many components in a design I created myself so I decided to build all the circuits on a breadboard module and connect all their voltage supplies and grounds to a single 5V and GND pin on the Arduino/ Battery
I realized here that the Arduino must have some specification for power output. It could be possible that with all LED's on, I would run out of power and require a power supply module. I probably should have just estimated the total power output and compared it to 
some Google searched statistic on the power output of the Arduino but that did not seem as fun as just brute force building it so I built it, falling into some silly situations which I explain in the video:

{% include embed.html url="https://www.youtube.com/embed/Ak-EwyCQpS8" %}
<sub> A video of me demonstrating each logic gate, this project made me realized I had to buy many more resistors for future projects, especially if I intend to do soldering</sub>

![IMG_20230721_020510](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/f585366e-08a0-4e00-a612-42cdbb7093f7)
<sub> Final Breadboard Build seen in video</sub>

After demonstrating that the project works well, I decided to create it in KiCAD with the idea to order it from a PCB manufacturing website. I did some refreshers on how to use KiCAD as well as how to create a Gerber/Drill file. I also brushed up on the Layers of the PCB as I had forgotten
exactly which layers must be used. I even learned how to make a custom Footprint since I didn't like the selection of switch footprints preinstalled into Kicad compared to the switches I owned. When constucting the PCB,
I was really amazed by how intelligent the assistance was in creating copper pathing. I adapted to my choice in the positioning of the components to produce what would seem to be a much more material efficient way to design. I hope to do more PCB design in the future
as the tools in Kicad to prevent electrical and design errors/inefficiencies is something that I could also improved on and learn about from experience.
I will attach images of the completed schematic and PCB below:

![PCBschmatic](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/78362faa-8ff4-4e92-acdb-904be812ff78)

<sub> AND (top-left), NAND (top-right), OR (bottom-left), NOR (bottom-right). </sub>


![PCBlogicgate](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/b886fcfe-f470-4131-a99b-8d6d98420e76)


[Schematic](https://github.com/vincentkwok21/Logic-Gate/blob/main/LogicSchematic.pdf)      [PCB Image](https://github.com/vincentkwok21/Logic-Gate/blob/main/PCBlogicgate.PNG)      [PCB ZIP](https://github.com/vincentkwok21/Logic-Gate/blob/main/LogicPCB1.zip)

So I created the Gerber Files and such and uploaded the files onto various PCB websites. I found that the shipping for all these websites was at least 15 dollars for any reasonable shipping time which made me really sad actually. I didn't want to pay at least 25 
dollars for such a simple circuit, so I think I will pass on buying the PCB, alternatively I have some perf boards that I will use with solder which is actually even better for geting some solder practice! Anyways, I intend to actually order a PCB, though 
probably for a bigger project that does not go as well with perf boards. I noticed when I was scrolling through the footprint libraries that there were the Cherry MX keyboard switch footprints in a library which is kind of inspiring me to make a keyboard PCB?
Could be an interesting project that is a legitimate use of PCB companies. I have also seen several youtube videos about making your own PCB designs at home though that might require me to buy some materials that I just dont really want to have sitting around
so I will probably not go that course.

Anyways, I cannot exactly follow my PCB wiring too well for soldering since I do not have two distinguished layers that I can easily create so I will go back to the schmatic. I will update this post when that is done!

## Update: 7/27/2023 
I have been trying to solder the original schematic for the last several days. My first attempt was unsuccessful as the NOR gate simply didnt work and while testing the circuit without the buttons and main postiive battery and negative battery terminals, I shorted the diode by accidentally testing the incorrect lead for the or gate. I decided to resolder the whole thing, getting successful results after testing each logic gate after it was soldered. I then wired the buttons and positive and negative battery terminal lines into the perf board and did not get the circuits to work. The only functional circuit was the NOR gate. I then posted asking about ways to potentially fix my project, there wasnt too much advice that I already knew though a user did suggest that I print it on a PCB so I will order a PCB from my schmatic and get it ordered. It seems there are no problems with the schematic as I have already createdd it on breadboard as well as shown it on the internet without anyone noticing any problems. For now, I would not call this project a success though I did have a working model even on the PCB at some point though I do not have any video evidence as it took me a long time to test all the inputs for each logic gate. I will attach the images of my perfboard job below:

![IMG_20230727_001140](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/67eed9c3-4f77-4b61-94c0-ff9573d75b45)
<sub>Top right AND, Top Left NAND, Bottom Right OR, Bottom Left NOR</sub>

![IMG_20230727_001126](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/3b687852-89fa-4167-acf4-e0681ef4cdd9)

## Update: 8/15/2023
I have had the PCBs delivered last week though I was preoccupied with the robotic arm project. I got the board to work after an intial failed attempt where I put the button switches in the incorrect orientation for each position, causing every single LED to output the respective input if both buttons were considered as active or "1". As foolish as I felt for putting all the switches in the wrong orientation, this outcome was actually very promising since I is a pretty strong indicator that every thing was connected correctly, AKA: the schematic works and the problem with my solder job from the perf board attempts was truly a faulty connection and not a design error. Fortunately, the PCB manufacturer requires at least 5 copies of the same pcb to be ordered so I had multiple tries for this project. I used the second PCB and found that the design worked flawlessly :). So finally, after 4 attempts of failing a solder job, a whole day of refreshing and designing in KiCAD, a week of waiting for the PCB to arrive, and designing the logic gates from scratch by hand, my longest project so far has been competed! I am shocked at how long such as simle seeming project has taken but I have also learned a lot about the design process of creating a larger circuit and using software to make designs into reality.

{% include embed.html url="https://www.youtube.com/embed/2VDAhpkBWYY" %}

<sub>Final logic gate demo video</sub>
