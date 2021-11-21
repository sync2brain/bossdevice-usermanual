.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Matlab API Documentation
============================================

.. toctree::

   4_api_documentation

The bossdevice API is a set of low-level functions that allows user to control the bossdevice in a user-friendly programmable manner. The combined execution of such low level functions allows the bossdevice to perform certain high level tasks that could be amongst but not limited to generating various number and combination of TTL outputs, timed in a certain fashion, TTL output at a certain Phase of an ongoing EEG Oscillation and guided by the Oscillation Amplitude Thresholds.

.. important::

    Please downloaded Matlab 2021a, Simulink Real-Time and Simulink Coder, Simulink and MATLAB Coder Adds-On from Matlab toolboxes, download the repository from master branch as detailed on "Downloads & Dependencies" chapter of the user manual. Add the bossdevice firmware and repository to the search path of Matlab 2021a. Observe from command line that if bossdevice firmware has been loaded from intended path, otherwise many copies of API are available on your device and Matlab 2021a is confused about choosing one therefore it is recommended to change the current directory of Matlab 2021a to the correct path of bossdevice latest API.


Initialization
===============================
Description
-----------
The bossdevice API can be initialized as a MATLAB Class object and provides access to its methods and functions to generate various TTL outputs and define multiple brain states upon which the predefined TTL output sequence to be released. 

Syntax
------

.. code-block:: matlab

   [MATLAB_class_object] = bossdevice;

Example
-------

.. code-block:: matlab

   bd=bossdevice; % "bd" class object is created in MATLAB workspace

-----

sendPulse
===============================

Description
-----------
sendPulse method of the bossdevice class allows you to generate 1 TTL pusle at a specified Output port (out of 4) of the bossdevice.

Syntax
------
.. code-block:: matlab

   obj.sendPulse([bossdevice_port_number]) % [bossdevice_port_number] is non-zero positive integer less than or equal to 4

Example
-------
.. code-block:: matlab

    bd.sendPulse(1) % delivers a single pulse at the Output port 1  
    bd.sendPulse(3) % delivers a single pulse at the Output port 3

----

manualPulse
===============================
Description
-----------
manualPulse method of bossdevice allows you to deliver a pre-built sequence of up to 1500 TTL outputs where each TTL output is timed relative to the onset of manualPulse command, delivers on a specified Output port and also delivers an 8-bit marker assigned to each TTL output as well.
The pre-built sequence is generated using another command (see below: configure_time_port_marker) however the bossdevice starts delivering the pulses configured before when manualPulse method is executed.

Syntax
------
.. code-block:: matlab

    obj.manualPulse % start the previously configured TTL pulse sequences in [time port marker] style.

Example
-------
.. code-block:: matlab

    bd.manualPulse

----

configure_time_port_marker
===============================
Description
-----------
configure_time_port_marker method of bossdevice allows you to prepare a sequence of TTL output in [time port marker] vector style. Maximum of 1500 TTL outputs can be loaded onto this configuration method. In order to prepare a single TTL output in [time port marker] style, just give input argument as [0 1 1] vector.
Each TTL output in this vector has three elements , first index being time, second index being the port number of bossdevice to deliver the output pulse and the last index being the 8-bit event marker to be generated and written to the Biosignal processor for respective TTL output.

