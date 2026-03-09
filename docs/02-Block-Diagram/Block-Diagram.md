---
title: Module's Block Diagram
tags:
- tag1
- tag2
---

## Overview
This system uses a PIC18F47Q10 microcontroller and an ESP32 Wi-Fi module to control and monitor a motor over a wireless network. The PIC controls the motor driver to set the motor’s direction, turns an LED on or off to indicate the system is working, and reads a hall-effect sensor to measure the motor's rotation. The ESP32 communicates with the PIC and connects to an MQTT server over Wi-Fi, sending motor status and sensor data and receiving control commands from other devices. This setup allows the motor to be controlled remotely while providing feedback on its operation.


## Block Diagram 


<img width="1220" height="920" alt="image" src="https://github.com/user-attachments/assets/989ff5a9-2dde-4705-8301-b5b6d595870f" />



