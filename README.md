# Hifbox wearable

## Introduction
This is the project page for the "Help, I've Fallen" (HIF) Box Wearable. This repository will contain information regarding the project's architecture, design, and source code required to interface with the wearable's sensors. This project will use a Raspberry Pi B+ and Adafruit's Precision NXP 9-DOF Breakout board.

## Objectives
For this hardware project I will attempt to successfully read values from the target gyroscope sensor using a Raspberry Pi B+. In order to retrieve information from the sensor in a timely fashion the Raspberry Pi will facilitate information retrieval using an I2C serial connection. Once the values are read into the Raspberry Pi a C program running locally will display the values for the user. The program will also display the readout graphically should the user choose to.  

## Disclaimer
This project is an academic proof of concept and should not be used in professional settings. The maintainer or contributors of this project do not accept responsibility for any injury that this device could inflict.
