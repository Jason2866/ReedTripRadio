### Description
This is an alternative firmware for wireless 433MHz magnetic door/window reed sensors.

STC15W101/104 are 8051 based processors + SYN115 radio transmitter.  
'101 model has 1KB flash.  
'104 model has 4KB flash and circuit board also has a tamper detect switch.
 
Instead of supporting software serial or full hardware abstraction layer (HAL),  
it is decided to keep firmware under 1KB so it works on multiple processors/boards.  
One possibility however is to use emulated EEPROM area for code space.

Experiments with booting from other memory spaces have not worked so far:  
https://github.com/mightymos/stc15bootisp/issues/1

Finally, STC processors do not allow read/verify of written firmware.  
Therefore an open source alternative is needed to confirm program behavior.  
Also for this reason original firmware can not be reflashed once overwritten.  

Boards contain a header that may be populated with pins labeled with G (ground), T (transmit), and R (receive) for flashing with USB to UART module.

### Features

| Proposed | original or added | status |
| ------------- | ------------- | ------------- |
| Transmit on reed switch open/close (interrupt)  | original  | DONE |
| Transmit on tamper switch open/close (interrupt)  | original  | DONE |
| Manage power modes  | original  | DONE |
| Support inverted protocols  | added  | DONE |
| Ability to specify timings for transmission protocol (e.g. rc-switch)  | added  | DONE |
| "Heart beat" mode for periodic transmission   | added  | DONE |
| Add tamper closed key  | added  | DONE |
| Add tamper "trip" mode   | added  | DONE |
| Send code if pin goes low indicating low battery   | original  | DONE |
| Store settings in EEPROM  | added  | todo |
| Send information over radio (e.g., settings?, battery?)  | added  | todo |
| Compare power usage to original firmware  | added  | todo |
| Test other transmission protocols  | added  | todo |
| Adjustable LED blink behavior   | added  | todo |
| Adjustable sleep behavior  | added  | todo |
| User configuration/input with tamper switch press(es) | added  | todo |

![alt text](/photos/water_leak_store_hookup_example.jpg "Wireless 433 MHz Door Sensor")

### Installation
```
# install https://sdcc.sourceforge.net/ for your platform

cd ~/

# flashing tool
git clone https://github.com/area-8051/stcgal-patched.git

# hardware abstraction layer for stc
git clone https://github.com/area-8051/uni-STC.git
cd uni-STC/demos/

# this repository
git clone https://github.com/mightymos/ReedTripRadio.git
cd ReedTripRadio/

# build
make

# flash over usb to uart (serial)
make upload
```

### Receiver Hardware
Receiving radio packets requires a receiver. Options include the Sonoff RF Bridge 433 MHz and recommend flashing with open source firmware [Tasmota](https://tasmota.github.io/docs/devices/Sonoff-RF-Bridge-433/ "Tasmota") or [ESPurna](https://github.com/xoseperez/espurna "ESPurna"). ESPurna is nice because it treats wireless sensors as "virtual" sensors (show up as permanent switch entities in Home Assistant). Also ESPurna can learn/remember unique sensor codes.

Some Sonoff Bridge(s) contain an onboard EFM8BB1 which can additionally be flashed to support more radio protocols with [Portisch](https://github.com/Portisch/RF-Bridge-EFM8BB1 "Portisch"). I originally thought this would be helpful but apparently most rc-switch protocols are not supported.

You can also use a generic 433 MHz receiver and controller using [rc-switch](https://github.com/sui77/rc-switch) library.

### Wireless door/window sensor
| Source | Link | Price (USD) |
| ------------- | ------------- | ------------- |
| aliexpress  | https://www.aliexpress.com/item/3256803337417240.html  | $4.09 (12/05/2022) |
