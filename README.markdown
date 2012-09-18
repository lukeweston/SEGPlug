
**SEGPlug: A single-channel, single-phase, portable, wireless mesh-networked smart energy data acquisition and control appliance**

Design by Luke Weston, 2011-2012
(c) Luke Weston 2011-2012

This hardware design is released to you under the CERN Open Hardware License: http://ohwr.org/cernohl

The Smart Energy Groups SEGPlug is a small, visually unintrusive and portable device which plugs in to a domestic mains power outlet or cable and plugs into a mains-powered
load device. It is (with a small hardware reconfiguration) compatible with worldwide mains power grids, at 110-240VAC and 50-60 Hz. This "Internet of Things" device collects
data from its sensors and data acquisition circuits and transmits this data to a distributed network and ultimately to the Internet and receives commands from a distributed
network and/or the Internet and responds to such commands, for example by switching its onboard power relay on or off. 

SEGPlug acquires the voltage waveform across the load device(s) and the current waveform being consumed by the load device(s) and calculates instantaneous power draw as well
as energy consumption integrated across some period of time, and complex AC power characteristics such as phase angle, reactive power and power factor.

SEGPlug also incorporates a relay allowing the power supply to the load device to be completely switched off and switched on at the command of the device's (very low power)
internal microcontroller.

SEGPlug is powered from the mains power supply which it is plugged into - no additional power supplies or cables are required, everything is integrated into one clean and
simple plug-in cable.

SEGPlug sends and receives data and commands over a wireless radio mesh network, back to a gateway device which bridges the wireless mesh network onto an existing local area
network which is connected to the Internet - over ethernet, and/or standard IEEE 802.11 wireless networking (Wi-Fi), or 3G or similar cellular networks.

To ensure flexible compatibility with worldwide radiofrequency spectrum licensing and regulations and the device's local electromagnetic environment (with respect to spectrum
congestion, interference, or electromagnetic shielding, reflection or attenuation by nearby structures materials in the environment) SEGPlug's radio communications are handled
on a replaceable daughterboard which can be replaced and swapped with different boards or modules employing different communications frequencies or protocols.

My in-house daughterboard design based around the Atmel ATmega128RFA1 radio/microcontroller can be used, thus providing connectivity over IPv6 / 6LoWPAN etc, as well as
ZigBee. Alternatively, a commercial off-the-shelf product such as a Digi/MaxStream XBee module can be used, operating in the 2.4 GHz or 900 MHz spectra with the ZigBee or
DigiMesh protocols (which are, at their lower layers, also built on the 802.15.4 standard.) A daughterboard based around a sub-gigahertz radio device such as the Texas
Instruments CC1101 IC, operating in the 315/433/868/915 MHz bands can also be used, either in the form of a bespoke RF transceiver from us or a COTS device such as the
Seeed RFBee module.

SEGPlug also incorporates a temperature sensor, for measurement of the temperature in the local environment, a light sensor for sensing of the ambient light level, and support
for an optional PIR motion sensor for the detection of motion and an automated response - for example, turning on the room lights and/or reporting a message to the network.

