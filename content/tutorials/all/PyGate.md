## Pygate

__To connect your Pygate to a LoRa server, follow these steps:__  

1- Attach a Pycom development board e.g. a Wipy, LoPy4, GPy, to the Pygate. (The RGB LED of the development board should be aligned with the USB port of PyGate)

2- Attach the LoRa Antenna to the Pygate 

3- Flash the Pycom Device with latest PyGate Firmware.

4- Upload the Gateway configuration json file onto the attached Pycom device (via Pymakr on Atom or VSCode). Depending on the type of Pygate (EU868/US915) you should have different config files.

__The following example will demonstrate a simple script for getting started with a Pygate via a Wifi connection for the EU868 region:__

You can use the same file, just enter your GW unique ID, the LoRa server address and port numbers.

```python
from network import WLAN
import time
import machine
from machine import RTC
import pycom

#Disable Hearbeat
pycom.heartbeat(False)

#Define callback function for Pygate Events
def machine_cb (arg):
    evt = machine.events()
    if (evt & machine.PYGATE_START_EVT):
        # Green
        pycom.rgbled(0x103300)
    elif (evt & machine.PYGATE_ERROR_EVT):
        # Red
        pycom.rgbled(0x331000)
    elif (evt & machine.PYGATE_STOP_EVT):
        # RGB off
        pycom.rgbled(0x000000)
        
# register Callback func
machine.callback(trigger = (machine.PYGATE_START_EVT |
machine.PYGATE_STOP_EVT | machine.PYGATE_ERROR_EVT), handler=machine_cb)

# Connect to a Wifi Network
wlan = WLAN(mode=WLAN.STA)
wlan.connect(ssid='<SSID>', auth=(WLAN.WPA2, "<PASSWORD>"))

while not wlan.isconnected():
    time.sleep(1)

print("Wifi Connection established")

#Sync time via NTP server for GW timestamps on Events
rtc = RTC()
rtc.ntp_sync(server="0.nl.pool.ntp.org")

#Read the GW config file from Filesystem
fp = open('/flash/config.json','r')
buf = fp.read()

# Start Pygate
machine.pygate_init(buf)

```

A sample Config json file for GW configuration on EU868 region:

```json
{
	"SX1301_conf": {
		"lorawan_public": true,
		"clksrc": 1,
		"antenna_gain": 0,
		"radio_0": {
			"enable": true,
			"type": "SX1257",
			"freq": 867500000,
			"rssi_offset": -164.0,
			"tx_enable": true,
			"tx_freq_min": 863000000,
			"tx_freq_max": 870000000
		},
		"radio_1": {
			"enable": true,
			"type": "SX1257",
			"freq": 868500000,
			"rssi_offset": -164.0,
			"tx_enable": false
		},
		"chan_multiSF_0": {
			"enable": true,
			"radio": 1,
			"if": -400000
		},
		"chan_multiSF_1": {
			"enable": true,
			"radio": 1,
			"if": -200000
		},
		"chan_multiSF_2": {
			"enable": true,
			"radio": 1,
			"if": 0
		},
		"chan_multiSF_3": {
			"enable": true,
			"radio": 0,
			"if": -400000
		},
		"chan_multiSF_4": {
			"enable": true,
			"radio": 0,
			"if": -200000
		},
		"chan_multiSF_5": {
			"enable": true,
			"radio": 0,
			"if": 0
		},
		"chan_multiSF_6": {
			"enable": true,
			"radio": 0,
			"if": 200000
		},
		"chan_multiSF_7": {
			"enable": true,
			"radio": 0,
			"if": 400000
		},
		"chan_Lora_std": {
			"enable": true,
			"radio": 1,
			"if": -200000,
			"bandwidth": 250000,
			"spread_factor": 7
		},
		"chan_FSK": {
			"enable": true,
			"radio": 1,
			"if": 300000,
			"bandwidth": 125000,
			"datarate": 50000
		},
		"tx_lut_0": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_1": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_2": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_3": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_4": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_5": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_6": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 9,
			"dig_gain": 3
		},
		"tx_lut_7": {
			"pa_gain": 0,
			"mix_gain": 6,
			"rf_power": 11,
			"dig_gain": 3
		},
		"tx_lut_8": {
			"pa_gain": 0,
			"mix_gain": 5,
			"rf_power": 13,
			"dig_gain": 2
		},
		"tx_lut_9": {
			"pa_gain": 0,
			"mix_gain": 8,
			"rf_power": 14,
			"dig_gain": 3
		},
		"tx_lut_10": {
			"pa_gain": 0,
			"mix_gain": 6,
			"rf_power": 15,
			"dig_gain": 2
		},
		"tx_lut_11": {
			"pa_gain": 0,
			"mix_gain": 6,
			"rf_power": 16,
			"dig_gain": 1
		},
		"tx_lut_12": {
			"pa_gain": 0,
			"mix_gain": 9,
			"rf_power": 17,
			"dig_gain": 3
		},
		"tx_lut_13": {
			"pa_gain": 0,
			"mix_gain": 10,
			"rf_power": 18,
			"dig_gain": 3
		},
		"tx_lut_14": {
			"pa_gain": 0,
			"mix_gain": 11,
			"rf_power": 19,
			"dig_gain": 3
		},
		"tx_lut_15": {
			"pa_gain": 0,
			"mix_gain": 12,
			"rf_power": 20,
			"dig_gain": 3
		}
	},

	"gateway_conf": {
		"gateway_ID": "XXXXXXXXXXXXXXXX",
		"server_address": "router.eu.thethings.network",
		"serv_port_up": 1700,
		"serv_port_down": 1700,
		"keepalive_interval": 10,
		"stat_interval": 30,
		"push_timeout_ms": 100,
		"forward_crc_valid": true,
		"forward_crc_error": false,
		"forward_crc_disabled": false
	}
}
```
To stop the Pygate at any time use:

