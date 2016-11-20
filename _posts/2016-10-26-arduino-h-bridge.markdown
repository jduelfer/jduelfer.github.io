---
layout: 	post
title: 		"Arduino: H-Bridge IC"
date: 		2016-10-26 22:00:00 +0200
categories:	Arduino
---

This post will cover how to use an H-Bridge integrated circuit (IC) to reverse the direction of the motors on my robot car. I will also take advantage of the new motor control to improve my obstacle avoidance algorithm. Here is a video showing you what you will have by the end of the tutorial:

<iframe class="img-responsive video-responsive" src="https://www.youtube.com/embed/rFysFpAPYzg" frameborder="0" allowfullscreen></iframe>

<br>

### Material
_____________________

I am following [this tutorial](https://itp.nyu.edu/physcomp/labs/motors-and-transistors/dc-motor-control-using-an-h-bridge/) and [this tutorial](http://hardwarefun.com/tutorials/creating-robots-using-arduino-h-bridge).

Also going to use the Arduino Time library from [here](https://github.com/PaulStoffregen/Time) or [here](http://playground.arduino.cc/Code/Time).

<br>

### Integrated Circuit
_____________________

![Front_IC](/img/front_ic.jpg){:class="img-responsive"}
![Side_IC](/img/side_ic.jpg){:class="img-responsive"}
![Back_IC](/img/back_ic.jpg){:class="img-responsive"}

<br>

### Improving the Algorithm
_____________________

{% highlight C %}
#include <Time.h>
#include <NewPing.h>

// sensor stuff
#define TRIGGER_PIN   12
#define ECHO_PIN      11
#define MAX_DISTANCE  200

// motor stuff
#define E1 7  // enable pin for motor 1
#define E2 8  // enable pin for motor 2
#define I1 6  // control pin 1 for motor 1
#define I2 5  // control pin 2 for motor 1
#define I3 10 // control pin 1 for motor 2
#define I4 9  // control pin 2 for motor 2

NewPing sonar(TRIGGER_PIN, ECHO_PIN, MAX_DISTANCE);

time_t lastStartTime = now();
int distanceToStop = 40;

void setup() {
  pinMode(E1, OUTPUT);
  pinMode(E2, OUTPUT);

  pinMode(I1, OUTPUT);
  pinMode(I2, OUTPUT);
  pinMode(I3, OUTPUT);
  pinMode(I4, OUTPUT);
  
  Serial.begin(115200);
}

void loop() {
  unsigned int uS = sonar.ping(); // send ping and gets time in milliseconsds (uS)
  unsigned int distance = uS / US_ROUNDTRIP_CM;
  Serial.print("Ping: ");
  Serial.print(distance); // Convert ping time to distance and print result (0 = outside set distance range, no ping echo)
  Serial.println("cm");

  unsigned int diffSinceLastStart = second(lastStartTime);
  // set the distance to stop based upon time since last started
  Serial.println("DiffSinceLastStart : " + diffSinceLastStart);
  if (diffSinceLastStart < 1) {
    distanceToStop = 15;
  } else if (diffSinceLastStart < 2) {
    distanceToStop = 25;
  } else if (diffSinceLastStart < 3) {
    distanceToStop = 40;
  } else {
    distanceToStop = 50;
  }
  Serial.println("Distance to stop : " + distanceToStop);

  if (distance > distanceToStop || distance == 0) {
    // record this as last start time
    lastStartTime = now();
    
    // enable both motors
    digitalWrite(E1, HIGH);
    digitalWrite(E2, HIGH);
    digitalWrite(I1, HIGH);
    digitalWrite(I2, LOW);
    digitalWrite(I3, HIGH);
    digitalWrite(I4, LOW);

    // delay 50 ms for the next ping
    delay(50);
  } else {
    // stop
    digitalWrite(E1, LOW);
    digitalWrite(E2, LOW);
    delay(200);

    // back up a little
    digitalWrite(E1, HIGH);
    digitalWrite(E2, HIGH);
    digitalWrite(I1, LOW);
    digitalWrite(I2, HIGH);
    digitalWrite(I3, LOW);
    digitalWrite(I4, HIGH);

    delay(250);
    digitalWrite(E1, LOW);
    digitalWrite(E2, LOW);
    delay(200);

    unsigned int randomDirection = random(0,2);
    if (randomDirection == 0) {
      // run left motor for a second
      digitalWrite(E1, HIGH);
      digitalWrite(I1, HIGH);
      digitalWrite(I2, LOW);
    } else {
      // run right motor for a second
      digitalWrite(E2, HIGH);
      digitalWrite(I3, HIGH);
      digitalWrite(I4, LOW);
    }
   delay(300);
  }
}
{% endhighlight %}