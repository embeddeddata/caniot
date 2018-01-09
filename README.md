# caniot (Preliminary)
CAN IoT Example Project

https://github.com/embeddeddata/caniot

## TODO
* Images
* Tables
* Linux commands
* High level walkthrough

## Overview

Historically embedded communication has been handled via either low level embedded interfaces that are hard to interface with at a high level and susceptible to noise such as SPI or I2C, analog buses such as a 4-20mA that require special analog hardware and processing or the SCADA standard RS-485 with MODBUS which can be fussy with many network topologies. 

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

## Gateway Hardware
To get this up and running we will be using the [G4N03RHT](http://gps4net.com/images/gps4net/products/g4n03rht/g4n03rht_en.pdf) sensor with the [PiCAN](http://skpang.co.uk/catalog/pican2-canbus-board-for-raspberry-pi-2-p-1475.html) adapter board for a Raspberry Pi 3. 

### Setup with Raspberry Pi and CAN board:
 
* Installation
* Linux Distro: Minimal Raspbian
* Setup for PiCAN2 ($50)

## Testing CAN interface 

* via can-utils: https://github.com/linux-can/can-utils

## Python interaction on CAN Bus 

python-can: https://github.com/hardbyte/python-can

This is an optional step and depends on your application. For just this single sensor and some minimal visualizations the python-can library and a simple web based plotter or a terminal interface may be a very usable option on the Raspberry Pi. But for doing some serious data crunching and visualizations, it doesn't take too much to overwhelm the Raspberry Pi. Below we will take the data processing requirements off of the RasPi and let a more powerful computer do the work.
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
