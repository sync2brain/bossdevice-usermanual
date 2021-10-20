.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Setup bossdevice RESEARCH
============================================

.. toctree::

   2_setup_bossdevice_research




The bossdevice plugs into a compatible EEG amplifier on the input side and generates a fuzzy logic based binary signal on the output side. It reads a real-time biosignal data stream and generates a  signal when a pre-defined EEG-state is detected. The bossdevice is programmable from Matlab and enables complex experiments to be fully automated. Setting up the device involves several steps and the next sections descibre their details. 

Pre-requisites
====================================
-  Windows 10 operating system
-  MatLab R2021a
-  Simulink for MatLab R2021a update release 5
-  Simulink Coder add-on
-  Simulink Real-time add on
-  Simulink Real-Time target support package (add on)
-  The bossdevice RESEARCH API (download from here). The API contain the Matlab classes needed to control the device as well as a few MatLab demo scripts as example of codes that could be implemented to work with the device.
-  The bossdevice RESEARCH firmware, accessible at sync2brain.com/downloads.
-  Hardware drivers named as Speedgoat I/O Blockset R2021a (v9.2.2.1) drivers, accessible at sync2brain.com/downloads.
-  Optional: a screen (recommended in case of troubleshooting)


Connecting bossdevice with Control PC
====================================
In order to program the bossdevice RESEARCH, it needs to be connected to any standard PC hosting a Matlab and other dependencies as described in the "Downloads & Dependencies" chapter and Pre-requistes section above. The “Control PC” LAN port at the backplate of the bossdevice RESEARCH is connected to any standard LAN/Ethernet port on the backplate of Control PC. Then in order to open the network IP address, follow these steps:

1. On Microsoft Windows 7/8/10 Click Start Menu > Control Panel > Network and Sharing Centre or Network and Internet > Network and Sharing Centre.
2. Click Change adapter settings.
3. Right-click on Local Area Connection corresponding to LAN/Ethernet port where the “Control PC” LAN cable is connected and click Properties.

.. figure:: figures/Fig3_controlpc1.png
    :align: center

    Step 3: Local Area Connection setup of Control PC and bossdevice RESEARCH

4. Select Internet Protocol Version 4 (TCP/IPv4) and click Properties.

.. figure:: figures/Figure4_controlpc2.png
    :align: center

    Step 4: Local Area Connection setup of Control PC and bossdevice RESEARCH

5. Select Use the following IP address. Then enter the following IP address, Subnet mask, Default gateway, and DNS server.
.. code-block::

   IP Address:          192.168.7.10
   Subnet mask:         255.255.255.0
   Default gateway:     
   DNS server:          

6. Click OK and OK on your way out of the wizard.


Simulink Real-Time Target Support Package
=========================================
When starting Matlab 2021 release 5 for the first time, additional installations are needed.
In the Home tab, click on Add-Ons. Go to Get Add-ons and look for Simulink Real-Time Support Package. Install it, it will ask you to log in with your MatLab credentials.

Download & Install Hardware Drivers
======================================
Specific drivers for the hardware of bossdevice RESEARCH  must be downloaded installed at the first time using bossdevice RESEARCH with the new installation of Matlab, specifically the Speedgoat I/O Blockset for R2021a (v9.2.2.1). In order to download the drivers, visit sync2brain.com/downloads and download the Speedgoat I/O Blockset. When downloaded, unzip the file and save it in a temporary folder that must be deleted from the computer after the installation have been completed.   

The hardware drivers must be installed manually. Go into the unzipped drivers’ folder from Matlab path search, right click on the executable in there and run the installation (a documentation page will pop up, just close it as not necessary). When asked by Matlab, accept the waiver and type Y (i.e., yes) twice; the second Y will close the software as Matlab needs to be restarted after this installation.
At the end of these two installations, reopen Matlabv and type Speedgoat to check that everything ran smoothly. If successful, a hardware dedicated window of speedgoat should pop up with different options available (e.g., troubleshoot, support, etc.).

Windows Firewall Check
======================
First, make sure all the network communication for Matlab R2021a are all allowed in the Windows Defender Firewall. To do this, search for Windows Defender Firewall with Advanced Security and look for Matlab R2021a Inbound and Outbound rules: if they are blocked, right click and Allow them. If they are already allowed, exit and continue to the next step.


