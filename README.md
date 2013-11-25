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

On the Raspberry Pi, I copied this entire tree into /home/pi/pi-distance.  

Then run (as root):  
cp init.d/distance /etc/init.d/  
cp cron.d/checkup /etc/cron.d/  

If you decide to move things around, you will need to fix up the paths in the script files.

The init script is based on: http://www.debian-administration.org/articles/28

init.d/distance:  init script to be placed into the /etc/init.d/ directory
make sure that it is executable and owned by root:  
  -rwxr-xr-x 1 root root 470 Feb  1 12:09 distance  

Then run (as root):  
  update-rc.d distance defaults  
to create the needed symlinks so that this will all start on boot  

if you decide to remove the script from the startup sequence run:  
  update-rc.d -f distance remove  

The checkup script pings the default gateway every 10 minutes and reboots the Pi if it doesn't get a response.  This is to recover from issues with the USB WiFi adapter.

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
