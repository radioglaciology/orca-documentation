---
title: Configuration options
description: All of the options in `config.yaml`
weight: 30
---

The entire configuration that controls the radar is defined in a single configuration file. This file is provided at runtime to `run.py` and encapsulates all of the settings needed to run various experiments on different hardware setups. Configuration files are specified in the YAML format and contained in the `config/` folder of the ORCA repository. Default configuration files are included to run the code on a B205mini-i (`default_b205.yaml1`) and on an X310 (`default_x310.yaml`). Here we explain the basics behind the settings available to the user via the config file. 

### Chirp and Pulse Parameters
* `sample_rate`: Sample rate of the generated chirp (used as TX and RX rate too), specified in Hz
    - On the X310, the sample rate should be an even integer division of the main clock rate
* `chirp_type`: Chirp frequency progression type
    - Supported options: "linear", "hyperbolic"
* `chirp_bandwidth`: Bandwidth of the chirp, specified in Hz
    - Should be less than or equal to `sample_rate` to satisfy Nyquist
* `lo_offset_sw`: Center frequency of the chirp, relative to `RF0:freq` (the RF center frequency), specified in Hz
* `window`: Window function applied to the chirp
    - Supported options: "rectangular", "hamming", "blackman", "kaiser10", "kaiser14", "kaiser18"
    - Applied on transmit and receive by default
* `chirp_length`: Chirp length without zero padding, specified in seconds
* `pulse_length`: Total pulse length (chirp + symmetric zero padding), specified in seconds
    - set equal to `chirp_length` if no zero padding is desired
* `out_file`: Name of the output binary file containing pulse samples
* `show_plot`: Whether to display a time-domain plot of the generated chirp (True or False)

### Device Connection and Data Transfer Parameters
* `device_args`: USRP device arguments are used to identify specific SDRs (if multiple areconnected to the same computer), to configure model-specific parameters, and to set transport parameters of the link (USB, ethernet) between the SDR and the host computer. 
    - See the default config file or Ettus UHD examples for the SDR you wish to use to set appropriately
* `subdev`: Active SDR submodules
    - See the default config file or Ettus UHD examples for the SDR you wish to use to set appropriately
* `clk_ref`: SDR clock reference source
    - Supported options: "internal", "external", "gpsdo"
    - "external" requires an external clock reference connected to the SDR
    - "gpsdo" requires a GPSDO module to be installed on the SDR
* `clk_rate`: SDR main clock frequency, specified in Hz
    - only specific frequencies are allowable for each SDR, see Ettus documentation for more information
* `tx_channels`: list of TX channels to use (comma separated)
* `rx_channels`: list of RX channels to use (comma separated)
* `cpu_format`: CPU-side sample format
    - Supported options: "fc32", "sc16", "sc8"
* `otw_format`: On the wire format
    - any format supported

### GPIO Configuration Parameters
Many of the Ettus SDRs have GPIO pins which can be used for conveying automatic transmit/receive signals or other general signals to external devices. The parameters in this section are specific to how MAPPERR and PEREGRINE use the SDR GPIO and can be adapted for other use cases. 
* `gpio_bank`: Which GPIO bank to use
    - "FP0" is the front panel and default bank on the X310
* `pwr_amp_pin`: Which GPIO pin to use fo external power amplifier control
    - set to "-1" if not using
* `ref_out`: Turns the 10 MHz reference out signal on the X310 on (1) or off (0)
    - set to (-1) if the SDR does not support a 10 MHz reference out signal

### RF Frontend 0 Configuration Parameters
RF configuration parameters for a single-channel radar setup. 
* `rx_rate`: RX sample rate, specified in Hz
    - defaults to be equal to `sample_rate`
* `tx_rate`: TX sample rate, specified in Hz
    - defaults to be equal to `sample_rate`
* `freq`: Center frequency (LO/mixer frequency), specified in Hz
* `lo_offset`: hardware LO/mixer offset, specified in Hz
* `rx_gain`: RX gain, specified in dB
    - available gain range dependent on specific SDR
* `tx_gain`: TX gain, specified in dB
    - available gain range dependent on specific SDR
* `bw`: Configurable hardware filter bandwidth, specified in Hz
    - not supported on all SDRs
    - set to 0 if not supported
* `tx_ant`: Port to be used for TX
* `rx_ant`: Port to be used for RX
* `transmit`: Whether to transmit data samples or not
    - set to "true" (or leave blank) to transmit samples (normal operation)
    - set to "false" to completely disable transmit
* `tuning_args`: Set integer-N or fractional tuning arguments
    - only supported on some SDRs
    - leave as "" to do nothing

### RF Frontend 1 Configuration Parameters
RF configuration parameters for the second channel of a multi-channel radar setup. This is only supported on SDRs with more than 2 TX/RX ports. Parameters are the same as in RF Frontend 0 Configuration. 

### Pulse Timing Parameters
* `time_offset`: Offset time after set up before the first received sample, specified in seconds
* `tx_duration`: Transmission duration, specified in seconds
    - defaults as, and should be, equal to `pulse_length`
* `rx_duration`: Receive duration, specified in seconds
    - should be long enough to capture echoes from the expected most distant target
* `pulse_rep_int`: pulse repetition interval, specified in seconds
    - should be longer than or equal to `rx_duration`
* `error_recovery_time`: Additional time added (on top of `pulse_rep_int`) to the next scheduled set of commands after an error occurs.
    - Most errors are due to a command being enqueued too late, so providing some extra buffer time lets the host computer "catch up"
* `tx_lead`: Time between start of TX and RX, specified in seconds
    - can be used for psuedo-blanking 
* `num_pulses`: Number of chirps/pulses to transmit and receive
    - set to -1 to continuously transmit and record until stopped by CTRL-C
    - code will record `num_pulses` of error-free pulses before terminating
* `num_presums`: Number of received pulses to coherently average before writing to file
* `phase_dithering`: Whether to enable phase dithering or not

### During Recording File Location Parameters
* `chirp_loc`: Which chirp file to transmit
    - in most cases, should be the same as `out_file`
* `save_loc`: (Temporary) location to write received samples to
* `gps_loc`: (Temporary) location to save GPS data
    - only works if "gpsdo" is selected for `clk_ref`
* `max_chirps_per_file`: Maximum number of chirps (after presumming) to write to a single file
    - set to -1 to avoid breaking into multiple files

### File Save Locations for run.py
These settings are only used by `run.py`, they are not read by `main.cpp`. 
* `final_save_loc`: Save location for the big final file, set to `null` if you don't want to save a big file
    - if `max_chirps_per_file` -= -1 (i.e. all data will be written directly to a single file), then final_save_loc and save_partial_files will be ignored
* `save_partial_files`: Set to True if you want individual small files to be copied with the timestamp, set to False if you just want the big merged file to be copied with the timestamp
    - if `max_chirps_per_file` == -1 (i.e. all data will be written directly to a single file), then final_save_loc and save_partial_files will be ignored
* `save_gps`: Set to True if using GPS and wanting to save GPS location data, set to False otherwise