Software Update on bossdevice
=============================
This step is also needed only once in the beginning of setup. In Matlab, type slrtExplorer: a new window should pop up. If you want, change the name of the device (meaningful could be bossdevice), and click on Software Update (not a necessity but still good step just in case). When the update is finished, click on the Disconnected button at the top left corner of the window. A connection (shown by a green symbol) should be established. If this is not the case, the sync2brain support team should be contacted for support.

Please note: if the bossdevice RESEARCH main screen shows a Matlab version different from Matlab R2021a, a major update is necessary. In this case, the sync2brain team should be contacted for support. After the update is made, make sure to switch off and on the device for complete update.


Configure biosignal Ethernet port of bossdevice
===============================================
The connection between the bossdevice RESEARCH and biosignal amplifier (NeurOne/TurboLink) can be done now. In Matlab, type speedgoat.configureEthernet and a new window should pop up. If the window have not popped up, carefully repeat all the above mentioned steps. Select your device from the dropdown menu: its features should appear in the window below. On the right-hand side, type the following:

.. code-block::

   IP Address:          192.168.200.255
   Subnet mask:         255.255.255.0
   Default gateway:     192.168.7.5 


You can then click on Load configuration. If the green light appears, the communication has been successfully established.

Setup bossdevice API
====================
Your bossdevice RESEARCH is now ready to work! In MatLab, go into the bossdevice API folder or simply add the path to the code. Type b = bossdevice; in Matlab and check that the firmware is being loaded (i.e., that no error appears and the status on the screen attached to the bossdevice is updated). You can now run the demo script or your experimental paradigm.

Please note: if you encounter an out-of-date error when trying to load the firmware it could be a problem of Matlab not being able to read the firmware properly. In this case, contact the sync2brain team for support.

Connecting with Biosignal Amplifier (EEG Device)
================================================
The input of the bossdevice i.e. the EEG data is provided by connecting the Biosignal Amplifier (EEG device e.g. Bittium's NeurOne or Brain Product's actiCHamp) with the bossdevice using a standard LAN Cable. Search the “REAL TIME OUT” LAN port at the back plate of your EEG device and connect it to the “Biosignal” LAN port available at the bossdevice research. More details on this topic are available in the chapter 4 'Setup Biosignal Amplifier'. You can also see the "Biosignal" port on the bossdevice research in the figure shown below.

.. figure:: figures/Figure2_biosignallanport.png
    :align: center

    Ports available on the bossdevice (see the Biosignal LAN port).

Connecting with Synchronizing Devices
=====================================
The bossdevice RESEARCH can send up to 4 TTL output signals to connected synchronizing devices from its output port. Search the TTL OUT 1-4” BNC ports at back plate of the bossdevice RESEARCH and connect it to the BNC ports of your intended device.

.. figure:: figures/Figure6_ttlports.png
    :align: center

    Ports available on the bossdevice (see the BNC output ports 1-4 named as TTL OUT 1 till 4).

In addition to the TTL output the 8 bit LPT marker ports can also be connected to the bossdevice RESEARCH for a numerical input or can also be output from bossdevice RESEARCH to write synchronizing markers to the raw EEG data or for any other purpose.

.. figure:: figures/Figure7_markersport.png
    :align: center

    Ports available on the bossdevice (see the Marker 1 and Marker 2 8 bit LPT ports).


Connecting with Monitor
====================================
Monitor setup is optional and not recommended for normal use cases. It shows the useful information and analytics of the bossdevice RESEARCH time of execution and errors (if any) at the ocassion of debugging and setting up the device. The Display Port from backplate of the bossdevice RESEARCH may be connected to the monitor.

.. figure:: figures/Figure2_biosignallanport.png
    :align: center

    Ports available on the bossdevice (see the Display port (DP) for connection to monitor).

Power Supply
====================================
The power supply provided along with the bossdevice research can be plugged into the “DC IN 12V” port at the back plate of the bossdevice RESEARCH as shown in figure below.

.. figure:: figures/Figure2_biosignallanport.png
    :align: center

    Ports available on the bossdevice RESEARCH (see the 12V DC power supply connection port).


Switching ON and OFF
====================================
The bossdevice can be switched on or off using the push button at on of the sides of bossdevice. When switched off, the LED present on the button is not illuminated while when it is switched on, a blue LED is illuminated.


.. toctree::


