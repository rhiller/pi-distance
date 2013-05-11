Raspberry Pi distance measurement with HC-SR04 Ultrasonic Distance Sensor

Used for sump pump water level monitoring.

Requires two resistors to form a voltage divider to bring the 5v output of the
HC-SR04 down to 3v3 to be safe as input to the Raspberry Pi GPIO pin.


The python file 'distance' does the measurement by controlling the distance
sensor and then timing the response.  Since this is done on the Raspberry Pi
Linux (not exactly a real-time operating system), there are occasiional glitches in the measurements.  Those are (hopefully) filtered out.

Since this is intended for measuring sump pump water levels, the ReferenceDistance is a critical parameter.  It is the measured length of the PVC pipe from the
sensor to the other end.

This also calculates the intervals between pump runs by looking for rapid changes in water level.

Sensor connections:

The program uses the GPIO.BOARD option, meaning that the numbers correspond to
the pins on the Raspberry Pi P1 connector.  The correspondence between P1
connector and the Broadcom GPIO values can be found in a pinout diagram such
as http://www.raspberrypi-spy.co.uk/2012/06/simple-guide-to-the-rpi-gpio-header-and-pins/

(I used the Adafruit Prototyping Plate http://www.adafruit.com/products/801 which has screw numbers corresponding to the Broadcom GPIO numbers)


<pre>
  HC-SR04
 +---------+
 |         |
 |         |
 |     Vcc |--- +5v
 |         |
 |    Trig |--- P1.12 (GPIO18)
 |         |
 |    Echo |--------------------+
 |         |                    |
 |     Gnd |----+               \ 
 |         |    |               / 470 ohm
 +---------+    |               \
                |               /
               Gnd              |
                                +---------------> P1.16 (GPIO23)
                                |
                                \
                                /
                                \ 1k ohm
                                /
                                |
                               Gnd
</pre>
