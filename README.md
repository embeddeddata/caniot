# caniot (Preliminary)
[CAN IoT Example Project](https://github.com/embeddeddata/caniot)

## TODO
* Images
* Linux commands
* Python commands
* Influx DB
* Grafana

## Overview

Historically embedded communications have been handled via either low level embedded interfaces that are hard to interface with at a high level and susceptible to noise such as SPI or I2C, analog buses such as a 4-20mA that require special analog hardware and processing or the SCADA standard RS-485 with MODBUS which can be fussy with many network topologies. 

As electronics have gotten more powerful, the need for these devices to communicate in a simple, robust, and scalable way has gotten more and more important. Car manufacturers have been one of the first to adopt the CAN bus as it is one of the most resilient communication interfaces out there for operating in such a demanding environment. Now we are starting to see CAN crop up in more and more things as the advantages of using a CAN start to win out.

## CAN Bus  

A CAN (Controller Area Network) is a differential bus interface to an embedded device. It operates rather similar to USB to handle environmental noise, however arbitration on the bus is built in at the signal level which means it requires no other hardware to expand the device count on the bus, just wire in your device and you are ready to go! 

### Distance vs Bit Rate

<center>

|Bit rate|Bus length|
|----|---:|
|1 Mbit/s   |25 m         
|800 kbit/s |50 m         
|500 kbit/s |100 m   
|250 kbit/s |250 m    
|125 kbit/s |500 m        
|50 kbit/s  |1000 m   
|20 kbit/s  |2500 m  
|10 kbit/s  |5000 m 

</center>

### Sensor
**Custom**

The most economical way to add CAN bus sensing to a project is to add it into a microcontroller that is already going to be in the system. You can get up and going to experiment with an [STM32 Olixduino](https://www.olimex.com/Products/Duino/STM32/OLIMEXINO-STM32/open-source-hardware) which already has an on-board CAN Bus transceiver to interface with the integrated CAN peripheral on an STM32. However the CAN Bus does tend to be one of the more complicated peripherals on these types of controllers so we will leave those details for another article. I would suggest using one of the integrated sensors below for now. 

**Integrated Sensor**

These kinds of sensors are a bit more expensive as they have the hardware and MCU built in, but they are very handy and could be perfect for many applications. 

* [G4N03RHT](http://gps4net.com/images/gps4net/products/g4n03rht/g4n03rht_en.pdf)
* [STW Temperature Transmitter T01-CAN](https://www.stw-technic.com/wp-content/uploads/2015/08/T01_ManualJ1939.pdf)
* Others?

## Gateway Device Setup
To get this up and running we will be using the [G4N03RHT](http://gps4net.com/images/gps4net/products/g4n03rht/g4n03rht_en.pdf) sensor with the [PiCAN](http://skpang.co.uk/catalog/pican2-canbus-board-for-raspberry-pi-2-p-1475.html) adapter board for a Raspberry Pi 3. 

### Setup with Raspberry Pi and CAN board:
 
To get started with the Raspberry Pi as a CAN Bus Gateway you need an operating system. [Raspian Lite](https://www.raspberrypi.org/downloads/raspbian/) is recommended as you shouldn't need a terribly complex setup, however a desktop version would work just as well. There are plenty of [guides](https://www.raspberrypi.org/documentation/installation/installing-images/README.md) on getting an SD card setup with the images for your OS.

Next you will need something to interface with the CAN Bus. The best and easiest way I have found is to use the [PiCAN2](http://copperhilltech.com/pican-2-can-interface-for-raspberry-pi-2-3/) adapter board. There are versions available that have an [integrated power supply](http://copperhilltech.com/pican2-can-interface-for-raspberry-pi-2-3-with-smps/) to power the device from the CAN Bus itself, as well as dual CAN adapters if that is something you might need.

**Hardware install**

[Here](http://skpang.co.uk/catalog/images/raspberrypi/pi_2/PICAN2SMPSUGB.pdf) is a document to walk you through the setup of the adapter board. Basically just press it onto the header, install the standoffs, and screw it together. **MOST IMPORTANT**, be sure to set the solder jumpers as needed, you will likely need to jumper them to work with CAN Cables unless you would like to use this in an OBDII setup.

**OS Config**

Of course now we need to setup Raspbian to detect our freshly installed adapter board!

First we need to update the OS with the typical:
```
sudo apt-get update
sudo apt-get upgrade
```
Next we need to add in the overlay to interface with the CAN bus via the SPI interface that is driving the CANPi2. Open up an editor to `/boot/config.txt` and add the following at the end:

```
dtparam=spi=on 
dtoverlay=mcp2515-can0-overlay,oscillator=16000000,interrupt=25 
dtoverlay=spi-bcm2835-overlay
```
That's it! Save your changes and reboot the Raspberry Pi: `sudo reboot`

**Wiring**

Of course we need to wire this system up. Each sensing device may be a little different, but typically you will need +12V DC, Ground, and CAN High and CAN Low. Sometimes you will have a separate CAN GND which you should use if available.

*schematic image*

The Raspberry Pi will need a matching connections on the PiCAN board for CAN High, CAN Low and CAN Ground (use the sensor ground if no dedicated CAN Ground). The Raspberry Pi itself will just need USB power if it isn't powered from the CAN Bus, and Ethernet unless you would like to setup a USB Wifi interface.

*actual wired image*

## Testing Your CAN interface 
### Command line

* http://www.skpang.co.uk/dl/can-test_pi2.zip
    * (Is this special or different from can-utils??)
* via can-utils: https://github.com/linux-can/can-utils

### Python interaction on CAN Bus 

python-can: https://github.com/hardbyte/python-can

This is an optional step and depends on your application. For just this single sensor and some minimal visualizations the python-can library and a simple web based plotter or a terminal interface may be a very usable option on the Raspberry Pi. But for doing some serious data crunching and visualizations, it doesn't take too much to overwhelm the Raspberry Pi using the tools in this article. Below we will take the data processing requirements off of the RasPi and let a more powerful computer do the work.
* Code snippets based on examples to get raw data

## SocketCANd setup and configuration for Raspberry Pi: 
* https://github.com/dschanoeh/socketcand

SocketCAN is a standard that is used to allow network access to a CAN bus. Socketcand packages this all up and gives us a nice interface to setup the interface and run it in the background as a daemon.  This will allow us to get the raw data off of the CAN Bus and send it to another computer.
* Steps to clone, compile, make, execute
* Set to run on boot

## Setting up a simple socket interface with Python from another device/computer
## Scaling raw CAN data to J1939 or other format
## Sending scaled data to a TSDB and basic queries 
https://www.influxdata.com/
## Setting up Grafana and linking to InfluxDB
https://grafana.com/

## Cost Breakdown for an Industrial Grade CAN Bus Environmental Monitoring setup

* CAN Bus Sensor
    * Integrated CAN Device: $85-$150
    * Custom: $30 or less
* Raspberry Pi: $30
    * SD Card: $10
    * Power USB (optional): $5
* CAN Adapter Board w/Enclosure: $85
* Total : ~$155->$215

## Potential Optimizations

* Read-only OS for power safe operation
* Larger CAN bus for multi point sensing
* Sensor Meta Data?? Location/type/ranges
* J1939 detail abstraction with SQLite
* CAN Bus Writing and simulation
* InfluxDB Data optimization, retention
* Filter levels and sampling with Grafana
