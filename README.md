# Hifbox wearable

## Introduction
This is the project page for the "Help, I've Fallen" (HIF) Box Wearable. This repository will contain information regarding the project's architecture, design, and source code required to interface with the wearable's sensors. This project will use a Raspberry Pi B+ and NXP's 9-DOF Breakout board.

## Disclaimer
This project is an academic proof of concept and should not be used in professional settings. The maintainer or contributors of this project do not accept responsibility for any injury that this device inflicts on end users. 

## Objectives
For this hardware project I will attempt to successfully read values from the target gyroscope sensor using a Raspberry Pi B+. In order to retrieve information from the sensor in a timely fashion the Raspberry Pi will retrieve raw data using an I2C serial connection. Once the values are read into the Raspberry Pi, a C program running locally will display the values for the user. The program will also display the readout graphically should the user choose to.  

## Hardware Platform
In order to interface with the sensor I will be using the Raspberry Pi B+, a small credit card-sized, ARM-based computing platform that is loaded with Raspbian. Raspbian is a distribution based off of Debian GNU/Linux which features additional blobs required to make use of the on-board FPU. The Raspberry Pi features a 40-pin GPIO header and the ability to communicate with external serial capable devices. Serial protocols supported by the Raspberry Pi B+ are I2C, SPI, and UART.

## The Sensor
![Image of NXP 9-DOF Breakout Board](https://user-images.githubusercontent.com/43853823/47609574-7a898e80-da0f-11e8-8cb0-d7fec0fdcf10.png)
The sensor for this project is the [FXAS21002 3-Axis gyroscope](https://cache.freescale.com/files/sensors/doc/data_sheet/FXAS21002.pdf) and is part of the [Freescale Freedom 9-axis Sensor Toolbox Development Board](http://cache.freescale.com/files/sensors/doc/support_info/FRDM-STBC-AGM01-QRC.pdf).
This breakout board also features a [FXOS8700 3-Axis accelerometer and 3-Axis magnetometer](https://www.nxp.com/applications/solutions/internet-of-things/smart-things/healthcare/hearing-aids/digital-motion-sensor-3d-accelerometer-2g-4g-8g-plus-3d-magnetometer:FXOS8700CQ).

