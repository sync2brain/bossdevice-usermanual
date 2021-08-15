.. User Manual - bossdevice research documentation master file, created by
   sphinx-quickstart on Fri Jul  9 21:52:50 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.



============================================
Demo Scripts
============================================

.. toctree::

   5_demo_scripts


Open Loop Jittered TTL Output
===============================
This demo script uses bossdevice research and 2 different approaches to generate jittered open loop TTL output.

Resources: 1) bossdevice Switched On, 2) bossdevice Open Source MATLAB API

.. code-block:: matlab

   NumberOfTrials=10; 
    ITI=[4 6]; %ITI is seconds - a random number between these two values
    %% Initializing bossdevice research API 
    bd=bossdevice;
    %% Approach 1 - For Loop Based Open Loop TTL output generation
    bd.configure_time_port_marker([0 1 1]); %Configuring TTL output Sequence in [Time Port Marker] format  
    for TrialNumber=1:NumberOfTrials
        bd.manualPulse
        disp(['Generated Trial #' num2str(TrialNumber)])
        min_inter_pulse_interval= ITI(1)+ (ITI(2)-ITI(1)).*rand(1,1); %Assigning New Random ITI for this Trial to the bossdevice research
        pause(min_inter_pulse_interval) %Wait for next trial start
    end
    disp('Trials Completed')
    %% Approach 2 - bossdevice research Sequence Generator Based Open Loop TTL output
    time_port_marker_sequence=[];
    Time=0;
    time_port_marker_sequence(NumberOfTrials,3)=0; %Pre filling the array 
    for TrialNumber=1:NumberOfTrials
        Time=Time+ITI(1)+ (ITI(2)-ITI(1)).*rand(1,1); %Generating Sequence of Jittered ITIs for all Trials 
        Port=1; %In order to generatre TTL output always on first port , use 2 for 2nd port and 3 for third port
        Marker=TrialNumber; 
        time_port_marker_sequence(TrialNumber,:)=[Time Port Marker];
    end
    bd.configure_time_port_marker(time_port_marker_sequence); %Assigning Pregenerated sequence to the bossdevice research 
    bd.manualPulse % Generating the TTL output of the bossdevice research to start sequence TTL Output Generation
    disp('TTL output Sequence Started by bossdevice research')

-----

mu-Rhythm Phase Locked TTL Output
=================================
This demo script uses bossdevice to deliver mu Rising or Falling Flank Phase locked TTL outputs
Resources: 1) bossdevice Switched On
2) bossdevice Open Source MATLAB API
3) Biosignal Amplifier streaming atleast 5 EEG Channels

.. code-block:: matlab

   no_of_trials=10;
    minimium_inter_pulse_interval=5; %s
    phase=[+pi/2 -pi/2]; %[falling_flank rising_flank]
    phase_tolerance=pi/40; 
    individual_peak_frequency=11; % Hz
    bandpassfilter_order= 75;
    eeg_channels=5; %Assigning Number of channels as equivalent to Num of Channels streamed by Biosignal Processor
    spatial_filter_weights=[1 -0.25 -0.25 -0.25 -0.25]'; %Column Vector of Spatial Filter Indexed wrt corrosponding Channels

    time=0;
    plasticity_protocol_sequence=[];

    %% Initializing bossdevice research API
    bd=bossdevice;
    bd.sample_and_hold_period=0;
    bd.calibration_mode = 'no';
    bd.armed = 'no';
    bd.sample_and_hold_period=0;
    bd.theta.ignore; pause(0.1)
    bd.beta.ignore; pause(0.1)
    bd.alpha.ignore; pause(0.1)
    bd.eeg_channels=eeg_channels; 

    %% Preparing an Individual Peak Frequency based Band Pass Filter for mu Alpha
    bpf_fir_coeffs = firls(bandpassfilter_order, [0 (individual_peak_frequency + [-5 -2 +2 +5]) (500/2)]/(500/2), [0 0 1 1 0 0], [1 1 1] );

    %% Setting Filters on bossdevice research
    bd.spatial_filter_weights=spatial_filter_weights;
    bd.alpha.bpf_fir_coeffs = bpf_fir_coeffs;

    %% Controlling bossdevice research for mu Alpha Phase Locked TTL Output
    condition_index=0;
    while (condition_index <= no_of_trials)
        if(strcmp(bb.armed, 'no'))
            bb.pulses_remaining = 1;
            bb.alpha.phase_target(1) = phase(randi(1:numel(phase), 1));
            bb.alpha.phase_plusminus(1) = phase_tolerance;
            bb.configure_time_port_marker(([0, 1, 0]))
            bb.min_inter_pulse_interval = minimium_inter_pulse_interval;
            pause(0.1)
            bb.arm;
        end
        % trigger has been executed, move to the next condition
        if(bb.pulses_remaining == 0)
            condition_index = condition_index + 1;
            bb.disarm;
            disp (['Triggered around ' (num2str(rad2deg(bb.alpha.phase_target(1)))) ' degrees Phase angle.'])
            pause(minimium_inter_pulse_interval)
        end
        pause(0.01);
    end

    %% End
    disp ('Protocol has been completed');

