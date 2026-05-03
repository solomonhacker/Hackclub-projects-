# Smart Blind Navigation System (Glasses + Smart Stick + Intelligent Detection)

⚠️before we proceed i want to add in codes am using because am still building this project 

## Overview
The Smart Blind Navigation System is an assistive technology designed by inventorsolomon with the mail solomonmapereka@gmail.com to help visually impaired individuals navigate safely using a combination of smart glasses and a smart walking stick.


## Features
- Obstacle detection using ultrasonic sensors
- Distance measurement and real-time feedback
- Intelligent object detection (basic classification)
- Audio explanation of surroundings
- Dual system:
  - Smart Glasses (head-level detection)
  - Smart Stick (ground-level detection)
- Works offline (core features)
- Expandable with AI modules


## Advanced Features

### 1. Text Reading Mode (OCR-Based)
The system includes a text reading mode that allows the user to read printed text using a camera.

#### How it works:
1. Camera captures an image (via ESP32-CAM or smartphone)
2. Optical Character Recognition (OCR) processes the image
3. Extracted text is converted to speech

#### Example Output:
- "Reading text: Welcome to Kamwezi Health Center"
- "Text detected: Danger, high voltage area"


### 2. Signpost Detection and Direction Guidance
The system can detect and interpret signposts, then guide the user using speech.

#### How it works:
1. Camera scans the environment
2. OCR extracts text from signboards
3. Direction logic estimates position (left, right, center)
4. Audio feedback gives instructions

#### Example Output:
- "Sign detected: Hospital, direction ahead"
- "Shop on your left"
- "Road sign detected on your right"


### 3. Voice Guidance System
The system provides real-time spoken feedback instead of just buzzer alerts.

#### Features:
- Distance-based warnings
- Object awareness alerts
- Direction guidance
- Environment description

#### Example Outputs:
- "Object detected 1 meter ahead"
- "Obstacle at head level"
- "Step detected below"
- "Move slightly to the right"


### 4. Fire Detection System 🔥
The system includes a fire detection module to improve user safety.

#### Components:
- Flame sensor and temperature sensor

#### How it works:
1. Sensor detects presence of flame or high temperature
2. System triggers immediate alert
3. Audio warning is activated

#### Example Output:
- "Warning! Fire detected nearby"
- "High temperature detected, please move away"


## System Modes

The device operates in multiple modes:

1. Navigation Mode
   - Obstacle detection
   - Distance measurement
   - Direction guidance

2. Reading Mode
   - Text reading (OCR)
   - Signpost interpretation

3. Safety Mode
   - Fire detection
   - Emergency alerts

###  Embedded System (ESP32-CAM)
- Lightweight AI processing
- Basic image detection
- Lower power usage



## Voice Logic (Pseudo Code)

in python language 
if fire_detected:
    speak("Warning! Fire detected nearby")

elif distance < 50:
    speak("Object very close")

elif object_at_head_level:
    speak("Obstacle ahead at head level")

elif ground_obstacle:
    speak("Step detected below")

elif text_detected:
    speak("Reading text: " + extracted_text)

elif sign_detected:
    speak("Sign detected: " + sign_text + " direction " + direction)


## How It Works
1. Sensors continuously scan the environment
2. Distance to objects is calculated in centimeters and tells if on head level or groud level
3. The system determines:
   - How far the object is
   - Where it is (head level or ground level)
4. The system provides feedback:
   - Buzzer (basic warning)
   - Voice output (advanced version)



## Intelligent Detection System

### 1. Distance Detection
- Uses ultrasonic sensors (HC-SR04) and combine use of IR sensors
- Calculates distance using sound wave reflection
- Output example:
  - "Object detected at 50 centimeters"


### 2. Object Awareness (Basic AI Logic)
The system can estimate the type of object based on:
- Height (glasses vs stick)
- Distance patterns
- Movement behavior

#### Example Outputs:
- "Obstacle ahead at head level"
- "Step detected below"
- "solomon Object very close, please stop"

### 3. Advanced Object Detection
Using camera + AI (ESP32-CAM or phone integration):
- Detect objects like:
  - Person
  - Wall
  - Door
  - Vehicle

#### Example Output:
- "Person detected 2 meters ahead"
- "Wall detected very close on tye head level or maybe on the ground level if so the case"


## Components Used
- Microcontroller (Arduino and ESP32)
- Ultrasonic Sensors (HC-SR04)
- ir sensors
- Buzzer / Speaker Module
  - ESP32-CAM
- Battery (Lithium / Power Bank)
- Connecting wires


## System Design

### Smart Glasses
- Detect obstacles at head level
- Useful for:
  - Walls
  - Doors
  - People

### Smart Stick
- Detect ground-level obstacles
- Useful for:
  - Steps
  - Holes
  - Stones


###Arduino Code (Distance Detection + Alert)

```cpp
#define trigPin 9
#define echoPin 10
#define buzzer 8

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(buzzer, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // Trigger ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo
  duration = pulseIn(echoPin, HIGH);

  // Calculate distance
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Alert logic
  if (distance < 100 && distance > 50) {
    tone(buzzer, 1000); // medium warning
  } 
  else if (distance <= 50) {
    tone(buzzer, 2000); // strong warning
  } 
  else {
    noTone(buzzer);
  }

  delay(200);
}
