Assignment 2 — Multi-Sensor Temperature & Humidity Simulation
🎯 Objective

Simulate a 6-channel temperature and humidity validation system inspired by the Kaye Validator, using an Arduino board, breadboard, and LED indicators for each sensor channel.

🧩 Hardware Requirements
Component	Quantity	Notes
Arduino Uno / Nano	1	Any equivalent board
Temperature + Humidity sensors (DHT11 / DHT22 / AHT10)	6	Each represents one validation point
LEDs (Red, Yellow, Green)	18	3 per channel
Resistors (220 Ω)	18	Current limiting for LEDs
Breadboard & Jumper Wires	—	For prototyping connections
(Optional) MQTT / Node-RED setup	—	For data publishing or visualization

💡 If hardware is not available, students can simulate the setup using Wokwi
 online Arduino simulator.

⚙️ Logic Overview

Each sensor channel acts as an independent validation point.

Condition	LED	Meaning
Temp > High limit (e.g. 30 °C)	🔴 Red	Over temperature
Temp within ideal range (20–30 °C)	🟢 Green	Stable / Perfect
No reading or sensor disconnected	🟡 Yellow	Idle / Not working

Humidity thresholds can also be applied similarly (optional).

🧠 Step-by-Step Tasks

Connect 6 sensors to Arduino digital pins (2–7 for example).

Assign 3 LEDs per sensor for Red, Green, Yellow indicators.

Define threshold logic in code.

Display readings and LED states on the Serial Monitor.

Test by simulating different temperature values.

(Optional) Publish readings via MQTT to a Node-RED dashboard.

🧾 Deliverables

Arduino .ino file uploaded to your /Assignment2_<YourName>/ folder

Breadboard photo or Wokwi simulation screenshot

Short description of how your thresholds are defined

(Optional) Small video of working setup

🧰 Example Code Snippet
#include "DHT.h"
#define NUM_SENSORS 6

int sensorPins[NUM_SENSORS] = {2,3,4,5,6,7};
int redPins[NUM_SENSORS]    = {8,9,10,11,12,13};
int greenPins[NUM_SENSORS]  = {22,23,24,25,26,27};
int yellowPins[NUM_SENSORS] = {28,29,30,31,32,33};

DHT dht[NUM_SENSORS] = {
  DHT(2, DHT22), DHT(3, DHT22), DHT(4, DHT22),
  DHT(5, DHT22), DHT(6, DHT22), DHT(7, DHT22)
};

void setup() {
  Serial.begin(9600);
  for (int i=0; i<NUM_SENSORS; i++) {
    dht[i].begin();
    pinMode(redPins[i], OUTPUT);
    pinMode(greenPins[i], OUTPUT);
    pinMode(yellowPins[i], OUTPUT);
  }
}

void loop() {
  for (int i=0; i<NUM_SENSORS; i++) {
    float t = dht[i].readTemperature();
    if (isnan(t)) {
      digitalWrite(redPins[i], LOW);
      digitalWrite(greenPins[i], LOW);
      digitalWrite(yellowPins[i], HIGH);
    } else if (t > 30) {
      digitalWrite(redPins[i], HIGH);
      digitalWrite(greenPins[i], LOW);
      digitalWrite(yellowPins[i], LOW);
    } else if (t >= 20) {
      digitalWrite(greenPins[i], HIGH);
      digitalWrite(redPins[i], LOW);
      digitalWrite(yellowPins[i], LOW);
    } else {
      digitalWrite(yellowPins[i], HIGH);
      digitalWrite(redPins[i], LOW);
      digitalWrite(greenPins[i], LOW);
    }
    Serial.print("CH"); Serial.print(i+1);
    Serial.print(": "); Serial.print(t);
    Serial.println(" °C");
  }
  delay(2000);
}

🧩 Learning Outcomes

Students will:

Understand multi-sensor acquisition and multiplexing

Implement basic threshold logic with visual indicators

Simulate validation logic similar to thermal mapping / calibration

Gain experience with circuit prototyping and code structuring

📚 Optional Extension

Add averaging over time to simulate data stabilization.

Send data via MQTT and visualize in Node-RED Dashboard 2.0.

Log data to CSV or local database for “mock validation reports.”

🏁 Submission Checklist

 Arduino code tested

 Circuit diagram or simulation included

 Screenshot or photo uploaded

 Explanation of limits used

 Commit pushed to GitHub repository

🌟 Assignment Theme

“Simulating Thermal Validation Logic — Six Channels, Six States.”