![Alt text](https://github.com/lukeweston/SEGPlug/raw/master/SEGplug-pcb.png)

*Engineering notes for hardware developers, prototype testers, and early adopters:*

This device uses a non-isolated, transformerless mains power supply, with the mains active serving as the local DC ground. What this means is that all parts of the device,
including the microcontroller, XBee module, temperature sensor, and the light and PIR sensors if they're connected, are at mains potential when the device is plugged into the
mains, and all components represent an electric shock hazard. Do not not poke around with this device while it is powered up, with the device not within an earthed enclosure,
or a double-insulated plastic enclosure, unless you know exactly what you're doing.

Also, DO NOT plug the device into any kind of FTDI cable, ISP programmer or any other serial communications interface or programming interface while it is plugged into the
mains. If you do, this will connect your computer's local DC ground to the 240VAC active, and your computer will be destroyed and will probably explode or something, as well
as presenting an electric shock hazard. So don't do it.

It's OK to plug those kinds of interfaces in for programming, obviously, just make damned sure that the mains is unplugged first.

There's nothing "wrong" with a transformerless power supply, though, as long as you understand what it is and what its characteristics are. You'll find very much the same kind
of circuit inside a commercial "kill-a-watt" device, or any similar product. As long as the device does not need to have any cables for data communications out to a computer
or another device, it works absolutely fine. In fact, using this kind of circuit is the only way you're going to keep the size, weight and cost of the device down so that it
is competitive with those kinds of similar commercial products.

The transformerless power supply uses a relatively large cap (2.2 uF) because it needs to supply a relatively large amount of current (about 80 mA) at peak times in order to
reliably run the XBee module (which draws about 50 mA by itself) as well as the relay and AVR without rebooting or crashing the AVR whenever there is a momentary spike in
current draw, whenever the XBee starts transmitting or something like that.

High current means low capacitive reactance which means relatively big capacitor.

Commercial RF modules with relatively high power consumption, such as the Digi/Maxstream "XBee Pro" are unsupported with this system due to power supply constraints in this
design variant and probably won't work reliably, although the standard, lower-power Digi/MaxStream XBee modules are supported. Basically, when using commercial third-party
RF communications modules, the maximum device current consumption during transmission should not exceed 50 mA at 3.3 volts.

It would straightforward to design a safe, optically-isolated plug-in serial cable interface for your computer if something like that was required... but at this point in
time, no such device exists and plugging the unit into your computer (unless the mains is disconnected) is a big no-no.

The maximum power rating of the unit is 10 A at 240 VAC. Do not exceed this limit.

This device is designed for use with 240 VAC mains only. Do not use in a country with a 120 VAC mains supply. It should be possible to change the value of the
capacitive reactance in the mains neutral so that it is OK with 120 VAC operation (which probably means changing the 2.2 uF capacitor to about double its capacitance), but at
the present time this has not been tested and is not supported at all.

The control pushbutton must be rated for mains use. If there is not sufficient electrical isolation between the switch contacts and the plastic switch actuator, an electric
shock hazard may exist during use of the button.

In order to prevent electric shock, fire, explosion, death, etc, careful quality control is required if you're assembling this unit from a kit yourself. Solder everything
carefully, and check that you've got the components oriented correctly with the right kinds of components in the right places.

The 470 uF electrolytic capacitor must be installed with the correct polarity, and the AVR microcontroller must be soldered in with the right orientation.
The 5 different diodes in the circuit all must be installed with the correct polarity and with the right type of diode used in the right spot on the PCB.

**Microcontroller notes, and wireless programming**:

The AVR (which is essentially an Arduino, if it is programmed with an Arduino bootloader) can be reprogrammed remotely via the XBee, if it is programmed with the Arduino
bootloader. Follow the directions here:

http://www.ladyada.net/make/xbee/arduino.html

Note that the hardware configuration at the remote end is already implemented on the board... the XBee modules just need to be configured appropriately and the XBee module
at the host end appropriately connected to a UART interface to the PC.

Note that the AVR is clocked from an 8 MHz crystal and it is running at 3.3V. Therefore, the Arduino IDE must be told that the target device is a 3.3V 8 MHz Arduino prior to
programming.

It is perfectly fine to perform XBee wireless communications and XBee program downloads while the device is plugged into the mains power supply, of course.

During "wired" flashing or programming (either with the ISP programmer or with a FTDI serial cable), the *XBee module must be removed from its socket*.

**Temperature sensor**:

A substantially cheaper Microchip MCP9701 temperature sensor IC with a linear analog output is used to replace the relatively expensive DS18B20 temperature sensor.

**Measurement resolution**:

The power measurement resolution or "granularity" should be about 2.5W per LSB, when operating from 230VAC.

