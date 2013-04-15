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
(the pin numbers correspond to the markings on the Adafruit Prototyping Plate http://www.adafruit.com/products/801 )

  HC-SR04
 +---------+
 |         |
 |         |
 |     Vcc |--- +5v
 |         |
 |    Trig |--- pin 18 (GPIO 12)
 |         |
 |    Echo |--------------------+
 |         |                    |
 |     Gnd |----+               \ 
 |         |    |               / 470 ohm
 +---------+    |               \
                |               /
               Gnd              |
                                +---------------> pin 23 (GPIO 16)
                                |
                                \
                                /
                                \ 1k ohm
                                /
                                |
                               Gnd
