.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Setup bossdevice research
============================================

.. toctree::

   2_setup_bossdevice_research




The bossdevice plugs into a compatible EEG amplifier on the input side and generates a fuzzy logic based binary signal on the output side. It reads a real-time biosignal data stream and generates trigger signal when a pre-defined EEG-state is detected. The bossdevice is programmable from Matlab and enables complex experiments to be fully automated. Setting up the device involves several steps and the next sections descibre their details. 

Connecting with Biosignal Amplifier
====================================
The input of the bossdevice i.e. the EEG data is provided by connecting the Biosignal Amplifier (EEG device e.g. Bittium's NeurOne or Brain Product's actiCHamp) with the bossdevice using a standard LAN Cable. Search the “REAL TIME OUT” LAN port at the back plate of your EEG device and connect it to the “Biosignal” LAN port available at the bossdevice research. You can also see the "Biosignal" port on the bossdevice research in the figure shown below.

.. figure:: figures/Figure2_biosignallanport.png
    :align: center

    Ports available on the bossdevice (see the Biosignal LAN port).

Connecting with Control PC
====================================
In order to program the bossdevice research, it needs to be connected to any standard PC hosting a Matlab and other dependencies as described in the "Downloads & Dependencies" chapter. The “Control PC” LAN port at the backplate of the bossdevice research is connected to any standard LAN/Ethernet port on the backplate of Control PC. Then in order to open the network IP address, follow these steps:

1. On Microsoft Windows 7/8/10 Click Start Menu > Control Panel > Network and Sharing Centre or Network and Internet > Network and Sharing Centre.
2. Click Change adapter settings.
3. Right-click on Local Area Connection corresponding to LAN/Ethernet port where the “Control PC” LAN cable is connected and click Properties.

.. figure:: figures/Fig3_controlpc1.png
    :align: center

    Step 3: Local Area Connection setup of Control PC and bossdevice research

4. Select Internet Protocol Version 4 (TCP/IPv4) and click Properties.

.. figure:: figures/Figure4_controlpc2.png
    :align: center

    Step 4: Local Area Connection setup of Control PC and bossdevice research

5. Select Use the following IP address. Then enter the following IP address, Subnet mask, Default gateway, and DNS server.
.. code-block::

   IP Address:          10.10.10.100
   Subnet mask:         255.255.255.0
   Default gateway:     10.10.11.88
   DNS server:          10.10.11.68

.. figure:: figures/Figure5_controlpc3.png
    :align: center

    Step 5: Local Area Connection setup of Control PC and bossdevice research

6. Click OK and OK on your way out of the wizard.

Similar approach can also be used on Linux/MAC OS to setup the requried IP network for communciation between the bossdevice research and any control PC.

Connecting with Synchronizing Devices
====================================
The bossdevice research can send up to 4 TTL trigger signals to connected synchronizing devices from its output port. Search the “TRIGGER OUT 1-4” BNC ports at back plate of the bossdevice research and connect it to the BNC ports of your intended device.

.. figure:: figures/Figure6_ttlports.png
    :align: center

    Ports available on the bossdevice (see the BNC output ports 1-4 named as TRIGGER OUT 1 till 4).

In addition to the TTL output the 8 bit trigger LPT marker ports can also be connected to the bossdevice research for a numerical input or can also be output from bossdevice research to write synchronizing markers to the raw EEG data or for any other purpose.

.. figure:: figures/Figure7_markersport.png
    :align: center

    Ports available on the bossdevice (see the Marker 1 and Marker 2 8 bit LPT ports).


Connecting with Monitor
====================================
Monitor setup is optional but recommended and it shows the useful information and analytics of the bossdevice research time of execution and errors (if any). The Display Port from backplate of the bossdevice research may be connected to the monitor.

.. figure:: figures/Figure2_biosignallanport.png
    :align: center

    Ports available on the bossdevice (see the Display port (DP) for connection to monitor).

Power Supply
====================================
The power supply provided along with the bossdevice research can be plugged into the “DC IN 12V” port at the back plate of the bossdevice research as shown in figure below.

.. figure:: figures/Figure2_biosignallanport.png
    :align: center

    Ports available on the bossdevice (see the 12V DC power supply connection port).


Switching ON and OFF
====================================
Since the device is all setup, the device can be switched on using the push button at on of the sides of bossdevice. When switched off, the LED present on the button is not illuminated while when it is switched on, a blue LED is illuminated.


