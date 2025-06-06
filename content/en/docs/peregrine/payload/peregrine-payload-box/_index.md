---
title: Peregrine Radar Payload Box Assembly
description: Build instructions for the core of the radar payload
weight: 2
resources:
  - src: "*.{png,jpg}"
---

<!-- {{< figure src="payload-box-cad.png" title="Cut-away view of a CAD rendering of the radar payload box" caption="Major components of the radar payload box" >}} -->


{{% imgproc payload-box-cad Fit "800x400" %}}
Major components of the Peregrine radar payload box
{{% /imgproc %}}

## Bill of Materials

A complete bill of materials for the payload box is available in [this Google Sheet](https://docs.google.com/spreadsheets/d/1dFItvJ4mGw7Axdo4Iq9m_65ks9YspEahcemvucNRllk/edit?usp=sharing), embedded below.

Some parts are custom made. See the [Custom Parts]({{< ref "custom-parts.md" >}}) page for suggestions on how to build these yourself or source them from reliable vendors.

{{% iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vSJbgsZBh6xJ8d5_O48yUdmh09coE1oZ8njQWZqQKYorwuMoUEFA6LCj9P5QiVQtoCGBCOOl3b1RTjY/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" style="min-height: 40vh;" %}}

## Assembly instructions

### Preparing enclosure halves

The payload box is a clamshell-style design with two interlocking halves. The
"upper" half contains the SDR. The "lower" half holds the Raspberry Pi.
Sandwiched between the two halves is the "payload divider" PCB, which houses
some sensors and provides for external connections.

Starting with the Pi Shell:

1. Place 4x M2.5 heat inserts into the four mounting holes at the bottom of the box.
2. Place 2x M2.5 heat inserts into the two front panel mounting holes on the
front of the box.

Continuing with the SDR Shell:

3. Place 4x M2.5 heat inserts into the four holes in the tabs sticking up, to
accept bolts from the outside to connect the two shells together.

### Copper foil wrapping

{{% alert title="Why is the box wrapped in copper tape?" color="info" %}}
The SDR and the Pi communicate over a USB 3 interface. Unfortunately, USB 3 has
known RF interference issues. The copper foil wrapping helps to minimize the
impact of the noisy USB 3 connection on other devices. We have particularly
had problems with GPS receivers (such as the one connected to the Peregrine
autopilot) not being able to get a fix without this copper wrapping.

You can read more about the USB 3 interference issues [from this Intel
appnote](https://www.usb.org/sites/default/files/327216.pdf).
{{% /alert %}}

Print out the cutout templates **TODO LINK**. The first two pages are the
cutout templates for the copper foil wrapping on the top and bottom of the
enclosure.

{{% alert title="Double check the dimensions" color="warning" %}}
Use a ruler to measure the marked dimensions on the first page to make sure your
print is at the right scale.
{{% /alert %}}

Spread a layer of adhesive-backed copper out on a cutting-safe surface
(such as a sacrificial piece of wood) and use some masking tape to hold it down.
Tape the cutout template on top. Use an X-Acto knife (or similar) to cut:

1. A small "X" across each of the external bolt holes (to allow a bolt to go
through -- no need to try to cut out the circle itself)
2. The internal cutout for the heatsink
3. The exterior outline of the entire piece.

If you're not sure which lines to cut, take a moment to see how the paper folds
around the 3D printed pieces. Your goal is to fully cover the exterior.

Carefully peel away the adhesive from part of the copper foil and start pasting
it onto the 3D printed piece. It's easier to do this bit by bit, rather than
pulling the entire backing off at once.

Repeat with the other half of the enclosure.

