# UltrasonicRobot
### Johnny Krosby and Elliot Greenwell



**Design**



<img src="https://github.com/jkrosby51/UltrasonicRobot/blob/main/images/UltraSonicRobot-Design.png" alt="Design"
	title="Design" width="650" height="500" />

**Code**

```C++
#include <Servo.h>

Servo rWheel;
Servo lWheel;

int servoPinL = 9; //left wheel
int servoPinR = 11; //right wheel
int triggerPin = 2;
int echoPin = 3;
int distanceMax = 12;
int distanceMin = 0;
int distance;
int duration;
int speed; //servo speed
int obstacle = 0; //0 = clear path, 1 = object in the way

void setup() {
  Serial.begin(9600);
  pinMode(triggerPin, OUTPUT);
  pinMode(echoPin, INPUT);
  rWheel.attach(servoPinR);
  lWheel.attach(servoPinL);
}

void loop() {
  distance = 0;
  duration = 0;
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  duration = pulseIn(echoPin, HIGH);

  // Convert in CM
  distance = (duration / 2) / 29.1;

  if (distance >= distanceMax || distance <= distanceMin) {
    Serial.println("Target not found");

    obstacle = 0;
  } else {
    Serial.print(distance);
    Serial.println(" cm");

    lWheel.detach();
    rWheel.detach();
    
    obstacle = 1;
  }

  if(obstacle == 0){
    Serial.println("Moving");
    
    lWheel.attach(servoPinL);
    rWheel.attach(servoPinR);
    
    lWheel.write(0);
    rWheel.write(0);
  }

}
```
