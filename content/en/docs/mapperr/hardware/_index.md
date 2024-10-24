---
title: MAPPERR Core Hardware
linkTitle: Core Hardware
#description: Details about the MAPPERR system
#menu: {main: {weight: 20, pre: "<i class='fa-solid fa-map'></i>"}}
weight: 20
---


### Minimum Required Hardware
The core components of MAPPERR are an [Ettus X310](https://www.ettus.com/all-products/x310-kit/) software-defined radio (SDR), an external host computer, and two antennas (one for transmitting and one for receiving). We use [these](https://www.ahsystems.com/catalog/SAS-512-2.php) log-periodic dipole array antennas, as well as homemade resistively loaded dipoles, depending on our desired center frequency. For our host computer, we typically use a laptop running Ubuntu. 

MAPPERR can be operated in a basic configuration by connecting the antennas as shown in the diagram below and utilizing the [`default_x310.yaml`](/docs/radar/sdr-interface/config/) configuration file with ORCA. 

* ADD DIAGRAM

### Interfacing with the X310
The Ettus X310 can connect to a host computer via either 1 Gb or 10 Gb ethernet. When connected via 1 Gb ethernet, the maximum radar sample rate that does not produce significant communications errors between the host computer and SDR is 25 MHz. When using 10 Gb ethernet, sample rates of up to 100 MHz are possible. To achieve the maximum possible sample rate (200 MHz), dual 10 Gb ethernet connection is needed. 

Communicating with the SDR over ethernet requires network settings on the host computer to be set appropriately. See this guide and this guide from Ettus for more information. A [startup script](https://github.com/radioglaciology/uhd_radar/blob/main/x310_startup.sh) is available in ORCA that can be modified be each user. You can check that you are connected correctly to the X310 by pinging its IP address (typically 192.168.10.2 for a 1 Gb ethernet connection and 192.168.40.2 for a 10 Gb connection), or running `uhd_find_devices` and `uhd_usrp_probe` from a terminal. 

### UHD Version
Currently, ORCA has only been tested with an X310/MAPPERR when running and using UHD 3.15. In principle, upgrades to future UHD versions should be possible, however, we have previously run into [some issues](https://lists.ettus.com/empathy/thread/LO46KABJ6H3KQLITF65JGQPHE6QB3JAO) when upgrading. 