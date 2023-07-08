---
title:  "Making an Ultrasound Instrument"
mathjax: true
layout: post
categories: media
---

Following the idea of creating some larger "musical" system using electrical engineering which I touched upon in the GLSL post, I think I 
will probably require some sort of basic instrument as a starting point. I have been looking at some basic arduino sensors that I have to see what I can use to generate noise.
I decided to go with a HC-SR04 Ultrasonic Sensor. The HC-SR04 sensor measures distance away from the nearest object in it's cone of vision. The sensor has a transmitter 
and a reciever (called the trig and echo pin/sensor respectively). This sensor uses the concept of the speed of sound being a constant, with this assumption,
 a sound wave is emitted from the trig emitter and received by the echo receiver. By using some basic math involving the speed of sound and the time 
between ultra sound emission and receiving, it is possible to calculate the distance of an object from the sensor.

![IMG_20230706_174925](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/0ec86593-d4f9-491a-ab23-5ff877f84324)



Here is an image detailing distance calculation, speed * time is divided by 2 due to the fact that the sound has to travel between the reciever and object twice.

![image](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/db830e99-4a26-41c4-9fb8-2ca0416bda75)


I then began to wire the sensor, having it print the distance readings. I did some testing and the sensor fails to measure the 
distance of very curved objects, thin objects, and very near objects which are all reasonable flaws for a sensor that relies on sound. Curved objects likely alter the reflection of sound waves, thin objects create a small reflection of sound compared to the sound going past the object
and very near objects are likely very strange due to the path that sound may have to travel to leave a transmitter and reach another sensor that is not in the same location. Anyways, after preliminary testing, it seemed
as long as the object used to reflect the ultrasound is reasonable, everything should function well.

After this sensor is functioning properly, I had to add a passive buzzer. Passive buzzers require square waves in order to function. The voltage must be rapidly alternating between 
a high and low voltage in order to vibrate a magnet inside the speaker. When this magnet vibrates fast enough its sound can be measured in terms of musical notes. At around 262Hz, the corresponding note 
becomes C4, the middle C on the piano. Using a [table](https://pages.mtu.edu/~suits/notefreqs.html) to find corresponding frequencies to notes, we can just set certain distances to certain frequencies by using the 
tone() function in arduino, which handles any details to generate a square wave at a 50 percent duty cycle.

The sound was working pretty well but I noticed random fluctuations in the sound of the instrument despite my hands not being at the distance required to make that noise. I realized that there are sometimes incorrect readings that cause a large
spike in the pitch of the sound. I have recognized historical dilemmas like this in signal processing. I can either using a convolution filter or a simpler averaging filter to smooth out a dataset. I decided to go with the simpler apporach of using an averaging filter.
Using an array to store values and cycling through values by filling them up chronologically, I was able to calculate the average of the last 10 distance samples, then rounding it to the nearest integer to get very stable readings. Using the averaging over time,
rounding, as well as giving a generous range for each distinct musical note allowed me to make a pretty dependable instrument. In order to play a rest, I need to disable the speaker which I did by adding a button that acts as a switch, creating an open circuit whenever
the button is not pressed. This effectively gives me an instrument that can play any melody that does not have overlapping notes, though perhaps a strong understanding of music theory could still make me able to replicate any musical combination with a single speaker since it seems to be possible with
modern technology.

{% highlight ino %}

void loop() {
  digitalWrite(trigPin, LOW);  // turn the LED on (HIGH is the voltage level)
  delayMicroseconds(2);                      // wait for a second
  digitalWrite(trigPin, HIGH);   // turn the LED off by making the voltage LOW
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);                      // wait for a second

  noInterrupts();
  duration = pulseIn(echoPin, HIGH);
  interrupts();
  distance = (duration*.0343)/2;
  stabilization[distance_readings_counter % 9] = distance;
  distance_readings_counter++;
  for ( int i = 0 ; i < 9 ; i++) {
    average += stabilization[i] / 9.0;
  }
  int round = (int) average;
  Serial.print("Distance: ");
  tone(13, note(round));
  Serial.println(round);
  lcd.setCursor(0, 1);
  lcd.print(notePrint(round));
  average = 0;
  delay(10);
}

{% endhighlight %}
<sub>^the main behavior of the code is shown here, stabilization[] is used as the sliding averaging window, the calculations for distance as well as rounding are heres as well [Link to arduino code](https://github.com/vincentkwok21/Ultrasonic-Musical-Instrument/blob/main/Music_ultrasonic.ino)</sub> 

I then wanted to see if I could incorporate an LCD screen into the build as well. For example, displaying notes that you are playing could be useful, or even some variation of sheet music could be very insteresting as well. I followed this simple [hello world tutorial](https://www.arduino.cc/en/Tutorial/LibraryExamples/HelloWorld) to get it running,
most of the work done in this section was correctly wiring the circuit. The final build is quite busy and dense, especially with so many devices all coneected to the same power source and ground. I have attached some images/videos of the build below:

Me playing the chorus of my current favorite [song!](https://www.youtube.com/watch?v=lFUDk5G_1s8)
{% include embed.html url="https://www.youtube.com/embed/Hh_Fox8wAEc" %}
![IMG_20230706_174402](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/e7435a6e-0a1a-46b9-ad3a-530fa3633e4c)
![IMG_20230706_174925](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/c9635151-2946-47ad-9cd5-6071d11c8cad)
![IMG_20230706_174829](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/67cda8b4-2a59-4b3a-a667-5144df754153)
![IMG_20230706_175042](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/9f0d2210-6fa9-485a-91a1-295ce3af1e25)

[Link to arduino code](https://github.com/vincentkwok21/Ultrasonic-Musical-Instrument/blob/main/Music_ultrasonic.ino)
