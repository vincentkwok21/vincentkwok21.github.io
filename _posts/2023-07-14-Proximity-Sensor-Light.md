---
title:  "Making a Proximity Sensor Light"
mathjax: true
layout: post
categories: media
---
While the Ultrasonic sensor project was interesting, I wanted to add a little more flexibility in my skillset of Arduino, I have noticed a lot of
Arduino related projects are very focused on manipulation of light and so I have decided to kind of combine the ideas of GLSL's color related syntax as
well as the ultrasonic radar's ability to detect distance. I went and bought some WS2812B LED's from Amazon and got to work on a light that would change color 
the closer you are to it. I believe this is the first project I have created that could easily be a device that can change someone's quality of life with just a 
few changes. A light that gets brighter as you get closer to it could be used as a night light for young children of people with bad eyesight. It can be used
as under lighting for a bed, desk, or stairs to prevent someone from running into it. LED lights are also a popular decoration tool right now for younger people.
Being able to program LED lights outright seems like a valuable skill to have so lets do it!
![IMG_20230714_145451](https://github.com/vincentkwok21/vincentkwok21.github.io/assets/137122312/01d1df65-b9ef-405a-ab21-500f4d51a74f)



The first thing I stumbled upon when trying to figure out how to do this project was that I needed to download to FastLED library which can be easily downloaded through the Arduino
IDE. While this dictionary is pretty straightforward, it is one of the few dictionaries that I have used for Arduino. The documentation can be found [here](https://fastled.io/),
it is filled with many quality of life methods and variables that enable programmers to easily create and display color gradients, timings, and display settings.
I found the syntax of this entire dictionary was very similar to my adventures into the GLSL programming language, the syntax of RGBA, color gradients and more
were almost exactly the same. GLSL though is much more math and computationally intensive than arduino, which makes sense considering the operational compatibilities of the Arduino
in complicated tasks. I found [Scott Marley's](https://www.youtube.com/playlist?list=PLgXkGn3BBAGi5dTOCuEwrLuFtfz0kGFTC) tutorials on FastLED to be very helpful. I ended up wathcing the entire series even though I really only used the first 2 videos of information. 
learning about LED color and programming is a really interesting physics lesson in of itself!

I first began with the basics, how to change independent LED's as well as using predefined keywords to specify colors. I have attached the code for this post
in [github](https://github.com/vincentkwok21/Motion-Sensor-FastLED) as well as below:

{% highlight ino %}

#include <FastLED.h>

#define NUM_LEDS  18
#define LED_PIN   2

CRGB leds[NUM_LEDS];

void setup() {
  FastLED.addLeds<WS2812B, LED_PIN, GRB>(leds, NUM_LEDS);
  FastLED.setBrightness(50);
}

void loop() {
  leds[0] = CRGB::Red;
  leds[1] = CRGB::Green;
  leds[2] = CRGB::Blue;
  FastLED.show();
}

{% endhighlight %}
<sub>^ This code sets the first LED to Red, second to Green, third to Blue [youtube video demonstration skip to end for lights](https://youtu.be/SgsR8GOGOnc)</sub> 

So here I learned the general syntax and method to address particular LED's. There is a type called CRGB which seems to be an array that allows the user to choose how many LED's are
in the project. I wonder engineering is in these diodes that allows us to address specific LED's. The thing is that you can cut the strips apart into smaller segments and
each segment can be individually programmable. I suspect this might have something to do with using resistance to decide which led recieved information, not exactly sure but just 
a big question I can while working on this project.

Next I attempt to program gradient, by using bytemark.com as well as Paletteknife, it is very easy to import gradients into FastLED. By creating an object of 
CRGB Palette16 type, we can cycle though the values of this data structure and fill each led to match the respective color. This allows us to imitate any gradient smoothly
provded enough leds are supplied. Although my strip has 300 LED's, it is fascinating how I can specify that I want only 18 LED's to work at all.
 This is actually a really good safety measure. Since each led will require a certain voltage and amperage, it will be very easy to overdraw power from a computer port. I'm sure that this library was created by someone over a long time very skillfully but 
these safety considerations are still really cool in my opinion. I have attached the [code](https://github.com/vincentkwok21/Motion-Sensor-FastLED/blob/main/LED_light_Moving_gradient.ino) below 
for a green-blue gradient. There is an EVERY_N_MILLSECONDS() method which is fascinatingly helpful. It can be used to move this gradient and other
light elements up and down the strip by just incrementing the gradient index every dozen or so milliseconds.

{% highlight ino %}

#include <FastLED.h>

#define NUM_LEDS  18
#define LED_PIN   2

CRGB leds[NUM_LEDS];
uint8_t colorIndex[NUM_LEDS];

DEFINE_GRADIENT_PALETTE( greenblue_gp ) { 
  0,   0,  255, 245,
  46,  0,  21,  255,
  179, 12, 250, 0,
  255, 0,  255, 245
};

CRGBPalette16 greenblue = greenblue_gp;

void setup() {
  FastLED.addLeds<WS2812B, LED_PIN, GRB>(leds, NUM_LEDS);
  FastLED.setBrightness(50);

  //Fill the colorIndex array with random numbers
  for (int i = 0; i < NUM_LEDS; i++) {
    colorIndex[i] = random8();
  }
}

void loop() {
  
  // Color each pixel from the palette using the index from colorIndex[]
  for (int i = 0; i < NUM_LEDS; i++) {
    leds[i] = ColorFromPalette(greenblue, colorIndex[i]);
  }
  
  EVERY_N_MILLISECONDS(5){
    for (int i = 0; i < NUM_LEDS; i++) {
      colorIndex[i]++;
    }
  }
  FastLED.show();
}

{% endhighlight %}

<sub>^ Very Simple to incorporate Gradients</sub>

So after this code, I thought I was ready to take on my original problem! I was trying to figure out how to slide through a gradient based on changing a single variable but I could 
not really figure out so intead of running off a gradient, I decided I would just use a basic HSV syntax and manipulating a single value based on distance.
I found the HSV syntax to be very interesting. It represents Hue, Saturation, Value, which is a very helpful selection of variables for some color transitions.
For example, if I wanted to make a transition where I start at green and make it a darker green over time, I can just adjust the saturation value over time. For an
RGB syntax, I would probably have to keep the same ratio between RGB while also decreasing the values of all R, G, and B to make the total color darker. There are also some 
corrections what can be doe to the color. I found using the TypicalPixelString setting accentuates the Reds and weakens the greens/yellows which to the human visual specturm are
typically more bright than red. TypicalPixelString makes the greens and yellows less bright in order to balance the colors out in the display making everything a
bit more accurate.

I added the Ultrasound sensor into the project using a lot of similar code from my musical instrument [post](https://vincentkwok21.github.io/UltrasoundInstrument/). I found a lot of fluctuations and inconsistencies with the distance measurements causing the light to
flicker between saturation values instead of smoothly transitioning between distance values. I did some internet searching saying at medium distances, the ultrasound may end up overalpping ontop of itself and 
confusing the reciever if the frequency of emitted sound rates is too tight. I decide to reduce the rate the emitter and reciver make/record values and it seemed to help.
I found some very interesting details in this library as well. It turns out, if you adjust the brightness to a small negative values, the strips will appear very bright. I was afraid the strip might be demanding too much current from the arduino so I freaked out and unplugged it (lol).
Near the end of the code, you will see that I had to create an if statment to prevent the brightness from wrapping back around. This is not the only value that wraps around though. It seems so do Hue, Saturation and Values values.
I found that if distance changes the hue, I can wrap back around to the same colors at a different distance. This forced me to limit the distance that my sensor can record. Since I have hue as
an integer that is a factor of distance, I cannot perfectly multiply it to 255 (maximum value for Hue, Saturation, Value). There are definitely still some flaws that prevent it from
being as functional as possible. First, my ultrasonic sensor just is not that stable, a LIDAR or laser distance sensor would be better. The sensor also is not great at detection an object moving perpendicularly to the field or
vision. The way I have programmed this does turn the light off if the object gradually gets further away from the FOV of the sensor but this sensor cannot measure more than a 30 degree cone in front of it, so it cannot
turn off if the percieved distance changes. I intend to use the stabilizaion array to keep track of static distance inputs and fade the light the longer a nearby object keeps a constant distance.
Anyways, here is the final code as well as a video of me using it and explaining the [code](https://github.com/vincentkwok21/Motion-Sensor-FastLED/blob/main/Variable_Saturation.ino).

{% highlight ino %}

#include <FastLED.h>
#define NUM_LEDS 16
#define LED_PIN 2
const int trigPin = 9;
const int echoPin = 10;
uint8_t hue = 108;
long duration;
int distance;
CRGB leds [NUM_LEDS];
int stabilization[3] = {};
int distance_readings_counter = 0;
int brightness = 10;

void setup() {
  FastLED.addLeds<WS2812B, LED_PIN, GRB>(leds, NUM_LEDS);
  FastLED.setBrightness(10);
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an Output
  pinMode(echoPin, INPUT); // Sets the echoPin as an Input
  Serial.begin(9600); // Starts the serial communication
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(100);
  digitalWrite(trigPin, LOW);
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;
  if (distance < 90) {
    stabilization[distance_readings_counter % 3] = distance;
    distance_readings_counter++;
  } else {
    distance = stabilization[distance_readings_counter % 3];
  }
 
  hue = distance * 2.5;
   Serial.print("Distance: ");
  Serial.println(distance);
    FastLED.setCorrection (TypicalPixelString);
  for (int i = 0; i < NUM_LEDS; i++) {
    leds[i] = CHSV(170, hue, 255);
  }
  if ( 80 - distance < 0) {
    brightness = 0;
  }else {
     brightness = 80-distance; 
  }
  FastLED.setBrightness(brightness);
  FastLED.show();
}
{% endhighlight %}

{% include embed.html url="https://www.youtube.com/embed/t7PWfO0ui6Q" %}

