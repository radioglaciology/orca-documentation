---
title: Interfacing with SDRs
description: Documentation for ORCA code that directly interacts with the SDR
weight: 10
---

ORCA is designed to run on any non-embedded SDR within the Ettus family. It uses C++ and [Ettus' UHD](https://github.com/EttusResearch/uhd) API to interface with the SDRs. ORCA has been explicitly tested on the following Ettus instruments:
* B200 
* B205mini-i
* X310 with UBX-160 daughterboards

ORCA should run without problems on any of the following Ettus instrument families:
* USRP X Series
* USRP Bus Series

In principle, ORCA can be run on instruments in the USRP Networked Series and the USRP Embedded Series, however additional setup is required to support these platforms. 

### Code Architecture

To help achieve higher duty cycles, the code is divided into two threads: a scheduler thread responsible for enqueueing commands for the SDR, and a data-writing thread responsible for pulling received data from the SDR and storing it in persistent memory.

### Repository Organization
Code to interface with the SDRs is contained within the [`sdr/`](https://github.com/radioglaciology/uhd_radar/tree/main/sdr) folder of the ORCA repository. 
* `main.cpp` contains the bulk of the radar code, including the scheduling and data-writing threads
* `psuedorandom_phase.cpp` contains the seeded random number generator used to implement phase dithering
* `rf_settings.cpp` takes care of the RF-related frequency, gaining, and LO settings
* `utils.cpp` holds utility functions

### Running the Code
Everything about the experiment you want to run is defined by a configuration YAML file, explained more here. 

We recommend running everything (pre-processing chirp generation and radar code) by using the `run.py` utility. In general, you run it like this:

`python run.py config/your_config_file.yaml`

This utility handles creating the chirp that will be transmitted, compiling and running the C++ code that interacts with the SDR, and collecting the output data. All of the configuration for this command is contained within your configuration YAML file.



