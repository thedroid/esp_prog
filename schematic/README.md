# Hardware

In order to enter the bootloader GPIO0 needs to be low at system start. Before the regular programming/flashing is taking place, the modified 'esptool' ensures that by using the spare RTS and DTR lines of the serial port. 
A reset is carried out by the RTS line while GPIO0 is held low via the DTR line.

Note that, we silently assume that GPIO2 and GPIO15 are pulled high during system start.

# Wiring

## Diagram

FTDI |Connection| ESP8266 | Note
-----|----------|---------|------
TX   |   ----   | RX      |
RX   |   ----   | TX      | 
RTS  | -220Ohm- | Reset   | see below
DTR  | -220Ohm- | GPIO0   | see below
Vio  | - 56Ohm- | Vdd     | Optional. Vio can alternativly be connected to 3.3Vout of the FTDI. Just ensure 3.3V levels.
GND  |   ----   | Vss     |


## Considerations
Open-drain output for DTR/RTS would be ideal for a true In System Programmer (ISP). In dormant state they would leave the actual circuit unaffected. However, the lines are active low. Meaning that during inactivity the lines are high. A double inverting MOSFET stage would be required to accomplish the desired behavior. As the same issues holds for the TX line I currently decided to go with the cheap solution. The more advanced solution would be driver IC which switches RTS, DTR, and TX into a Tri-Z state. The driver could be controlled by the DTR line.

## Pinout

The ESP-03/4 has the most GPIO pins. Thus, it is most likely that the majority of the people actively developing code for this chip will use this model and its specific pin configuration. 
The following pinout of the ISP is optimized for the usage of the ESP-03/4 board on top of a single sided PCB. A 6 pin (3x2) SMD header can easily be routed w/o intersections. Making it perfect for low-cost home-echted PCBs.
 
Pin header on the PCB (top view):

Description |   |   | Description
------------|---|---|------------
Vss         | 1 | 2 | Tx
Rx          | 3 | 4 | /Reset
Vdd         | 5 | 6 | GPIO0

Note that, for the ISP cable the RX/TX lines need to be twisted.

 


