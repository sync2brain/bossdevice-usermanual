.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Introduction to bossdevice RESEARCH
============================================


bossdevice RESEARCH is a real-time digital signal processor consisting of hardware and software algorithms. It is designed to read-in a real-time raw data stream from a biosignal amplifier (electroencephalography, EEG), to continuously analyze this data and to detect patterns based on oscillations in different frequencies. When such a specific bio-signal pattern is detected, the device indicates this through a standard output port. This enables a connected device to know with millisecond accuracy when a specific bio-signal pattern occurs. 

.. important::

    bossdevice RESEARCH is not a medical device. It may not be used outside of research
    and it may not be used in trials involving patients. It is not intended as an accessory
    to a medical device or to control a medical device. It may only be connected to a
    medical device through a connection that is not considered a “control” connection
    but only an information transmission. What the connected device does in response
    to information received from the bossdevice is determined by the connected device
    only.


.. figure:: figures/Fig1_bossdeviceandneurone.png
    :align: center

    The bossdevice RESEARCH placed along with Bittium NeurOne biosignal amplifier.

.. toctree::