-----

Phase Locked Plasticity Protocol
===================================
This demo script uses bossdevice research and 2 different approaches to generate jittered open loopTTL output

Resources: 1) bossdevice Switched On, 2) bossdevice Open Source MATLAB API

.. code-block:: matlab

   no_of_trials=25;
    no_of_pulses=100; 
    pulse_frequency=100; %Hz
    minimium_inter_pulse_interval=5; %s
    phase=0; %peak
    phase_tolerance=pi/40; 
    individual_peak_frequency=11; % Hz
    bandpassfilter_order= 75;
    eeg_channels=5; %Assigning Number of channels as equivalent to Num of Channels streamed by Biosignal Processor
    spatial_filter_weights=[1 -0.25 -0.25 -0.25 -0.25]'; %Column Vector of Spatial Filter Indexed wrt corrosponding Channels

    time=0;
    plasticity_protocol_sequence=[];

    %% Initializing bossdevice research API
    bd=bossdevice;
    bd.sample_and_hold_period=0;
    bd.calibration_mode = 'no';
    bd.armed = 'no';
    bd.sample_and_hold_period=0;
    bd.theta.ignore; pause(0.1)
    bd.beta.ignore; pause(0.1)
    bd.alpha.ignore; pause(0.1)
    bd.eeg_channels=eeg_channels; 

    %% Preparing a Plasticity Protocol Seqeuence for bossdevice research
    plasticity_protocol_sequence(no_of_pulses,3)=0; %Pre filling the array 
    for iPulse=1:no_of_pulses
        time=time+0.01;
        port=1;
        marker=iPulse;
        plasticity_protocol_sequence(iPulse,:)=[time port marker];
    end

    %% Preparing an Individual Peak Frequency based Band Pass Filter for mu Alpha
    bpf_fir_coeffs = firls(bandpassfilter_order, [0 (individual_peak_frequency + [-5 -2 +2 +5]) (500/2)]/(500/2), [0 0 1 1 0 0], [1 1 1] );

    %% Setting Filters on bossdevice research
    bd.spatial_filter_weights=spatial_filter_weights;
    bd.alpha.bpf_fir_coeffs = bpf_fir_coeffs;



    %% For plasticitz, we have the same condition, multiple times, we can run everything on the device:
            bd.pulses_remaining = 100;
            bd.alpha.phase_target(1) = phase;
            bd.alpha.phase_plusminus(1) = phase_tolerance;
            bd.configure_time_port_marker(plasticity_protocol_sequence)
            bd.min_inter_pulse_interval = minimium_inter_pulse_interval;
            pause(0.1)
            bd.arm;

            fprintf('\nSystem running, pulses remaining: %03i', bd.pulses_remaining)
            while (bd.pulses_remaining > 0)
                fprintf('\b\b\b%03i', bd.pulses_remaining);
                pause(0.1)
            end
            fprintf('\b\b\bDone\n')


    %% Controlling bossdevice research for mu Alpha Phase Locked TTL Ouput % this could be for excitability, where we have interleaved different conditions
    condition_index=0;
    while (condition_index <= no_of_trials)
        if(strcmp(bd.armed, 'no'))
            bd.pulses_remaining = 1;
            bd.alpha.phase_target(1) = phase;
            bd.alpha.phase_plusminus(1) = phase_tolerance;
            bd.configure_time_port_marker(plasticity_protocol_sequence)
            bd.min_inter_pulse_interval = minimium_inter_pulse_interval;
            pause(0.1)
            bd.arm;
        end
        % TTL output has been generated, move to the next condition
        if(bd.pulses_remaining == 0)
            condition_index = condition_index + 1;
            bd.disarm;
            disp TTL_Output_Generated!
            pause(minimium_inter_pulse_interval)
        end
        pause(0.01);
    end

    %% End
    disp ('Plasticity Protocol has been completed');

