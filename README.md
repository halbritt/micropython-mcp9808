# micropython-mcp9808 - ESP8266/WiPy version
This repository contains a MicroPython "driver" implementation for the MCP9808
temperature sensor from Microchip.

Currently only a reasonably small subset of the features the sensor is
implemented.

## Implemented
* Reading the temperature value in degree celsius. The `read()` method supports floating point values and the `read_int()` method does not use floating point arythmetic at all and does return a tuple of decimal and fraction parts of the temperature reading.
* Shutdown mode to save power. When not in shutdown mode the sensor
does draw 200-400 uA. Data acquisition can only be stopped in the so called
"shutdown mode". In this mode communication is still possible using I2C.
* Changing the data acquisition resolution and duration. On power up the sensor
is set to maximum resolution.

    | Mode Name | Resolution  | Duration | Samples / sec |
    | --------- | ----------- | -------- | ------------- |
    | T_RES_MIN | +-0.5 °C    |    30 ms | 33            |
    | T_RES_LOW | +-0.25 °C   |    65 ms | 15            |
    | T_RES_AVG | +-0.125 °C  |   130 ms |  7            |
    | T_RES_MAX | +-0.0625 °C |   250 ms |  4            |

## Not yet implemented
* Alert temperature boundaries, hysteresis and comparator mode
* Interrupt mode

## Usage

~~~python
# 5 : D1 : SDL
# 4 : D2 : SDA
from machine import Pin, I2C
import mcp9808

i2c = I2C(scl=Pin(5), sda=Pin(4), freq=10000)
mcp = mcp9808.MCP9808(i2c=i2c)
mcp.set_resolution(mcp9808.T_RES_AVG)
temp = mcp.get_temp()

print "Temperature is: {}".format(temp)
~~~
