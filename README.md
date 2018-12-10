# Hifbox wearable

## Introduction
This is the project page for the "Help, I've Fallen" (HIF) Box Wearable. This repository will contain information regarding the project's architecture, design, and source code required to interface with the wearable's sensors. This project will use a Raspberry Pi B+ and NXP's 9-DOF Breakout board.

## Disclaimer
This project is an academic proof of concept and should not be used in professional settings. The maintainer or contributors of this project do not accept responsibility for any injury that this device inflicts on end users. 

# Build Instructions
The sections below describe the build instructions I followed in order to have a working wearable prototype for demo purposes. I will do my best to outline the instructions I followed and some of the general concepts behind the choices I made. Some of the instructions I mention may not work for you and I that is the case the reference materials mentioned at the bottom of the section should point you in the right direction.

## 1. Prerequisites and scope
### Objectives
This prototype will allow you to successfully read values from the target gyroscope sensor using a Raspberry Pi B+. In order to retrieve information from the sensor in a timely fashion the Raspberry Pi will retrieve raw data using an I2C serial connection. Once the values are read into the Raspberry Pi, a C program running locally will display the values for the user.

### Hardware Platform
In order to interface with the sensor I will be using the Raspberry Pi B+, a small credit card-sized, ARM-based computing platform that is loaded with Raspbian. Raspbian is a distribution based off of Debian GNU/Linux which features additional blobs required to make use of the on-board FPU. The Raspberry Pi features a 40-pin GPIO header and the ability to communicate with external serial capable devices. Serial protocols supported by the Raspberry Pi B+ are I2C, SPI, and UART. If you choose to use a different Raspberry Pi version check the associated documentation to ensure compatability.

### The Sensor
![Image of NXP 9-DOF Breakout Board](https://user-images.githubusercontent.com/43853823/47609574-7a898e80-da0f-11e8-8cb0-d7fec0fdcf10.png)

The sensor for this project is the [FXAS21002 3-Axis gyroscope](https://cache.freescale.com/files/sensors/doc/data_sheet/FXAS21002.pdf) and is part of the [Freescale Freedom 9-axis Sensor Toolbox Development Board](http://cache.freescale.com/files/sensors/doc/support_info/FRDM-STBC-AGM01-QRC.pdf). This breakout board also features a [FXOS8700 3-Axis accelerometer and 3-Axis magnetometer](https://www.nxp.com/applications/solutions/internet-of-things/smart-things/healthcare/hearing-aids/digital-motion-sensor-3d-accelerometer-2g-4g-8g-plus-3d-magnetometer:FXOS8700CQ).

This development board is built around the [BRKT-STBC-AMG01](http://cache.freescale.com/files/sensors/doc/support_info/BRKT-STBC-AGM01-QRC.pdf) breakout board. More information about this breakout board can be found [here](https://www.nxp.com/downloads/en/schematics/BRKT-STBC-AGM01-SCH.pdf).

## 2. Raspberry Pi Setup & Configuration
In order to obtain the ability to have a program run on the Raspberry Pi we will need to image the Raspberry Pi with the latest version of Raspbian. Raspbian can be obtained at the official [Raspberry Pi website](https://www.raspberrypi.org/downloads/raspbian/). Once downloaded you will need to unzip the ".img" file into a directory and write the contents of the file to your target SD card. There are several methods for imaging the SD card depending on the OS you are using.

### \*NIX Setup
On UNIX systems with GNU coreutils installed you can use the "dd" utility to write the contents of the Raspbian img file to your target SD card. Once you have downloaded the image to you local filesystem you can use the "dd" utility in a terminal emulator as follows: dd if=<Raspbian image file> of=<SD card block device> bs=512 oflag=sync status=progress. If executed with the appropriate privileges and correct arguments the filesystem image from the Raspbian image file should start writing to the SD card. If something goes wrong "dd" should return the error to you. Note: if you don't add the "status=progress" flag no status output will return to the controlling terminal until execution has completed. You may also have to adjust the "bs" flag to match the I/O block size of your SD card.

After the utility has finished writing the contents of the image file to your SD card it should display the number of bytes written to the SD card. Next we will configure the SD card to allow SSH to be enabled by default.

### Windows Setup
Windows systems do not come equipped with file copy tool equivalent to "dd", so in order to write the contents of the Raspbian image file to the SD card you will need to download and install a third-party tool called "Win32DiskImager" from here. Once the tool is installed you will need to ensure that the SD card does not have any existing partitions.

In order to clear any existing partitions we will use DISKPART, a disk management tool available on all modern Windows systems. To use DISKPART you will need to open "Command Prompt" as Administrator. Once "Command Prompt" type in "diskpart" and wait until it has opened. Once opened you will need to select the SD card from the list of devices. After the correct device has been selected type in "clean" to wipe the partition header on the SD card.

You can now write the image file to the SD card by opening "Win32DiskImager", selecting the correct device volume letter, the Raspbian image file, and clicking "Write" to start the write process. Once this has completed successfully a dialog notifying you of the successful write will appear, otherwise a dialog outlining the failure will interrupt the write process. You should now have a SD card with the Raspbian filesystem setup. Next we will be configuring SSH so that it is available on boot. 

## SSH Configuration
In order to have the SSH server accesible from the first boot you will need to make a small configuration to SD card. According to the Raspbian documentation SSH is disabled by default to mitigate the attack surface on the Raspberry Pi. To enable SSH you will need to create an empty file with no file extension called "ssh" on the root of the boot partition. This partition is available on both \*NIX based and Windows systems. Be extra careful when creating the file to not accidentally append a file extension as this will cause the SSH to not appear.

## 3. Networking Configuration
For this project I had the Raspberry Pi B+ which happens to not have any builtin wireless capabilities. I could have used a wireless USB NIC to allow for network communications between the Internet and Ethernet for my controlling PC, however a more elegant solution exists. In order to have a working Internet connection on the Rasberry Pi I created a bridged connection to the interface on my controlling PC that has an active Internet connection was made. The following sections below will show two possible methods for configuring this type of networking on systems with NetworkManager and Windows machines.

### \*NIX Networking

### Windows Networking

## 4. PCB Setup

## 5. PCB Soldering 

## 6. I2C programming
