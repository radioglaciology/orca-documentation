---
title: Host computer transport parameter tuning
description: Overview of host interface tuning to achieve maximum duty cycle
#weight: 10
---

The maximum duty cycle you can achieve is a function of the SDR you use, the
host computer, and the bandwidth you need. This page outlines suggestions for
how to figure out an achievable duty cycle and what parameters can be tweaked
to maximize this.

# General process for benchmarking duty cycle


## Determine the maximum achievable bandwidth with benchmark_rate

Use the USRP example benchmark_rate program (located at
`<path to conda install>/envs/<uhd environment name>/lib/uhd/examples`) to
determine the approximate maximum duty cycle you can achieve. For instance, with
a Pi 4 and b205mini, I can reach about a 25% duty cycle -- defined as sample
rate divided by desired sample rate (14 Msps / 56 Msps in this case) -- while
consistently getting 0 dropped packets.

```
./benchmark_rate --tx_rate 14e6 --rx_rate 14e6 --tx_otw sc12 --rx_otw sc12 --args="num_recv_frames=512,num_send_frames=512,recv_frame_size=16000,send_frame_size=16000"
```

This is also the stage to experiment with what transport parameters allow for
the highest throughput on your combination of computer/interface/SDR. However
the choice of (send/recv)_frame_size should be based on the number of samples
per chirp and won’t have the same kind of impact in this continuous streaming
case. In other words, you can get close but more tweaking later will help --
probably.

Read about
[USB transport parameters on the Ettus website](https://files.ettus.com/manual/page_transport.html).

## Figure out desired pulse lengths (RX and TX) and set frame sizes appropriately

Basically you want one full set of received samples to fit in a frame.
See many more details about this in “Transport layer - throughput limitations”
below.

## Run `tests/error_code_late_command_sweep.py` to figure out your max duty cycle

This will sweep across effective duty cycles from 100% to 1, testing the
equivalent pulse_rep_int for each one. The script produces a plot (saved as
`error_code_late_command.png`) that shows percent of errors versus duty cycle.

{{% imgproc duty_cycle_comparison_with_pi5 Fit "800x400" %}}
Example results from running `error_code_late_command_sweep.py` on several
combinations of host computers and SDRs.
{{% /imgproc %}}

# Tuning transport parameters

In addition to getting late commands, the other issues we can run into are
overflows (D on network-based devices, O on other devices) and underflows (U on
all devices). There are also sequence errors (S) that often go along with late
commands (L). Overflows mean that the buffers filled up and samples from the SDR
are not being consumed fast enough. Underflows mean that the SDR did not receive
samples to transmit before it needed to transmit those samples.

The Ettus knowledge base has a page detailing
[theoretical maximum transfer rates for each device](https://kb.ettus.com/About_USRP_Bandwidths_and_Sampling_Rates).

Depending on the device, data is transferred over USB using libUSB, GigE, 10GigE,
or PCI-E. For each option, there are various tunable parameters that impact the
packet sizes and buffers used.

For `libUSB`, the transport parameters are:

`(send/recv)_frame_size` is the maximum packet payload size in bytes. The
maximum number of samples that fits into a packet is `<packet size in bytes>/<bytes per sample>`.
The bytes per sample is determined by the over-the-wire format
(see [`otw_format`](https://files.ettus.com/manual/structuhd_1_1stream__args__t.html)).
For example, `sc12` requires 24 bit/samples, which is 3 bytes. It’s named `sc12`
because each value is 12 bits, but there are two values per samples (I, Q).

You can query the max number of samples based on your selected
`(send/recv)_frame_size` through the TX or RX streamers:
`tx_stream->get_max_num_samps()` or `rx_stream->get_max_num_samps()`

The default is around 8 MB (on my laptop and on the Pi 4B, both running
Ubuntu-derived variants). If you set it too high, you'll get an error message.

In most cases one full chirp (TX) or the received samples associated with one
full chirp should fit and a good choice is to set the frame sizes to be equal to
(or just barely larger than) the expected number of TX and RX samples,
respectively, per chirp.

`num_(send/recv)_frames` controls how many buffers of size
`(send/recv)_frame_size` are created. The more buffers you allocate for
receiving, the longer your host program can lag for before causing an overflow.

LibUSB will give you a memory error if you try to allocate too much memory
overall (roughly frame size * number of frames).

This is also why you don’t want arbitrarily large frame sizes. You end up
wasting RAM if you’re allocating buffers that are longer than the maximum packet
size you expect.

`num_recv_frames` is one of the most commonly talked about points on the USRP
mailing list for throughput issues on the b2xx devices. Apparently the default
value is very small (though I wasn’t able to figure out how to find the actual
default).

You set these parameters in the device arguments string. For us, that means
something like this in the config YAML:

```
device_args: "num_recv_frames=700,num_send_frames=700,recv_frame_size=11000,send_frame_size=11000"
```

As described above, the USRP example program benchmark_rate is very useful for
playing with these parameters quickly and figuring out what the limits are.

The host side matters a lot here too. The amount of RAM available on the host
directly limits the `num_(send/recv)_frames`. Processing speed of the host in
general can easily be the bottleneck. Finally, some USB 3.0 controllers are
known to work better than others. The Ettus knowledge base used to have a table
of performance on a few selected USB controllers. An archive of that page is
available [here](https://web.archive.org/web/20160421212045/https://www.ettus.com/kb/detail/usrp-b200-and-b210-usb-30-streaming-rate-benchmarks).

(The Pi 4 uses a VL805 controller. The Pi 5 has a new USB controller that performs far better.)