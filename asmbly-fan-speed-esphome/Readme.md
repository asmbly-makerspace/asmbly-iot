# HVAC Temp

We've want to be able to remotely control the fan speed of the filtration
system in the 3DP enclosure (based on air quality measurements). The fan
expects a PWM signal at 10V with the desired duty cycle corresponding to
the fan speed.

The idea is described here https://github.com/albahmed23/terrabloom_ec_fans/tree/main/arduino_platform/basic_fan_speed_control

We're using ESPHome and a ESP32-C3 Super Mini board, rather than the
Arduino they use.

## Setup

Broadly following
https://esphome.io/guides/getting_started_command_line/

Generating the ESPHome config

 # Set up python virtual environment in the venv direcotry
 /opt/homebrew/bin/python3 -m venv venv # or just python -m venv venv
 source venv/bin/activate.fish # choose the activate for your shell
 pip install wheel esphome

 # Set up the yaml
 esphome wizard asmbly-fan-speed.yaml
 name: fan-speed-3dp-enclosure
 microcontroller: esp32
 board: esp32-c3-devkitm-1
 wifi: Asmbly

We're just using this for a PWM output, so...
https://esphome.io/components/output/ledc/

And we're representing it as fan speed...
https://esphome.io/components/fan/speed/

After filling out asmbly-hvac.yaml, plug in the board over USB, and

 esphome run asmbly-fan-speed.yaml

Tell it to connect over the serial port. You should see it compile and
load the code onto the board, then see debugging info as it joins the
wifi.

At this point, if all has gone well, you should be able to connect to
Home Assistant and see that a new Esphome device is available. Add it
and enjoy your data.