# Polverine Air Quality Monitor

We'd like to keep track of the air quality in the 3DP enclosure where the resin
printing currently happens. We found a fairly cheap board with a pair of sensors
and an ESP32-based microcontroller that looked pretty good. The web page from
the company that makes it is currently unavailable, so maybe that wasn't the best
choice.

The board has a bunch of unused GPIO pins, and, in theory it would be possible
to *also* do the fan control (see asmbly-fan-speed-esphome) on the same board,
but I had enough trouble building and installing the firmware for just the sensors
that I'm not excited about doing that right now.

I would have loved to use ESPHome for the whole thing, but the support for these
particular sensors is not included yet. See:

https://github.com/esphome/esphome/pull/13480
https://gist.github.com/jarz/415874fac81f0e813d6d3ece221c3992
https://github.com/sweitzja/esphome-bmv080

When that settles (check again in Summer 2026), we can revisit this if we care.


## Setting up the Polverine

At its core, the process is as "simple" as following the instructions here:
https://github.com/BlackIoT/Polverine/tree/main/POLVERINE_HOMEASSISTANT_DEMO

I used VSCode and installed the Platformio extension, which handled the build
and install of the firmware.

However, the instructions are wrong about how to set the credentials. See:
https://github.com/BlackIoT/Polverine/pull/32

I was surprised to find that leaving out the MQTT username and password caused
the connection to fail _even though the MQTT broker I was using does not have
authentication turned on_. I think that the values it got for those parameters
was uninitalized memory or something and caused a problem. Setting the values
to empty strings had the same problem, but using dummy values like `username`
and `password` worked fine.

## Home Assistant Integration

The POLVERINE_HOMEASSISTANT_DEMO does the right thing for HA to discover the
device using MQTT. Once it's running, it should be as simple as going to the
"Devices & services" config and accepting the discovered MQTT device.