-----

Real-Time Oscillation Amplitude Threshold Tracking
==================================================
This demo script uses bossdevice research and 2 different approaches to generate jittered open loop TTL output

Resources: 1) bossdevice Switched On, 2) bossdevice Open Source MATLAB API

.. code-block:: matlab

    no_of_trials=25;
    minimium_inter_pulse_interval=4; %s
    phase=0; %[positive]
    phase_tolerance=pi/40;
    amplitude_threshold=[25 75]; %[min max] in percentile
    amplitude_assignment_period=60*2; %s
    individual_peak_frequency=11; % Hz
    bandpassfilter_order= 75;
    eeg_channels=5; %Assigning Number of channels as equivalent to Num of Channels streamed by Biosignal Processor
    spatial_filter_weights=[1 -0.25 -0.25 -0.25 -0.25]'; %Column Vector of Spatial Filter Indexed wrt corrosponding Channels
    time=0;
    plasticity_protocol_sequence=[];

    %% Initializing bossdevice research API
    bd=bossdevice;
    bd.sample_and_hold_period=0;
    bd.calibration_mode = 'no';
    bd.armed = 'no';
    bd.sample_and_hold_period=0;
    bd.theta.ignore; pause(0.1)
    bd.beta.ignore; pause(0.1)
    bd.alpha.ignore; pause(0.1)
    bd.eeg_channels=eeg_channels;

    %% Preparing an Individual Peak Frequency based Band Pass Filter for mu Alpha
    bpf_fir_coeffs = firls(bandpassfilter_order, [0 (individual_peak_frequency + [-5 -2 +2 +5]) (500/2)]/(500/2), [0 0 1 1 0 0], [1 1 1] );

    %% Setting Filters on bossdevice research
    bd.spatial_filter_weights=spatial_filter_weights;
    bd.alpha.bpf_fir_coeffs = bpf_fir_coeffs;

    %% Configuring Real-Time Scopes for Amplitude Tracking
    AMP_TRACING_SCOPES_IDS = [101 102];

    % remove any pre-existing scopes with these ids
    for id = AMP_TRACING_SCOPES_IDS
        if(find(bd.tg.Scopes == id))
            fprintf('\nRemoving scope %i', id)
            remscope(bd.tg, id);
        end
    end

    sig_id_amp = getsignalid(bd.tg, 'OSC/alpha/IA'); %amplitude
    sig_id_qly = getsignalid(bd.tg, 'QLY/Logical Operator2'); %eeg_is_clean

    sc = addscope(bd.tg, 'host', AMP_TRACING_SCOPES_IDS);
    addsignal(sc, [sig_id_amp sig_id_qly]);

    sc(1).NumSamples = 500;
    sc(1).Decimation = 10;
    sc(1).TriggerSample = -1;

    sc(2).NumSamples = 500;
    sc(2).Decimation = 10;
    sc(2).TriggerSample = -1;

    sc(1).TriggerMode = 'Scope';
    sc(1).TriggerScope = AMP_TRACING_SCOPES_IDS(2);

    sc(2).TriggerMode = 'Scope';
    sc(2).TriggerScope = AMP_TRACING_SCOPES_IDS(1);

    start(sc); % now they are ready for TTL output generation

    activeScope = 1;
    mAmplitudeScopeCircBufTotalBlocks = amplitude_assignment_period;
    mAmplitudeScopeCircBufCurrentBlock = 1;
    mAmplitudeScopeCircBuf = [];
    hAmplitudeHistoryAxes = subplot(1,2,1);
    hAmplitudeDistributionAxes = subplot(1,2,2);

    trigger(sc(activeScope));

    %% Controlling bossdevice research for mu Alpha Phase Locked TTL Output
    condition_index=0;
    while (condition_index <= no_of_trials)
        if (strcmp(sc(activeScope).Status, 'Finished') || ...
                strcmp(sc(activeScope).Status, 'Interrupted'))
            
            time = sc(activeScope).Time;
            data = sc(activeScope).Data;
            plot(hAmplitudeHistoryAxes, time, data(:,1));
            
            fprintf(['Restarting Scope ' num2str(activeScope)]);
            
            % Restart this scope.
            start(sc(activeScope));
            
            % Switch to the next scope.
            if(activeScope == 1)
                activeScope = 2;
            else
                activeScope = 1;
            end
            
            % append data in circular buffer
            mAmplitudeScopeCircBuf{mAmplitudeScopeCircBufCurrentBlock} = data';
            
            maxmindata = cell2mat(cellfun(@(data) quantile(data(1, data(2,:) == 1), [amplitude_threshold(1)/100 amplitude_threshold(2)/100])', mAmplitudeScopeCircBuf, 'UniformOutput', false))';
            maxmindata = circshift(maxmindata, mAmplitudeScopeCircBufCurrentBlock);
            plot(hAmplitudeHistoryAxes, maxmindata)
            xlim(hAmplitudeHistoryAxes, [1 mAmplitudeScopeCircBufTotalBlocks])
            set(hAmplitudeHistoryAxes, 'Xdir', 'reverse')
            
            circular_buffer_data = cell2mat(mAmplitudeScopeCircBuf);
            
            % Switch to the next data block
            if(mAmplitudeScopeCircBufCurrentBlock < mAmplitudeScopeCircBufTotalBlocks)
                mAmplitudeScopeCircBufCurrentBlock = mAmplitudeScopeCircBufCurrentBlock + 1;
            else
                mAmplitudeScopeCircBufCurrentBlock = 1;
            end
            
            %tic
            
            % remove post-TTL pulse data
            amplitude_clean = circular_buffer_data(1, circular_buffer_data(2,:) == 1);
            
            % calculate percentiles
            amplitude_sorted = sort(amplitude_clean);
            plot(hAmplitudeDistributionAxes, amplitude_sorted)
            
            amp_lower = quantile(amplitude_clean, amplitude_threshold(1)/100); % TODO: INCLUDE THIS IN INFO STRUCT
            amp_upper = quantile(amplitude_clean, amplitude_threshold(2)/100); % TODO: INCLUDE THIS IN INFO STRUCT
            
            hold(hAmplitudeDistributionAxes, 'on')
            plot(hAmplitudeDistributionAxes, [1 length(amplitude_clean)], [amp_lower amp_upper; amp_lower amp_upper]);
            hold(hAmplitudeDistributionAxes, 'off')
            
            if length(amplitude_clean) > 1
                xlim(hAmplitudeDistributionAxes, [1 length(amplitude_clean)]);
            end
            if (amplitude_sorted(end) > amplitude_sorted(1))
                ylim(hAmplitudeDistributionAxes, [amplitude_sorted(1) amplitude_sorted(end)]);
            end
            
            %toc
            
            % set amplitude threshold
            mDbsp.alpha.amplitude_min(1) = amp_lower;
            mDbsp.alpha.amplitude_max(1) = amp_upper;
            bd.alpha.amplitude_min(1)=amp_lower;
            bd.alpha.amplitude_max(1)=amp_upper;
            title(hAmplitudeDistributionAxes, ['Min Amplitude: ', num2str(amp_lower)]);
            
        end % handle the amplitude tracking
        if(strcmp(bd.armed, 'no'))
            bd.pulses_remaining = 1;
            bd.alpha.phase_target(1) = phase(randi(1:numel(phase), 1));
            bd.alpha.phase_plusminus(1) = phase_tolerance;
            bd.configure_time_port_marker(([0, 1, 0]))
            bd.min_inter_pulse_interval = minimium_inter_pulse_interval;
            pause(0.1)
            bd.arm;
        end
        % TTL output has been generated, move to the next condition
        if(bd.pulses_remaining == 0)
            condition_index = condition_index + 1;
            bd.disarm;
            disp TTL_Output_Has_Been_Generated!
        end
        pause(0.01);
    end

    %% End

---------

Phase Prediction Error Measurement
==================================================
This demo script uses bossdevice research and 2 different approaches to generate jittered open loop TTL output

Resources: 1) bossdevice Switched On, 2) bossdevice Open Source MATLAB API

