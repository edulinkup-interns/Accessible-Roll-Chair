# Accessible Smart Wheelchair for Visually Impaired ♿

## Overview
This project aims to create an **accessible smart wheelchair** for visually impaired individuals using **Arduino**. The wheelchair is equipped with sensors to detect obstacles and provide **audio alerts**, ensuring safer and independent navigation.

---

## Features
- Obstacle detection using **Ultrasonic Sensor (HC-SR04)**
- Audio alert using **Buzzer**
- Automatic stop when obstacles are detected
- Forward movement control using **L298N Motor Driver**

---

## Hardware Components
| Component                | Quantity |
|--------------------------|----------|
| Arduino UNO              | 1        |
| Ultrasonic Sensor HC-SR04| 1        |
| L298N Motor Driver       | 1        |
| DC Motors                | 2        |
| Buzzer                   | 1        |
| Jumper Wires             | As needed|
| Power Supply             | 1        |

---

## Pin Configuration

| Component | Arduino Pin |
|-----------|------------|
| Ultrasonic TRIG | 9  |
| Ultrasonic ECHO | 10 |
| Buzzer         | 8  |
| Motor IN1      | 2  |
| Motor IN2      | 3  |
| Motor IN3      | 4  |
| Motor IN4      | 5  |
| Motor ENA      | 6  |
| Motor ENB      | 7  |

---


## Arduino Code
Upload the code from `AccessibleWheelchair.ino`:

```cpp
// Accessible Smart Wheelchair for Visually Impaired
// Obstacle Detection + Audio Alert + Motor Control

#define TRIG_PIN 9
#define ECHO_PIN 10
#define BUZZER 8

#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5
#define ENA 6
#define ENB 7

long duration;
int distance;

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(BUZZER, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);
  digitalWrite(ENA, HIGH);
  digitalWrite(ENB, HIGH);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  duration = pulseIn(ECHO_PIN, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance > 0 && distance <= 30) {
    stopWheelchair();
    digitalWrite(BUZZER, HIGH);
  } else {
    moveForward();
    digitalWrite(BUZZER, LOW);
  }
  delay(100);
}

void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stopWheelchair() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

