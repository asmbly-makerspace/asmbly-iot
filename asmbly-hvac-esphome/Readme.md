# HVAC Temp

We've been having trouble with our HVAC system freezing over. This is a simple
ESPHome based temperature sensor.

## Setup

Broadly following
https://esphome.io/guides/getting_started_command_line/

Generating the ESPHome config

 # Set up python virtual environment in the venv direcotry
 /opt/homebrew/bin/python3.13 -m venv venv # or just python -m venv venv
 source venv/bin/activate.fish # choose the activate for your shell
 pip install wheel esphome

 # Set up the yaml
 esphome wizard asmbly-hvac.yaml
 name: hvac0
 microcontroller: esp32
 board: adafruit_feather_esp32s3_tft
 wifi: Asmbly

We're using an NTC Sensor, so...
https://esphome.io/components/sensor/ntc/
That references
https://esphome.io/components/sensor/resistance/

We're using components that are so generic that we can just use the sample:

 # Example configuration entry
 sensor:
  - platform: ntc
    sensor: resistance_sensor
    calibration:
      b_constant: 3950
      reference_temperature: 25°C
      reference_resistance: 10kOhm
    name: NTC Temperature

  # Example source sensors:
  - platform: resistance
    id: resistance_sensor
    sensor: source_sensor
    configuration: DOWNSTREAM
    resistor: 5.6kOhm
    name: Resistance Sensor
  - platform: adc
    id: source_sensor
    pin: A0

After filling out asmbly-hvac.yaml, plug in the board over USB, and

 esphome run asmbly-hvac.yaml

Tell it to connect over the serial port. You should see it compile and
load the code onto the board, then see debugging info as it joins the
wifi.

At this point, if all has gone well, you should be able to connect to
Home Assistant and see that a new Esphome device is available. Add it
and enjoy your data.