- REPL -> use CTRL-C
- using deinit function `machine.pygate_deinit()`

This will stop GW tasks and safely power-off the Concentrator.


__Note__: The Pygate packet forwarder is a legacy packet forwarder, so you must make sure you use select the legacy packet forwarder option in TTN as shown below.

![alt_text](https://wiki.dragino.com/images/c/c6/TTN_Create_Gateway_0.png)


## Pygate APIs

###machine Module

#### machine.pygate\_init([buff])

This function is used to initialize the Pygate 

- `buff`: the data contents of the gateway global config json file

When no parameter is passed to function the Pygate is just powered on. (will be useful when using pygate as just a concentrator controllable via uart by another device eg. RPi)

#### machine.pygate\_deinit()

This shuts down the concentrator.

#### machine.pygate\_cmd\_decode(buff)

This sends the LoRa gateway command to the concentrator. This is useful when packet forwarder / HAL software is run on a different device (e.g. Rpi) and commands to the concentrator are passed to the Pygate via UART.

#### machine.pygate\_cmd\_get()

This gets the command execution result from concentrator.

An example script demonstrating running packet forwarder software on a different device:

```python
from machine import UART
import machine
import time
import os
import gc

machine.pygate_init(None)
time.sleep(3)

uart = UART(1, 115200, timeout_chars=40, pins=('P23', 'P22'))

while True:
    if uart.any():
        rx_data = uart.read()
        machine.pygate_cmd_decode(rx_data)
        tx_data =  machine.pygate_cmd_get()
        l = uart.write(tx_data)
    else:
        time.sleep_us(10)
```

#### machine.callback(trigger, handler=None, arg=None)

- `trigger`: A trigger event(s) for invoking the callback function `handler`, the triggers/events are:

	`machine.PYGATE_START_EVT`

	`machine.PYGATE_STOP_EVT`

	`machine.MP_QSTR_PYGATE_ERROR_EVT`

- `handler`: The callback function to be called.  When not passed to function, any pre-registered callback will be disabled/removed.

- `arg`: Optional arg to be bassed to callback function.

#### machine.events()

Get the Pygate events


## Pygate Ethernet adapter APIs

`network.ETH` module

### ETH.init(hosname=None)

This function starts the Ethernet interface and enables the ethernet adapter.

`hostname`: set the interface hostname

### ETH.	ifconfig(config=\['dhcp' or configtuple\])

With no parameters given, this returns a 4-tuple of (ip, subnet_mask, gateway, DNS_server).

If DHCP is passed as a parameter, then the DHCP client is enabled and the IP params are negotiated with the DHCP server.

If the 4-tuple config is given then a static IP is configured. For instance:

`eth.ifconfig(config=('192.168.0.4', '255.255.255.0', '192.168.0.1', '8.8.8.8'))`

### ETH.hostname(string)

Set the interface host name.

### ETH.mac()

Get the ethernet interface mac address.

### ETH.deinit()

Shuts down the ethernet interface.

### ETH.isconnected(Bool)

Returns `True` if the ethernet link is up and IP is accquired, `Fasle` if otherwise

### ETH.register(reg, cmd, value)

Write/read specific register from/to the ksz8851 ethernet controller

`cmd`: 0 to read , 1 to write

Ex: to read register 0x90

`eth.register(0x90,0)`

To write:

`eth.register(0x90, 1, 0x0000)`
