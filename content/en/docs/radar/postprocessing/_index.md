---
title: Postprocessing
description: Working with data recorded using ORCA
weight: 100
---

ORCA provides pre-processing and post-processing code that supports operation of SDRs as chirped radar systems. These functions are meant to provide basic support and analysis for ice-penetrating radar data. More advanced processing can be built off this code. While we built this code to process data collected with radars running ORCA, we hope that the documentation makes it flexible enough to process other forms of ice-penetrating radar data as well.

### Data Format

### Pre-processing Functions
Pre-processing functions available in ORCA are centered around waveform (chirp) generation. They are found in the `preprocessing/` folder of the main repo.
* `preprocessing/generate_chirp.py` Generates digital IQ (in-phase and quadrature) samples based on configuration parameters provided in a YAML configuration file
    - Support for the following window functions is currently provided: Rectangular, Hamming, Blackman-Harris, Kaiser10, Kaiser14, and Kaiser18
    - We have found [this document by G. Heinzel and others](https://holometer.fnal.gov/GH_FFT.pdf) to be a very useful resource for understanding and implementing window functions

### Post-processing Functions
A variety of post-processing functions are available in ORCA, ranging from basic radar processing, to radar system coherence characterization. 
* `processing.py`
* `processing_dask.py`
* `save_data.py`
* `merge_data.py`

### Post-processing Notebooks
We provide several starter notebooks to enable radar data processing and system characterization. 
* `notebooks/Field Processing.ipynb` A basic one stop radar processing code notebook, intended to produce ample information for in-field (or lab) debugging
    - Loads a binary file containing IQ radar data and resaves it into [Zarr](https://zarr.readthedocs.io) format
    - Loads the metadata associated with that binary file (YAML configuration file and log output)
    - Generates a reference chirp (by calling `preprocessing/generate_chirp.py`) to use for pulse compression
    - Displays a single received pulse in the time domain 
    - Removes errors (if necessary) from the collected radar data
    - Coherently averages (stacks) the data based on user-input stacking parameter
    - Pulse compresses the data
    - Displays a 1D radargram of the first and last pulse compressed pulse
    - Displays a 2D radargram of the entire dataset
    - Displays spectrogram (fast-time) of received data
    - Displays power spectrum of received data
* `notebooks/Radar 1D File Compare.ipynb` A notebook to compare 1D pulse compressed radar data in different files
    - Facilitates comparison between different radar configurations
* `notebooks/Radar 1D Stacking.ipynb` A notebook to display a 1D radargram with different amounts of stacking
    - Facilitates comparison of stacking benefits and a quick check on coherence of data
* `notebooks/Radar Spectrogram.ipynb` A notebook to display a fast time and slow time spectrogram of radar data
    - Facilitates debugging of RFI, frequency spurs, and other noise sources
* `notebooks/Dask Noise Statistics.ipynb` A notebook to compute and display mean noise power and variance for radar data
    - Facilitates analysis of coherence by computing the mean noise power and variance for different amounts of radar stacking

The `notebooks/orca_paper` subdirectory contains notebooks used to produce the figures in our IEEE paper:

{{% readfile "/docs/tgrs_citation.md" %}}