.. code-block:: matlab

    ang_diff = @(x, y) angle(exp(1i*x)./exp(1i*y));
    ang_var = @(x) 1-abs(mean(exp(1i*x)));
    %ang_var2dev = @(v) sqrt(2*v); % circstat preferred formula uses angular deviation (bounded from 0 to sqrt(2)) which is sqrt(2*(1-r))
    ang_var2dev = @(v) sqrt(-2*log(1-v)); % formula for circular standard deviation is sqrt(-2*ln(r))

    %%  Initializing bossdevice research API
    bb = bossdevice;
    bd.eeg_channels = 1;
    bd.aux_channels = 1;
    bd.spatial_filter_weights = 1;

    bd.alpha.offset_samples = 5; %this depends on the loop-delay

    %% Setting Filters to bossdevice research
    % this allows calibrating the oscillation analysis to an individual peak frequency
    bd.alpha.bpf_fir_coeffs = firls(70, [0 6 9 13 16 (500/2)]/(500/2), [0 0 1 1 0 0], [1 1 1]);
    %fvtool(bd.alpha.bpf_fir_coeffs, 'Fs', 500) % visualize filter

    %% Configuring a scope to acquire data
    sc = addscope(bd.tg, 'host', 101);
    addsignal(sc, getsignalid(bd.tg, 'SPF/Matrix Multiply')); % this signals goes into the oscillation analysis
    addsignal(sc, getsignalid(bd.tg, 'OSC/alpha/IP')); % instantaneous phase estimate for alpha

    sc.NumSamples = 10 * 5000;
    sc.Decimation = 1;

    fprintf('\nAcquiring data ...'), start(sc);
    while(strcmp(sc.Status, 'Acquiring')), fprintf('.'), pause(1), end
    fprintf(' done')
                
    data = sc.Data(:,1);
    ip_estimate_causal = sc.Data(:,end);
    fs = 1/mean(diff(sc.Time));

    fprintf('\nDetermining phase using standard non-causal methods ...')
    % demean
    data = data - mean(data);
    % zero phase band-pass filter
    D = designfilt('bandpassfir', 'FilterOrder', round(1*fs), 'CutoffFrequency1', 9, 'CutoffFrequency2', 13, 'SampleRate', fs);
    data = filtfilt(D, data); %demean

    ip_estimate_noncausal = angle(hilbert(data));
    phase_error = ang_diff(ip_estimate_noncausal, ip_estimate_causal);

    fprintf('\n')

    %% Visualize           
    figure
    ax1 = subplot(2,2,1);
    plot(sc.Time-sc.Time(1), [sc.Data(:,1) data])
    ax2 = subplot(2,2,2);
    plot(sc.Time-sc.Time(1), [ip_estimate_causal ip_estimate_noncausal])
    linkaxes([ax1 ax2], 'x')
    subplot(2,2,4,polaraxes);
    polarhistogram(phase_error, 'Normalization', 'probability', 'BinWidth', pi/36);
    title(sprintf('circular standard deviation = %.1f°', rad2deg(ang_var2dev(ang_var(phase_error)))))
                
    % remove the scope
    remscope(bd.tg, 101)

