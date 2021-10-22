.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Setup biosignal Amplifier
============================================

.. toctree::

   3_setup_biosignal_amplifier

Bittium NeurOne
===============================

.. important::

    bossdevice RESEARCH digital sampling rate is 5khz and it is assumed that a NeurOne protocol at 5Khz is being streamed and the sampling rate of real time UDP out of NeurOne is be 1kHz (i.e. 5 samples/packet).

.. important::

    In your NeurOne protocol, keep all the EEG channels on the top and other Aux/bipolar/EMG/ECG channels at the bottom.

.. important::

    If two 32 channels preamplifiers are used, do not use the Aux/bipolar inputs/EMG/ECG channels numberd from 33-40, but only the last 8 of the 2nd amplifier i.e. 73 to 80. Similar pattern applies to other combinition of preamplifiers. 

.. important::

    bossdevice RESEARCH can take maximum of 128 EEG channels only, and 8 Aux/Bipoloar/EMG/ECG channels. 

Software Version
-----------------------------------
The NeurOne software version should be v1.5.1 or higher.


.. figure:: figures/Figure8_N1.png
    :align: center

     The version number of your NeurOne software can be found on the top most Title of the NeurOne Window opened – NeurOne -v1.5.1

Firmware Version
-----------------------------------
The firmware version should be 1.4.30 or higher.


Real Time Out Channels
-----------------------------------
The Target UDP Port should be changed to 25000 from default of 50000, and the rest of details as shown in the figure below.


.. figure:: figures/Figure9_N1.png
    :align: center
	
     On the Real Time Channels tab of NeurOne protocol, go to the Settings and then set the Target UDP Port and Send Triggers as Channels, copy all the settings as shown in this figure to the respective NeuroOne protocol.

Brain Products actiCHamp (Turbolink)
===================================

.. important::

    bossdevice RESEARCH digital sampling rate is 5khz and it is assumed that a turbolink protocol at 5Khz is being streamed and the sampling rate of real time UDP out of turbolink is be 1kHz (i.e. 5 samples/packet).

.. important::

    bossdevice RESEARCH can take maximum of 128 EEG channels only, and 8 Aux/Bipoloar/EMG/ECG channels.

Step # 1 - Connect all the components 
-----------------------------------
-  TurboLink to the mains (via power cable)
-  actiCHamp (Plus) with battery to the TurboLink (via USB cable)
-  Recorder PC with plugged in BrainVision Recorder USB license dongle to the left Ethernet port of the TurboLink (via network cable)
-  Data Client to the right Ethernet port of the TurboLink (via network cable)
-  Plug the TurboLink USB license dongle in the TurboLink

.. important::

    BrainVision Recorder 1.23.0001 or higher must be installed on the Recorder PC.
	
.. figure:: figures/TurboLink_SetUp_IMG-01.png
    :align: center
     Summarized infographics showing connection of all the components
	
Step # 2 - Setup a fixed IP address for the TurboLink
--------------------------------------------
-  On the Recorder PC, press Windows+[R], enter ncpa.cpl in the Run Window and hit [Enter]
-  Right-click on “Ethernet”, click on “Properties” and set the fixed IP address to 192.168.5.1 as shown in the screenshot

.. figure:: figures/TurboLink_SetUp_IMG-02.png
    :align: center

     Screenshots while setting up IP address for the TurboLink

Step # 3 - Switch On the TurboLink
--------------------------------------------
Switch on the TurboLink using the Power Button at the front. After the second set of beeps (~20 seconds), it’s ready to be used.

Step # 4 (Optional) - Check and set TurboLink connection properties
---------------------------------------------------------------------
-  On the Recorder PC, open a browser and go to 192.168.5.21:8000
-  Check if all connections are established
-  Set a dedicated sampling rate for the data client output (one sample per packet). Default is 1000 Hz.
-  Use the Packet rate option for a lower transmission rate (multiple samples per packet) to the data client. Recorder sampling rate is not affected.

.. figure:: figures/TurboLink_SetUp_IMG-04.png
    :align: center

     Highlights showing how to TurboLink setup
	 
.. figure:: figures/TurboLink_SetUp_IMG-Prerequisites.png
    :align: center

Step # 5 - Start BrainVision Recorder on the Recorder PC (in Admin-mode)
--------------------------------------------------------------------------
-  Go to Configuration > Select Amplifier and select “actiCHamp-TurboLink”

.. important::

    Leave the default IP address that is set here unchanged (i.e. 192.168.5.21).

-  Create a new (or edit an existing) workspace and “Scan for amplifier”
-  Start monitoring or check impedances to ensure signal quality
-  Optional: Start recording

.. important::

    To send data to the data client, it’s sufficient to start monitoring.

Step # 6 - After Use
--------------------------------------------------------------------------
Make sure to shut down the TurboLink using the Power Button at the front. The TurboLink status LED turns red.


.. toctree::

