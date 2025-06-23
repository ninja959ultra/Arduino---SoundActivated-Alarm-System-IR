# Arduino---SoundActivated-Alarm-System-IR

# Description:
This project is a basic alarm system using an infrared remote, a sound sensor, and LEDs. Initially, the system is inactive. When any button on the infrared remote is pressed, a blue LED turns on to indicate that the system is now active and the sound sensor is monitoring the environment.

If a sound above a certain threshold is detected, the system responds by triggering an audible buzzer alarm and blinking a red LED three times.

Pressing any button on the remote again turns off the system. The blue LED turns off, indicating that the sound sensor is no longer active and the system has been disabled.


![img1](arduino_project_alarm_IR_voice_sensor_PART1.jpg)
![img1](arduino_project_alarm_IR_voice_sensor_PART2.jpg)
![img1](arduino_project_alarm_IR_voice_sensor_PART3.jpg)


# Code below:

```cpp
#include <IRremote.hpp>

IRrecv ir(2);


byte redLED = 9;
byte blueLED = 10;
byte buzzer = 4;
int voiceValue;
int voice = A5;


bool alertState = false;


void alert(){
  byte count = 0;

  do{
    digitalWrite(redLED, HIGH);
    digitalWrite(buzzer, HIGH);
    delay(300);
    digitalWrite(redLED, LOW);
    digitalWrite(buzzer, LOW);
    delay(300);
    count++;
  }

  while (count < 3);
}


void setup() {
  Serial.begin(9600);
  ir.enableIRIn();
  pinMode(redLED, OUTPUT);
  pinMode(blueLED, OUTPUT);
  pinMode(buzzer, OUTPUT);
}

void loop() {
  if (ir.decode()) {
    alertState = !alertState;
    delay(100);
  }

  if (alertState){
    digitalWrite(blueLED, HIGH);

    voiceValue = analogRead(voice);

    if (voiceValue > 520){
      alert();
    }

  }

  else{
    digitalWrite(blueLED, LOW);
  }

  

  ir.resume();

  Serial.println(voiceValue);

  delay(300);

}
```