Syntax
------
.. code-block:: matlab

    obj.configure_time_port_marker([ttloutput_sequence_vector]
    % [ttloutput_sequence vector] is an Nx3 vector , whereas N is number of TTL outputs and the three elements being the time onset, port number and event marker

Example
-------
.. code-block:: matlab

    bd.configure_time_port_marker([2 3 144]) %configures a TTL output sequence in which the pulse will be delivered 2 seconds after the manualPulse command execution, on the 3rd port and event writes 144 marker on the Biosignal processor
    bd.configure_time_port_marker([3 3 144;4 2 128])  %configures 2 TTL output sequences in which the first pulse will be delivered 3 seconds after the manualPulse command execution, on the 3rd port and will writes 144 marker on the Biosignal processor, and then the second pulse will be delivered 4 seconds after the manualPulse command execution, on the 2nd port and writes 128 event marker on the Biosignal processor,

----

stop
===============================
Description
-----------
Quite often stopping the TTL output sequence is required before the already conigured and long sequence ends. The stop method allows you to stop further generation and delivery of any sequence loaded onto the TTL output generator of the bossdevice.
The last configured sequence can again be started by using “manualPulse” method after the stop command.

Syntax
------
.. code-block:: matlab

    obj.stop % stops the generation of output TTL pulses from bossdevice however the class object remain intact.


Example
-------
.. code-block:: matlab

    bd.stop

----

eeg_channels
===============================
Description
-----------
 bossdevice can work on maximum of 128 EEG channels, however the minimum number of channels required to work with EEG associated features of bossdevice have been kept flexible and can be defined as in number of EEG cannnels being streamed from your Biosignal Processor.

Syntax
------
.. code-block:: matlab

    obj.eeg_channels=[number_of_streaming_eeg_channels] % [number_of_streaming_eeg_channels] is an integer from 1 up till 128.


Example
-------
.. code-block:: matlab

    bd.eeg_channels=32; % informing bossbox that 32 EEG channels are being streamed frm your Biosignal Processor
    bd.eeg_channels=64; % informing bossbox that 64 EEG channels are being streamed frm your Biosignal Processor
    bd.eeg_channels=128; % informing bossbox that 128 EEG channels are being streamed frm your Biosignal Processor
    bd.eeg_channels=5; % informing bossbox that 5 EEG channels are being streamed frm your Biosignal Processor

----

aux_channels
===============================
Description
-----------
 bossdevice can work on maximum of 8 Auxiliary/Biopolar/EMG channels, however the minimum number of channels required to work with Auxiliary Channels associated features of bossdevice have been kept flexible and can be defined as in number of Aux cannnels being streamed from your Biosignal Processor.

.. important::
    Auxiliary channels should always be the last 8 or N channels in your Protocol of Biosignal Processor otherwise bossdevice will assume them to be EEG channels

Syntax
------
.. code-block:: matlab

    obj.aux_channels=[number_of_streaming_aux_channels] % [number_of_streaming_eeg_channels] is an integer from 1 up till 8.


Example
-------
.. code-block:: matlab

    bd.eeg_channels=8; % informing bossbox that 8 aux/Emg/Bipolar channels are being streamed frm your Biosignal Processor
    bd.eeg_channels=2; % informing bossbox that 2 aux/EMG/Bipolar channels are being

----

aux_channels
===============================
Description
-----------
 Spatial filtering of the signals is an important step before commencing real-time brain states detection. The bossdevice allows to specify two different spatially filtered channels and can detect brain states (based on the target phase & amplitude) for both of these spatial filtered channels in parallel. If any one of them is specified then the other one is ignored and assigned 0 weights. The index of eeg_channels being streamed from the Biosignal processor e.g. ActiCHamp or NeurOne corrosponds to the index of the weights matrix explained below.

Syntax
------
.. code-block:: matlab

    obj.spatial_filter_weights=[weights_for_both_channels]
    %[weights_for_both_channels] is a Nx2 matrix where N is Number of EEG channels specified before (see eeg_channels) and the first column is first spatially filtered channel, similarly the second column is the second channel. Each element of the column vector is a "normalized weight" w.r.t. particular column such that the sum of all weights of a particular channel is zero.


Example
-------
.. code-block:: matlab

    % assume that there are 5 EEG channels 
    bd.spatial_filter_weights=[0; -0.5; 1; -0.5; 0]; % the channel 1 will be assigned the given weights and all the eeg_channels for 2nd spatially filtered channel would have 0 weights. 
    % assume that there are 3 EEG channels 
    bd.spatial_filter_weights=[0 -0.5; 1 1; 0 -0.5]; % the channel 1 will be assigned the weights given on first column and channel 2 will be assigned the weights given on 2nd column.  

----

phase_target
===============================
Description
-----------
 bossdevice contains 3 built in Oscillatory phase prediction models each for Theta (4-8 Hz), Alpha (8-14Hz) and Beta (14-30Hz) frequency bands. Real-time phase detection can be performed for maximum of 2 different, pre-specified spatially filtered channels in parallel. This method allows to define a target phase in radians for a particular frequency band.

Syntax
------
.. code-block:: matlab

    obj.alpha.phase_target([spatial_filter_channel_number])=phase_angle_in_radians % sets target phase of Alpha Oscillation
    obj.theta.phase_target([spatial_filter_channel_number])=phase_angle_in_radians % sets target phase of Theta Oscillation
    obj.beta.phase_target([spatial_filter_channel_number])=phase_angle_in_radians % sets target phase of Beta Oscillation
    % [spatial_filter_channel_number] is an integer having value 1 or 2.
    % [phase_angle_in_radians] is 0 for Peak, pi for Trough, -pi/2 for Rising Flank & +pi/2 for Falling Flank

.. figure:: figures/Figure10_phases.png
    :align: center

    Radian values of several target phases in bossdevice RESEARCH

Example
-------
.. code-block:: matlab

    bd.alpha.phase_target(1)=0; % bossdevice is loaded to detect peak (0 radians) from first spatially filtered channel  with the assumption that the Oscillation is in Alpha frequency band
    bd.beta.phase_target(2)=pi; % bossdevice is loaded to detect trough(pi radians) from second spatially filtered channel with the assumption that the Oscillation is in Beta frequency band

----

phase_plusminus
===============================
Description
-----------
 Defining absolute target phase angles in order to detect a brain state is often prone to error mainly due to the resolution of data obtained after sampling rate transition. In order to overcome this digitization resolution error another parameter has to be defined such that the vicinities of the target phase shall be made clear to the detection algorithm. For an instance, while detecting a 0 radians phase, the phase vector would probably look like this [-0.001324 -0.00234 0.00243 0.004324], and since none of them are mathematically equivalent to zero therefore in order to not allow to skip such Oscillatory Peak events and to increase the accuracy of the phase detection, a tolerance value is to be provided.

Syntax
------
.. code-block:: matlab

    obj.alpha.phase_plusminus([spatial_filter_channel_number])=phase_tolerance_in_radians % sets target phase tolerance of Alpha Oscillation
    obj.theta.phase_plusminus([spatial_filter_channel_number])=phase_tolerance_in_radians % sets target phase tolerance of Theta Oscillation
    obj.beta.phase_plusminus([spatial_filter_channel_number])=phase_tolerance_in_radians % sets target phase of Beta Oscillation
    % [spatial_filter_channel_number] is an integer having value 1 or 2.
    %Note: While targeting a random phase, the tolerance could go as high as possible i.e. pi radians or Nxpi radians e.g. 2pi etc


Example
-------
.. code-block:: matlab

    bd.alpha.phase_plusminus(1)=pi/40; % bossdevice is loaded to detect the setted target phase with a tolerance of pi/40 radians from first spatially filtered channel with the assumption that the Oscillation is in Alpha frequency band
    bd.beta.phase_plusminus(2)=pi/70; % ossdevice is loaded to detect the setted target phase with a tolerance of pi/70 radians from second spatially filtered channel with the assumption that the Oscillation is in Beta frequency band

----

amplitude_min
===============================
Description
-----------
 Defining minimum amplitude threshold in order to match a specific brain state is important. The bossdevice allows to define minimum amplitude threshold in micro volts that must be reached along with other brain state associated conditions such as maximum amplitude, phase target, and phase tolerance in order to be able to declare the brain state as detected and generate TTL output sequence. The bossdevice contains 3 built in Oscillatory minimum amplitude estimation models each for Theta (4-8 Hz), Alpha (8-14Hz) and Beta (14-30Hz) frequency bands. Real-time minimum amplitude detection can be performed for maximum of 2 different, pre-specified spatially filtered channels in parallel.

Syntax
------
.. code-block:: matlab

    obj.alpha.amplitude_min([spatial_filter_channel_number])=min_amplitude_microV % sets minimum amplitude threshold of Alpha Oscillation
    obj.theta.amplitude_min([spatial_filter_channel_number])=min_amplitude_microV % sets minimum amplitude threshold of Theta Oscillation
    obj.beta.amplitude_min([spatial_filter_channel_number])=min_amplitude_microV % sets minimum amplitude threshold of Beta Oscillation
    % [spatial_filter_channel_number] is an integer having value 1 or 2.

Example
-------
.. code-block:: matlab

    bd.alpha.amplitude_min(1)=1000; % bossdevice is loaded to monitor the specified minimum amplitude threshold from first spatially filtered channel with the assumption that the Oscillation is in Alpha frequency band
    bd.beta.amplitude_min(2)=1e5; % bossdevice is loaded to monitor the specified minimum amplitude threshold from second spatially filtered channel with the assumption that the Oscillation is in Beta frequency band

----

amplitude_max
===============================
Description
-----------
 Defining maximum amplitude threshold in order to match a specific brain state is important. The bossdevice allows to define maximum amplitude threshold in micro volts that must be not be reached in order to be able to declare the brain state as detected and generare TTL output sequence. The bossdevice contains 3 built in Oscillatory maximum amplitude estimation models each for Theta (4-8 Hz), Alpha (8-14Hz) and Beta (14-30Hz) frequency bands. Real-time maximum amplitude detection can be performed for maximum of 2 different, pre-specified spatially filtered channels in parallel.

Syntax
------
.. code-block:: matlab

    obj.alpha.amplitude_max([spatial_filter_channel_number])=max_amplitude_microV % sets maximum amplitude threshold of Alpha Oscillation
    obj.theta.amplitude_max([spatial_filter_channel_number])=max_amplitude_microV % sets maximum amplitude threshold of Theta Oscillation
    obj.beta.amplitude_max([spatial_filter_channel_number])=max_amplitude_microV % sets maximum amplitude threshold of Beta Oscillation
    % [spatial_filter_channel_number] is an integer having value 1 or 2.

Example
-------
.. code-block:: matlab

    bd.alpha.amplitude_max(1)=1000; % bossdevice is loaded to monitor the specified maximum amplitude threshold from first spatially filtered channel with the assumption that the Oscillation is in Alpha frequency band
    bd.beta.amplitude_max(2)=1e5; % bossdevice is loaded to monitor the specified maximum amplitude threshold from second spatially filtered channel with the assumption that the Oscillation is in Beta frequency band

----

lpf_fir_coeffs
===============================
Description
-----------
 Spatially filtered channel’s signals described in the “spatial_filter_weights” function are passed through a low pass FIR filter for anti-aliasing. The coefficients of this filter can be specified using this function provided that the filter has an order of 100 at maximum.

Syntax
------
.. code-block:: matlab

    obj.alpha.lpf_fir_coeffs = [filter_coefficients]; %for Alpha
    obj.theta.lpf_fir_coeffs = [filter_coefficients]; %for Theta
    obj.beta.lpf_fir_coeffs = [filter_coefficients]; %for Beta

Sampling Rates
--------------
.. code-block:: matlab

    Oscillation         Sampling Rate
    Alpha (8-14 Hz)         500 Hz
    Theta (4-8 Hz)          250 Hz
    Beta(14-30 Hz)          1000 Hz

Example
-------
.. code-block:: matlab

    bd.alpha.lpf_fir_coeffs =  [filter_coefficients];
    bd.beta.lpf_fir_coeffs =  [filter_coefficients];
----

bpf_fir_coeffs
===============================
Description
-----------
 Low pass filtered channel’s signal described in the “lpf_fir_coeffs” function is then pass through an FIR Band Pass filter. The coefficients of this filter can be specified using this function provided that the filter has an order of 100 at maximum.

Syntax
------
.. code-block:: matlab

    obj.alpha.bpf_fir_coeffs = [filter_coefficients]; %for Alpha
    obj.theta.bpf_fir_coeffs = [filter_coefficients]; %for Theta
    obj.beta.bpf_fir_coeffs = [filter_coefficients]; %for Beta

Sampling Rates
--------------
.. code-block:: matlab

    Oscillation         Sampling Rate
    Alpha (8-14 Hz)         500 Hz
    Theta (4-8 Hz)          250 Hz
    Beta(14-30 Hz)          1000 Hz

Example
-------
.. code-block:: matlab

    bd.alpha.bpf_fir_coeffs = firls(80, [0 (11 + [-5 -2 +2 +5]) (500/2)]/(500/2), [0 0 1 1 0 0], [1 1 1] ); %creates FIR Band Pass filter of order "80" , around peak frequency of 11 Hz and assigned it to Alpha model of bossdevice
    bd.beta.bpf_fir_coeffs = firls(100, [0 (19 + [-5 -2 +2 +5]) (1000/2)]/(1000/2), [0 0 1 1 0 0], [1 1 1] ); %creates FIR Band Pass filter of order "100" , around peak frequency of 19 Hz and assigned it to Beta model of bossdevice
----

arm
===============================
Description
-----------
Arming bossdevice would allow bossdevice to actively search for the Brain states already set into it and generate the TTL output sequence configured using “configure_time_port_marker” upon every instance of detection.

Syntax
------
.. code-block:: matlab

    obj.armed;


Example
-------
.. code-block:: matlab

    bd.armed; % will set bossdevice to detect the specified brain states and TTL output sequence 
----

disarm
===============================
Description
-----------
Disarming bossdevice would allow bossdevice to stop search for the Brain states already by now “Armed’ into it and immediately stops the generation of the planned TTL output sequence configured using “configure_time_port_marker” .

Syntax
------
.. code-block:: matlab

    obj.disarm;


Example
-------
.. code-block:: matlab

    bd.disarm; % will stop bossdevice to detect the specified brain states and stop ongoing TTL output sequence 
----

sample_and_hold_period
===============================
Description
-----------
Upon an artifactual event such as pulse artifact etc., the data gets distorted and is accumulated with a lot of noise, in order to hold the samples for a specified period of time, sample and hold period method can be helpful. 

Syntax
------
.. code-block:: matlab

    obj.sample_and_hold_period = [no_of_sample_to_hold]; % no_of_sample_to_hold are calculated at the rate of 5KHz 


Example
-------
.. code-block:: matlab

    bd.sample_and_hold_period = 500; % hold the samples for 100 ms  
----

min_inter_pulse_interval
===============================
Description
-----------
Minimum inter pulse interval allows you to define the time period for which the bossdevice should wait before generating another TTL output.

Syntax
------
.. code-block:: matlab

    obj.min_inter_pulse_interval = [time_in_secodns]; % Seconds to wait before generating another TTL output

Example
-------
.. code-block:: matlab

    obj.min_inter_pulse_interval = 2; % bossdevivce will wait for 2 seconds before generating any TTL output 
----

mep
===============================
Description
-----------
Get the auxillary channel data time locked to the trigger pulse for a given channel index, pre trigger period and post trigger period. 

Syntax
------
.. code-block:: matlab

    MEP_Data=obj.mep(Channel_Index,PreTriggerPeriodInMS,PostTriggerPeriodInMS;

Example
-------
.. code-block:: matlab

    mep_data=bd.mep(1,100,100); %bossdevice will output the 1st auxillary channel's data 100ms pre trigger time point and 100ms post trigger time point.
----

.. toctree::