-----


Loop Latency Measurement
==================================================
This demo script uses bossdevice research and 2 different approaches to generate jittered open loop TTL output

Resources: 1) bossdevice Switched On, 2) bossdevice Open Source MATLAB API

.. code-block:: matlab
    
    bd = bossdevice;

    %% Configuring Scope
    sc = addscope(bd.tg, 'host', 255);

    mrk_signal_id = getsignalid(bd.tg, 'UDP/raw_mrk') + int32([0 1 2]);

    addsignal(sc, mrk_signal_id);
    sc.NumSamples = 100;
    sc.NumPrePostSamples = -50;
    sc.Decimation = 1;
    sc.TriggerMode = 'Signal';
    sc.TriggerSignal = getsignalid(bd.tg, 'gen_running');
    sc.TriggerLevel = 0.5;
    sc.TriggerSlope = 'Rising';

    %% Generating TTL Output

    fprintf('\nTesting... ')
    start(sc);
    pause(0.1); % give the scope time to pre-aquire
    assert(strcmp(sc.Status, 'Ready for TTL Output generation'));

    s = [0, 1, 0];
    s(1000,3) = 0; % fill with zeros (TODO: this should be done in the API)
    bd.generator_sequence = s;

    bd.;

    pause(0.1)
    assert(strcmp(sc.Status, 'Finished'))

    fprintf('loop delay is %2.1f ms\n', (find(sc.Data(:,1), 1)-50)/5)

----

.. toctree::