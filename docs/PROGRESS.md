# Progress
## Date: November 15, 2018
The prototyping board is in working order, however not without several hiccups along the way. At the moment the Raspberry Pi can read and display the data from the sensor board without any issues. 

![demo](https://user-images.githubusercontent.com/43853823/48652144-fdc55100-e9cb-11e8-81e0-1c522f582454.png)

A Python script was used with a modified version of Adafruits CircuitPython library to facilitate reading the raw values.

It took two attempts to create a working PCB board. In my first attempt I had incorrectly designed the PCB by not including vias to route the copper traces to one side of the headers. This resulted in the traces which were disconnected from the headers. Another quirk of the first PCB was broken copper joints on most of the traces. It is speculated that a bug in Fritzing is responsible for generating Gerber files with broken trace joints.

The second PCB did not have any manufacturing issues, however during the soldering process I had removed the copper traces where the pin headers were supposed to go. To remedy this issue I simply ran soldering from the header pin to the end of the copper trace.

![stackednocase](https://user-images.githubusercontent.com/43853823/48652090-c6ef3b00-e9cb-11e8-9239-3a9147fd1ef0.jpg)

## Date: November 04, 2018
![fritzimage_schem](https://user-images.githubusercontent.com/43853823/47970091-c6ab8300-e04e-11e8-9fb5-d1d054d36639.png) 

The schematic for the interface board has been sketched and sent to the prototyping lab to be printed. The development board I will be creating a interface board the resembles the Arduino Uno R3. This design choice was made in order to follow the natural layout of the development board.

## Date: October 30, 2018
The development board has arrived in the mail. The board unfortunately doesn't include detailed instructions or documentation and relies on the datasheets for the [FXAS21002 3-Axis gyroscope](https://cache.freescale.com/files/sensors/doc/data_sheet/FXAS21002.pdf) and [FXOS8700 3-Axis accelerometer and 3-Axis magnetometer](https://www.nxp.com/applications/solutions/internet-of-things/smart-things/healthcare/hearing-aids/digital-motion-sensor-3d-accelerometer-2g-4g-8g-plus-3d-magnetometer:FXOS8700CQ). I was successfully able to connect to the board, however the I2C address for the gyroscope is slightly off. The I2C interface can be configured to use address 0x20 or 0x21 depending on the voltage supplied to pin 8 of the FXAS21002 and the SA0 MISO pin. For my sensor I will have to route an external connection to the 3.3V source to use address 0x21. The interface board for this sensor can be similar to the bridge used on the Raspberry Pi B+ to Arduino Uno R3 because the pinout for the sensor development board is compatible with the Arduino Uno R3. No additional expenses have been added to the total cost of this board.

## Date: October 27, 2018
I have successfully purchased a development board that features the FXAS21002 3-Axis gyroscope. This is a [development board](https://www.digikey.com/products/en?keywords=FRDM-STBC-AGM01) from NXP Semiconductors that features the FXAS21002 3-Axis gyroscope and FXOS8700C 6-axis sensor with a integrated linear accelerometer and magnetometer. These additional sensors will be used in the following semester to implement fall detection. The total cost of the order is $36.06 USD with $20.00 USD for the shipping costs. This order is expected to arrive by the 29th of October. To ensure I receive a working development board I have ordered two in the event one is defective. Information regarding the board can be found [here](http://cache.freescale.com/files/sensors/doc/support_info/FRDM-STBC-AGM01-QRC.pdf).

## Date: October 23, 2018
So far I have acquired a Raspberry Pi B+ and have installed Raspbian successfully. SSH has been configured to allow for X11 Forwarding to facilitate the viewing of graphical applications on the controlling machine. Unfortunately, at this time, I am unable to acquire the gyroscope breakout board from Adafruit because they are out of stock. A reseller called Elmwood Electronics offers the [same breakout board](https://elmwoodelectronics.ca/products/adafruit-precision-nxp-9-dof-breakout-board-fxos8700-fxas21002), however they are also sold out. I've been watching both sellers daily to seize the item as soon as they restock, but I haven't had any luck. In attempt to catch the item restocked I've also added myself to a email notification subscription for both sellers.
![email](https://user-images.githubusercontent.com/43853823/47387152-0c3a8880-d6fe-11e8-9f10-8310a742f904.png)
For the time being I will be writting the application for the target ARM platform and analyzing the communications using an oscilloscope. If the breakout board does not become available in the near future I will have to either order the gyroscope standalone and build the breakout board, or use a similar sensor breakout board that's available.
No additional expenses have been required for the construction of this project. So far all materials used were acquired some time before the project's inception.

## Date: October 16, 2018
The Raspbian distribution for the Raspberry Pi has been successfully flashed to the target SD. Intially I experienced issues with the SSH server's configuration file. Since the configuration file was modified incorrectly when the service was tasked to reload it failed to start, even when enabled to boot during startup.
