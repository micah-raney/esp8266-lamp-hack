# esp8266-lamp-hack
Barebones project to rig a lamp to be controllable over a network.

# Wiring

Here is the completed wiring:

![close-up of base](https://github.com/micah-raney/esp8266-lamp-hack/blob/master/packed_base.JPEG)

A bit harrowing. Let's break it down.

The first place power comes from the lamp is to a standard, generic USB charger. That harrowing-looking transformer board is actually just the charger with the case removed. I removed the outlet prongs and attached black (L for Live) and white (N for neutral) wires instead, for splicing into the lamp's power cord.

![USB charger breakdown](https://github.com/micah-raney/esp8266-lamp-hack/blob/master/usb_charger.jpg)

![close-up of breadboard](https://github.com/micah-raney/esp8266-lamp-hack/blob/master/breadboard.JPEG)

Here is the chip circuitry itself. I'm using an Adafruit Feather Huzzah for my controller. It accepts 5V of input power from a microUSB connection, hence the charger. This particular model can also use a rechargeable LiPo, but why bother when it can leech the lamp power? Also, it wouldn't take much work to adapt this project for a NodeMCU board.

The gist of the circuit is as follows:
Pin 14 is programmed to turn on or off as the firmware dictates. Pin 14 opens an NPN gate, allowing the increased current of the main (3V) to flow through after hitting a mild resistor. That increased current is enough to trip the 3V relay I bought. A pack can be acquired on eBay for under $10; everywhere else I've looked, it's rare and expensive. Finally, the relay connects the red and white wires together. The color coding is a bit confusing, so I'll try at a flow chart.

![portrait image of wire splices](https://github.com/micah-raney/esp8266-lamp-hack/blob/master/wiring_portrait.JPEG)

From source Hot to neutral, the wiring path is as follows:

```
Hot Source ---> lamp bulb hot ---> lamp bulb neutral -> white wire on breadboard -> relay -> red wire -> Neutral Source
            L-> USB charger hot -> USB 5V -> Adafruit Feather Huzzah -> USB charger neutral ----------^
```

![lamp](https://github.com/micah-raney/esp8266-lamp-hack/blob/master/lamp_portrait.JPEG)

# Firmware
The firmware is just a minor tweak of example Arduino code. It connects to a given Wi-Fi signal (plug credentials in the code!) and hosts a webpage on the network. Right now, the mechanism for turning the lamp on or off is to access a different webpage (http://lamp/on or http://lamp/off) but it would be a cinch to do it with something different, such as a button press. 
The hostname code is kind of spotty in my experience. DNS hates it. You might have to use the ip address of the device instead.
