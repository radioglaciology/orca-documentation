---
title: Sourcing Custom Parts
description: Suggestions for getting custom parts made
weight: 10
---

Many of the parts used to build Peregrine are custom designs. In some instances,
these may be things you can build yourself if you have the right tools
(3D printer, laser cutter, etc). In almost all cases, however, it is also
possible to use our provided design files to order parts from reliable vendors.
For each type of manufacturing, this page includes some notes about how we
produce parts or which external vendors we have worked with.

You can find design files for the hardware components of Peregrine in the
[Peregrine Hardware repository](https://github.com/thomasteisberg/peregrine_hardware).

## 3D Printing

All of our designs rely heavily on 3D printing. We use a
[Prusa MK3S+](https://www.prusa3d.com/category/original-prusa-i3-mk3s/) and
[Prusament ASA Lipstick Red](https://www.prusa3d.com/product/prusament-asa-lipstick-red-850g/)
filament.

All of our designs should be printable on any common FDM printer.

Outsourced 3D printing is expensive relative to the low costs of desktop 3D
printers, especially for parts (like ours) that generally don't need sub-mm
tolerances. That said, there many 3D printing services. I have had good
experiences with [Shapeways](https://www.shapeways.com/). If you have a vendor
produce your parts and you are intending to use them in a UAV, be sure to
specify that you don't want them to be printed solid. We usually print at about
20% infill density. Solid parts will weigh a lot more.

ASA is our plastic of choice because it forms parts that are relatively strong,
relatively light weight, work over an extended temperature range, and suffer
minimal UV degradation. The downside is that it is somewhat more difficult to
print with, as compared to PLA filament. We use an enclosure to provide a heated
build volume. Almost all prints are done with a brim (setting available in your
slicing software) to avoid warping.

PLA is great for prototyping but likely not a good choice for critical outdoor
applications. PLA can easily get hot enough to warp in direct sunlight and may
degrade under extended exposure to UV.

ABS or other similar materials should be fine.

Note that filament type, nozzle size, and slicing settings will all make a big
difference to the final weight of your parts.

## Laser Cutting

All laser cut parts are produced by [SendCutSend](https://sendcutsend.com/).
Ponoko, Xometry, and many other options also exist. If you want to match our
materials exactly, though, SendCutSend will be easiest.
If you have a laser cutter, you could also make them yourself.

## CNC Machining

We usually have CNC machined parts made through [HUBS](https://www.hubs.com/),
which distributes jobs to a network of machine shops. They are not the cheapest
option, but they provide a nice balance between cost, speed, and reliability.

## Printed Circuit Boards (PCBs) and PCB Assembly

You can send most of our design files to any PCB shop you like working with.

The one exception are the Peregrine under-wing antennas, which are larger
(about 56 cm by 11 cm) than the maximum dimensions allowed for standard orders
at many PCB manufacturers. Additionally, we usually get these printed on thinner
FR4 to reduce weight. These two factors make them very expensive at some board
shops. [PCBWay](https://www.pcbway.com/) will produce 5 (un-assembled boards)
for less than $150 including shipping to the US.