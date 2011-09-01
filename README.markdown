Smart Plug v1.0 (preliminary)
Single-channel, single-phase, portable, wireless smart energy appliance

Design by Luke Weston, August 2011.
Released as Open Hardware under the CERN Open Hardware License.

github.com/lukeweston/SEGplug

General background notes and safety notes, and guidelines for reliable use:
---------------------------------------------------------------------------

No hardware prototype of this design has been built yet. It might not work at all, it might catch fire, it might explode. Right now, I can't say for sure until a prototype
is built.

Just so that we're very clear here, I'm not liable at all if you try and build this and it burns your house down and kills your family.

This device uses a non-isolated, transformerless mains power supply.
This means that the +3.3V rail that runs the microcontroller is connected to the mains 240VAC active, and the DC ground is sitting 3.3 volts below that.
What this means is that all parts of the device, including the microcontroller, XBee module, etc, are at mains potential when the device is powered up, and they are an
electric shock hazard. Do not not poke around with this device while it is powered up, with the device not mounted within a safe, insulated box, unless you know exactly what
you are doing.

Also, DO NOT plug the device into any kind of FTDI cable, ISP programmer or any other serial communications interface or programming interface while it is plugged into the
mains. If you do, this will connect your computer's 5V rail across 240VAC, and your computer will be destroyed and will probably explode or something, as well as presenting
an electric shock hazard. So don't do it.

It's OK to plug those kinds of interfaces in for programming, obviously, just make damned sure that the mains is unplugged first.

There's nothing "wrong" with a transformerless power supply, though, as long as you understand what it is and what its characteristics are. You'll find very much the same kind
of circuit inside a commercial "kill-a-watt" device, or any similar product. As long as the device does not need to have any cables for data communications out to a computer
or another device, it works absolutely fine. In fact, using this kind of circuit is the only way you're going to keep the size, weight and cost of the device down so that it
is competitive with those kinds of similar commercial products.

The transformerless power supply uses a relatively large cap (2.2 uF) because I needed it to supply a relatively large amount of current (about 100 mA) in order to reliably
run the XBee module (which draws about 50 mA by itself) as well as the AVR and miscellaneous LEDs, triac etc. without rebooting or crashing the AVR whenever there is a
momentary current spike, whenever the XBee starts transmitting or something like that.

High current means low capacitive reactance which means relatively big capacitor.

Only use standard low-power XBee modules with this unit. The high-performance, relatively high power variants like the "XBee Pro" probably won't work.

It would probably be possible to build a safe, optically-isolated plug-in serial cable interface for your computer if something like that was required... but at this point in
time, no such device exists and plugging the unit into your computer (unless the mains is disconnected) is a big no-no.

The maximum power rating of the unit is 10 A at 240 VAC. Do not exceed this limit.

This device is designed for use with 240 VAC mains only. Do not use in a country with a 120 VAC mains supply. It should be possible to change the values of the resistor and
capacitor in the transformerless power supply so that it is OK with 120 VAC operation, but at the moment this is not supported or developed or tested.

The control pushbutton must be rated for 250VAC use. If there is not sufficient electrical isolation between the switch contacts and the plastic switch actuator, an electric
shock hazard may exist during use of the button.

Construction notes
------------------

In order to prevent electric shock, fire, explosion, death, etc, careful quality control is required if you're assembling this unit from a kit yourself. Solder everything
carefully, and check that you've got the components oriented correctly with the right kinds of components in the right places.

- The 470 uF electrolytic capacitor must be installed with the correct polarity. The voltage rating of this capacitor is not critical; it can be 10-25 V.
- The 2.2 uF capacitor must be a 250 VAC mains-rated (X2 class or similar) polymer film capacitor. Digi-Key P10738-ND is a suitable candidate. This capacitor is not polarised.
- The 100 nF 250 VAC capacitor must be a mains-rated (X2 class or similar) polymer film capacitor. Digi-Key P10730-ND is a suitable candidate. This capacitor is not polarised.
- The 47 R resistor must have a power dissipation rating of at least 1 watt. The Digi-key 47W-1-ND is a suitable candidate.
- The 110 R resistor must have a power dissipation rating of at least 3 watts. 5 watts is better. The Digi-Key P110W-3BK-ND is a suitable candidate.
- ZD1 is a 1N4730 or any other similar 1 watt Zener diode with a Zener voltage of 3.9 volts.
- D1 is a pretty standard 1N4007 diode.

- The S212S01F solid-state relay has a maximum load current of 12 A, although in this case the shunt resistor limits the circuit's load current to 10 A.
- A heatsink will probably be required on the solid-state relay.

- The current-sense shunt resistor is 10 milliohms, 5 watts. Digi-Key 989-1097-ND is the recommended device.

Check the following carefully during assembly:

- Check that the 1N4007 diode and the 1N4730 diode are both oriented the correct way.
- Check that the 1N4007 diode and the 1N4730 diode are not swapped with each other.
- Check that the 470 uF electrolytic capacitor is oriented the correct way.
- Check that the ATmega328 microcontroller chip is installed the correct way.


Microcontroller notes, and wireless programming
-----------------------------------------------

The AVR (which is essentially an Arduino, if it is programmed with an Arduino bootloader) can be reprogrammed remotely via the XBee, if it is programmed with the Arduino
bootloader. Follow the directions here:

http://www.ladyada.net/make/xbee/arduino.html

Note that the hardware configuration at the remote end is already implemented on the board... the XBee modules just need to be configured appropriately and the host end
set up for wireless programming.

Note that the AVR is clocked from an 8 MHz crystal and it is running at 3.3 V. Therefore, the Arduino IDE must be told that the target device is a 3.3V 8 MHz Arduino prior to
programming.

It is perfectly fine to perform XBee wireless communications and XBee program downloads while the device is plugged into the mains power supply, of course.

During "wired" flashing or programming (either with the ISP programmer or with a FTDI serial cable), the XBee module must be removed from its socket.


Temperature sensor
------------------

A LM35 temperature sensor with a linear analog output is used to replace the relativly expensive DS18B20 temperature sensor.

Measurement resolution
----------------------

The measurement resolution or "granularity" should be about 7 W per LSB, when operating from 240VAC.

