**SEG Plug v0.5 (preliminary)**:
**Single-channel, single-phase, portable, wireless smart energy appliance**

Design by Luke Weston, 2011-2012.
Released as Open Hardware under the CERN Open Hardware License.

github.com/lukeweston/SEGplug

![Alt text](https://github.com/lukeweston/SEGPlug/raw/master/SEGplug-pcb.png)

**General background notes and safety notes, and guidelines for reliable use**:

No hardware prototype of this design has been built yet. It might not work at all, it might catch fire, it might explode. Right now, no real support or warranty is even
remotely possible until a prototype is built.

Just so that we're very clear here, I'm not liable at all if you try and build this and it burns your house down and kills your family.

This device uses a non-isolated, transformerless mains power supply, with the mains active serving as the local DC ground.
What this means is that all parts of the device, including the microcontroller, XBee module, temperature sensor, and the light and PIR sensors if they're connected, are at
mains potential when the device is plugged in, and they are an electric shock hazard. Do not not poke around with this device while it is powered up, with the device not
mounted within a safe, insulated box, unless you know exactly what you're doing.

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

Only use standard low-power XBee modules with this unit. The high-performance, relatively high power variants like the "XBee Pro" probably won't work.

It would straightforward to design a safe, optically-isolated plug-in serial cable interface for your computer if something like that was required... but at this point in
time, no such device exists and plugging the unit into your computer (unless the mains is disconnected) is a big no-no.

The maximum power rating of the unit is 10 A at 240 VAC. Do not exceed this limit.

This device is designed for use with 240 VAC mains only. Do not use in a country with a 120 VAC mains supply. It should be possible to change the value of the
capacitive reactance in the mains neutral so that it is OK with 120 VAC operation (which probably means changing the 2.2 uF capacitor to about double its capacitance), but at
the present time this has not been tested and is not supported at all.

The control pushbutton must be rated for mains use. If there is not sufficient electrical isolation between the switch contacts and the plastic switch actuator, an electric
shock hazard may exist during use of the button.

**Construction notes**:

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

