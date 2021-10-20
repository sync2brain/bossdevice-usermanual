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

    bossdevice RESEARCH accepts a 1kHz sampling rate of the biosignal, therefore the sampling rate from the real time UDP out of NeurOne must be 1kHz.

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

    bossdevice RESEARCH accepts a 1kHz sampling rate of the biosignal, therefore the sampling rate from the real time UDP out must be 1kHz.

.. important::

    bossdevice RESEARCH can take maximum of 128 EEG channels only, and 8 Aux/Bipoloar/EMG/ECG channels.

Please follow the instructions provided with turbolink from brain products in order to set it up correctly to be able to use it with bossdevice RESEARCH.


.. toctree::

