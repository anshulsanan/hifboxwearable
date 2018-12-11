# Hifbox wearable

## Introduction
This is the project page for the "Help, I've Fallen" (HIF) Box Wearable. This repository will contain information regarding the project's architecture, design, and source code required to interface with the wearable's sensors. This project will use a Raspberry Pi B+ and NXP's 9-DOF Breakout board.

## Disclaimer
This project is an academic proof of concept and should not be used in professional settings. The maintainer or contributors of this project do not accept responsibility for any injury that this device inflicts on end users. 

# Build Instructions
The sections below describe the build instructions I followed in order to have a working wearable prototype for demo purposes. I will do my best to outline the instructions I followed and some of the general concepts behind the choices I made. Some of the instructions I mention may not work for you and if that is the case the reference materials mentioned at the bottom of the section should point you in the right direction.

## 1. Prerequisites and scope
### Objectives
This prototype will allow you to successfully read values from the target gyroscope sensor using a Raspberry Pi B+. In order to retrieve information from the sensor in a timely fashion the Raspberry Pi will retrieve data using an I2C serial connection. Once the values are read into the Raspberry Pi, a Python program running locally on the Raspberry Pi will display the values to the user.

### Hardware Platform
In order to interface with the sensor I will be using the [Raspberry Pi B+](https://www.raspberrypi.org/products/raspberry-pi-1-model-b-plus/), a small credit card-sized, ARM-based computing platform that is loaded with Raspbian. Raspbian is a distribution based off of Debian GNU/Linux which features additional blobs required to make use of the on-board FPU. The Raspberry Pi features a 40-pin GPIO header and the ability to communicate with external serial capable devices. Serial protocols supported by the Raspberry Pi B+ are I2C, SPI, and UART. If you choose to use a different Raspberry Pi version check the associated documentation to ensure compatability.

### The Sensor
![Image of NXP 9-DOF Breakout Board](https://user-images.githubusercontent.com/43853823/47609574-7a898e80-da0f-11e8-8cb0-d7fec0fdcf10.png)

The sensor for this project is the [FXAS21002 3-Axis gyroscope](https://cache.freescale.com/files/sensors/doc/data_sheet/FXAS21002.pdf) and is part of the [Freescale Freedom 9-axis Sensor Toolbox Development Board](http://cache.freescale.com/files/sensors/doc/support_info/FRDM-STBC-AGM01-QRC.pdf). This breakout board also features a [FXOS8700 3-Axis accelerometer and 3-Axis magnetometer](https://www.nxp.com/applications/solutions/internet-of-things/smart-things/healthcare/hearing-aids/digital-motion-sensor-3d-accelerometer-2g-4g-8g-plus-3d-magnetometer:FXOS8700CQ).

This development board is built around the [BRKT-STBC-AMG01](http://cache.freescale.com/files/sensors/doc/support_info/BRKT-STBC-AGM01-QRC.pdf) breakout board. More information about this breakout board can be found [here](https://www.nxp.com/downloads/en/schematics/BRKT-STBC-AGM01-SCH.pdf).

## 2. Raspberry Pi Setup & Configuration
In order to obtain the ability to have a program run on the Raspberry Pi we will need to image the Raspberry Pi with the latest version of Raspbian. Raspbian can be obtained at the official [Raspberry Pi website](https://www.raspberrypi.org/downloads/raspbian/). For my build I decided on using the Lite image because I didn't require the extras that come with the desktop or N00bs images. For our purposes you will need a working SSH connection and the basic networking service which is installed by default in the Lite image. Once you've downloaded the .img file you will need to unzip the file into a directory and write the contents of the file to your target SD card. There are several methods for imaging the SD card depending on the OS you are using.

### \*NIX Setup
On UNIX systems with GNU coreutils installed you can use the "dd" utility to write the contents of the Raspbian image file to your target SD card. Once you have downloaded the image to you local filesystem you can use the dd utility in a terminal emulator as follows: 

```dd if=<Raspbian image file> of=<SD card block device> bs=512 oflag=sync status=progress``` 

This command will have to executed as root or a user with permissions to write to disks. If executed with the appropriate privileges and correct arguments the filesystem image from the Raspbian image file should start writing to the SD card. 
If something goes wrong dd should return the error it encountered to you. Note: if you don't add the "status=progress" flag no status output will return to the controlling terminal until execution has completed. You may also have to adjust the "bs" flag to match the I/O block size of your SD card.

```xxx+0 records in
yyy+0 records out
yyyyyyyyyy bytes (yyy kB, yyy KiB) copied, 0.00144744 s, 283 MB/s
```

After the utility has finished writing the contents of the image file to your SD card it should display the number of bytes written to the SD card. Next we will configure the SD card to allow SSH to be enabled by default. For more information about writing the Raspbian distribution to an SD card please refer to [this documentation](https://www.raspberrypi.org/documentation/installation/installing-images/linux.md) from the Raspberry Pi foundation.

### Windows Setup
Windows systems do not come equipped with file copy tool equivalent to dd, so in order to write the contents of the Raspbian image file to the SD card you will need to download and install a third-party tool called "Win32DiskImager" from [here](https://sourceforge.net/projects/win32diskimager/). Once the tool has been downloaded and installed you will need to ensure that the SD card does not have any existing partitions because Windows has quirking volume management that can sometimes causes improper writing to devices that have an existing partition table.

In order to clear any existing partitions we will use DISKPART, a disk management tool available on all modern Windows systems. To use DISKPART you will need to open "Command Prompt" as Administrator. Once "Command Prompt" type in "diskpart" and wait until it has opened. It will block until the utility has populated all of the volumes and devices that are currently connected to your system. Once opened you will need to select the SD card from the list of devices. After the correct device has been selected type in "clean" to wipe the partition header on the SD card. For more information about this process please consult [this fine documentation](http://knowledge.seagate.com/articles/en_US/FAQ/005929en) from Seagate on clearing a device parititon using DISKPART and the Windows Disk Management Tool.

After wiping the target SD card can now write the image file to the SD card by opening "Win32DiskImager", selecting the correct device volume letter, the Raspbian image file, and clicking "Write" to start the write process. Once this has completed successfully a dialog notifying you of the successful write will appear, otherwise a dialog outlining the failure will interrupt the write process. For more information about his process checkout the [documentation](https://www.raspberrypi.org/documentation/installation/installing-images/windows.md) for writing Raspbian using Windows available on the Raspberry Pi foundation's website. You should now have a SD card with the Raspbian filesystem setup. Next we will be configuring SSH so that it is available on boot. 

## SSH Configuration
In order to have the SSH server accesible from the first boot you will need to make a small configuration to the filesystem on the SD card. According to the Raspbian [documentation](https://www.raspberrypi.org/documentation/remote-access/ssh/) SSH is disabled by default to shrink the attack surface on the Raspberry Pi. To enable SSH you will need to create an empty file with no file extension called "ssh" on the root of the boot partition. This partition is accessible on both Windows and \*NIX based systems that are capable of reading FAT32 partitions. Be extra careful when creating the file to not accidentally append a file extension as this will cause the SSH to not startup when the Raspberry Pi boots up. For more information about this process please consult step 3 from [these](https://www.raspberrypi.org/documentation/remote-access/ssh/) instructions on the Raspberry Pi foundation's website.

## 3. Networking Configuration
For this project I had the Raspberry Pi B+ which happens to not have any builtin wireless capabilities. I could have used a wireless USB NIC to allow for network communications between the Internet and Ethernet for my controlling PC, however a more elegant solution exists. In order to have a working Internet connection on the Rasberry Pi I created a shared connection to the interface on my controlling PC that has an active Internet connection on a separate network interface. The following sections below will show two possible methods for configuring this type of networking on systems with "NetworkManager" and Windows systems. By default the \*NIX settings below will assign a random IPv4 address in the 10.0.0.0/24 address space to the Ethernet interface connected to your Raspberry Pi. Windows will assign the IPv4 address of the Ethernet NIC connected to the Raspberry PI to the address 192.168.137.1/24. You will need to configure the Raspberry Pi to use an address on this same subnet to allow for easy communications, however if you choose not to do the static configuration the Raspberry Pi will automatically self assign a random IPv4 address in the same subnet.

### \*NIX Networking
NetworkManager is a service that can allow you to configure the network settings for a particular system. In order to allow the the Ethernet connection to the Raspberry Pi to forward the Internet connection to another interface that has Internet connectivity you will need to open the NetworkManager settings panel and select the wired connection profile that will be used to connect to your Raspberry Pi. Once you have selected the profile go to the IPv4 settings tab and using the "Method" dropdown select "Shared to other computer". 

### Windows Networking
Windows has a builtin Network Sharing service that allows for network connection sharing. This utility can be found in "Network and Sharing Center" via the "Control Panel" or by right-clicking the networking icon in the windows taskbar. From the "Network and Sharing Center" you will need to go to the "Advanced Settings", right-click the interface with the Internet connection you would like to share, click "Properties", and under the "Sharing" tab check the first check box only. After several seconds the networking settings for the adjacent Ethernet interface that is being used should be set to 192.168.137.1. If you have multiple interfaces other than the Internet capable interface and the Ethernet interface connecting to the Raspberry Pi you will need to set the "Home Networking Connection" field below the checkbox to the Ethernet interface name that is connected to the Raspberry Pi.

## Raspberry Pi Networking
The machine you're using to connect the Raspberry Pi should now be able to connect to the Raspberry Pi and allow an external Internet connection, but before you start working with the Raspberry Pi you will need to either find out the IP address that the Raspberry Pi self-assigns using a networking mapping tool or if you're using an operating system that has the drivers for interacting with EXT4 partitions you can manually configure the networking configuration before booting up.

### Scanning for The Raspberry Pi
To scan for the Raspberry Pi we will be using "nmap". Nmap can be obtained from [here](https://nmap.org/download.html) or through your Linux distributions package manager. Once installed you can run this command to scan your subnet to find the Raspberry Pi's IPv4 address.

```nmap -n -T3 -sP <CIDR network address>```

Be sure to insert the correct CIDR network address for the subnet that you are using for your connection. Once the command finishs it should have your Raspberry Pi's current IPv4 address somewhere in the output. You can then use an SSH client of you choice to connect to it. The default user is "pi" and the default password is "raspberry". More methods of IP address discovery can be found in the official [documentation](https://www.raspberrypi.org/documentation/remote-access/ip-address.md) from the Raspberry Pi foundation.

The caveat with this approach is that the IPv4 address of the Raspberry Pi might change when the DHCP lease from your controlling machine expires. You will then have to action this process again to find the new IPv4 address of the Raspberry Pi in order to make a successful connection.

### Manual Networking Configuration
To maintain a level of consistency when interacting with your Raspberry Pi you can preset a static IPv4 address to the Raspberry Pi's Ethernet interface. This will allow you to use the same IPv4 address consistently.

## 4. PCB Setup

## 5. PCB Soldering 

## 6. I2C programming
