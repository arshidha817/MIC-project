# Robotic Waiter for Restaurants (ME2200)

An autonomous hospitality robot designed to deliver food items from a kitchen to specific tables using line-following technology, integrated safety sensors, and smart weight detection.

---

## Project Overview
This project implements a fully functional robotic waiter capable of navigating a restaurant layout. It uses IR sensors to follow a dedicated path, an ultrasonic sensor for collision avoidance, and a load cell to monitor the delivery status. The robot is designed to wait for a load, travel to a specific table based on user input, and wait for the customer to pick up their order before resetting.

## Key Features
* **Autonomous Navigation:** Uses dual IR sensors for precise line following with memory logic to handle small gaps or surface inconsistencies.
* **Targeted Multi-Table Delivery:** Supports multiple destinations. The user selects the target table via a sequence of button presses.
* **Safety Obstacle Detection:** Integrated Ultrasonic sensor automatically halts the robot if a person or object blocks the path, resuming only when the path is clear.
* **Intelligent Payload Sensing:** Utilizes an **HX711 Signal Conditioner and Load Cell** to:
    * Stay in standby until food is placed on the tray (Weight > 10g).
    * Detect "Hand-off" (when the customer picks up the food) to complete the mission.
* **Audio Feedback:** Buzzer indicators for power-on, table selection confirmation, arrival, and mission completion.

## Hardware Components
* **Microcontroller:** Arduino Uno (with Adafruit Motor Shield L293D)
* **Drive System:** 2WD DC Geared Motors
* **Line Sensors:** 2x Infrared (IR) Digital Sensors
* **Proximity Sensor:** HC-SR04 Ultrasonic Sensor
* **Weight Sensor:** Load Cell (5kg/10kg) with HX711 Amplifier
* **User Input:** Momentary Push Button (connected to Pin 1)
* **Indicators:** Active Piezo Buzzer

## Pin Mapping
| Component | Pin | Function |
| :--- | :--- | :--- |
| **Motor R** | M2 (Shield) | Right Wheel Drive |
| **Motor L** | M1 (Shield) | Left Wheel Drive |
| **IR Left** | A3 | Left Line Detection |
| **IR Right** | A0 | Right Line Detection |
| **Ultrasonic Trig** | A5 | Trigger Pulse |
| **Ultrasonic Echo** | A1 | Echo Return |
| **Load Cell DT** | A4 | HX711 Data |
| **Load Cell SCK** | A2 | HX711 Clock |
| **Table Button** | Pin 1 (TX) | Destination Selector |
| **Buzzer** | Pin 2 | Status Audio Alerts |

## Operational Logic
1.  **Standby Mode:** The robot remains stationary until the Load Cell detects a weight greater than the 10g threshold.
2.  **Table Selection:** Press the button $N$ times to send the robot to Table $N$. The robot beeps to confirm each press.
3.  **Delivery Phase:** The robot follows the black line. It counts white-surface "cross-lines" (stations) as markers for table locations.
4.  **Obstacle Handling:** If an object is detected within 5cm, the robot stops and sounds a warning beep until the path is cleared.
5.  **Arrival:** Upon reaching the target count, the robot stops and beeps continuously.
6.  **Auto-Return/Reset:** The mission is marked complete once the weight is removed from the tray.

## Setup & Installation
1.  **Libraries:** Ensure you have the following installed in your Arduino IDE:
    * `AFMotor.h` (Adafruit Motor Shield library)
    * `HX711.h`
2.  **Calibration:** You may need to adjust `scale.set_scale(INSERT_FACTOR_HERE)` in the `setup()` function based on your specific load cell calibration.
3.  **Uploading:** **CRITICAL:** Disconnect the wire from Pin 1 (TX) while uploading the sketch to the Arduino to avoid serial communication conflicts.

---
*Developed as a course project for **ME2200 - Measurements, Instrumentation and Controls**.*
