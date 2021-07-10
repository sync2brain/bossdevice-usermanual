.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Matlab API Documentation
============================================

.. toctree::

    matlab_api_documentation


Introduction
============
The bossdevice research is a real-time digital signal processor consisting of hardware and software algorithms. It is designed to read-in a real-time raw data stream from a biosignal amplifier (electroencephalography, EEG), to continuously analyze the data and to detect patterns in this data based on oscillations in different frequencies. When such a pattern is detected, the device indicates this through a standard output port. This enables a stimulation device (such as a sound generator) to be triggered in response to a specific biosignal pattern occurring. The device can be programmed by the user to detect different patterns.

The bossdevice research is not a medical device. It may not be used outside of research and it may not be used in trials involving patients. It is not intended as an accessory to a medical device or to control a medical device. It may only be connected to a stimulation device if the stimulation device provides an input port for the purpose of receiving information regarding the desired timing of stimulation. Whether or not a stimulus is then generated in response to a signal from the bossdevice is determined by the stimulation device.

---------

Getting started with bossdevice
===============================
--- add some lines here about getting statrted then 

Connecting with Biosignal Amplifier
-----------------------------------
The input of the bossdevice i.e. the EEG data is provided by connecting the Biosignal Amplifier (EEG device e.g. Bittium's NeurOne or Brain Product's actiCHamp) with the bossdevice using a standard LAN Cable. Search the “REAL TIME OUT” LAN port at the back plate of your EEG device and connect it to the “REAL TIME IN” LAN port at the back plate of the BOSS Device.

Connecting with Control PC
-----------------------------------
The “Control PC” LAN port at the backplate of the BOSS Device is connected to any LAN/Ethernet port on the backplate of Control PC. Then in order to open the network IP address, follow these steps:

1. On Microsoft Windows 7/8/10 Click Start Menu > Control Panel > Network and Sharing Centre or Network and Internet > Network and Sharing Centre.
2. Click Change adapter settings.
3. Right-click on Local Area Connection corresponding to LAN/Ethernet port where the “Control PC” LAN cable is connected and click Properties.
figure
4. Select Internet Protocol Version 4 (TCP/IPv4) and click Properties.
figure
5. Select Use the following IP address. Then enter the following IP address, Subnet mask, Default gateway, and DNS server.

IP Address:                      10.10.10.100

Subnet mask:                  255.255.255.0

Default gateway:               10.10.11.88

DNS server:                     10.10.11.68
figure
Click OK and OK on your way out of the wizard.

Connecting with Stimulator
-----------------------------------
The BOSS device can send up to 4 TTL trigger signals to connected TMS devices (output). Search the “TRIGGER OUT 1-4” BNC ports at back plate of the BOSS device and connect it to the “Trigger In” BNC port of the Stimulator.
fig
Figure: Back plate view of bossdevice (see the BNC output ports 1-4 named as TRIGGER OUT 1 till 4)

