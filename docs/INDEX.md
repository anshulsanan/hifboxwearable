# HIFBox Wearable

## Disclaimer
This project is an academic proof of concept and should not be used in professional settings. The maintainer or contributors of this project do not accept responsibility for any injury that this device could inflict.

## Introduction
This is the project page for the "Help, I've Fallen" (HIF) Box Wearable. This repository will contain information regarding the project's architecture, design, and source code required to interface with the wearable's sensors. This project will use a Raspberry Pi B+ and Adafruit's Precision NXP 9-DOF Breakout board.

## Objectives
For this hardware project I will attempt to successfully read values from the target gyroscope sensor using a Raspberry Pi B+. In order to retrieve information from the sensor in a timely fashion the Raspberry Pi will facilitate information retrieval using an I2C serial connection. Once the values are read into the Raspberry Pi a C program running locally will display the values for the user. The program will also display the readout graphically should the user choose to.  

## Hardware Platform
In order to interface with this sensor I will be using the Raspberry Pi B+, a small credit card-sized, ARM-based computing platform that runs using Raspbian. Raspbian is a distribution based off of Debian which features additional blobs required to make use of the on-board FPU. The Raspberry Pi features a 40-pin GPIO header and the ability to communicate with external serial capable devices. Serial protocols supported by the Raspberry Pi B+ are I2C, SPI, and UART.

## The Sensor
![Image of NXP 9-DOF Breakout Board] (https://cdn-shop.adafruit.com/970x728/3463-02.jpg)
The sensor for this project is [FXAS21002 3-Axis gyroscope](https://www.adafruit.com/product/3463) and is part of the [Adafruit Precision NXP 9-DOF Breakout Board](https://www.adafruit.com/product/3463). This breakout board also features a [FXOS8700 3-Axis accelerometer](://www.nxp.com/applications/solutions/internet-of-things/smart-things/healthcare/hearing-aids/digital-motion-sensor-3d-accelerometer-2g-4g-8g-plus-3d-magnetometer:FXOS8700CQ).

# Progress
## Date: October 23, 2018
So far I have acquired a Raspberry Pi B+ and have installed Raspbian successfully. SSH has been configured to allow for X11 Forwarding to facilitate the viewing of graphical applications on the controlling machine. Unfortunately, at this time, I am unable to acquire the gyroscope breakout board from Adafruit because they are out of stock. A reseller called Elmwood Electronics offers the [same breakout board](https://elmwoodelectronics.ca/products/adafruit-precision-nxp-9-dof-breakout-board-fxos8700-fxas21002), however they are also sold out. I've been watching both sellers daily to seize the item as soon as they restock, but I haven't had any luck. In attempt to catch the item restocked I've also added myself to a email notification subscription for both sellers.
![email](https://user-images.githubusercontent.com/43853823/47387152-0c3a8880-d6fe-11e8-9f10-8310a742f904.png)
For the time being I will be writting the application for the target ARM platform and analyzing the communications using an oscilloscope. If the breakout board does not become available in the near future I will have to either order the gyroscope standalone and build the breakout board, or use a similar sensor breakout board that's available.
