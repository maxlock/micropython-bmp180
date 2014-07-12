micropython-bmp180
==================

Module bmp180
-----------------
bmp180 is a micropython module for the Bosch BMP180 sensor. It measures
temperature as well as pressure, with a high enough resolution to calculate
altitude.  
Breakoutboard: http://www.adafruit.com/products/1603  
data-sheet: http://ae-bst.resource.bosch.com/media/products/dokumente/bmp180/BST-BMP180-DS000-09.pdf

### Wiring the sensor to the pyboard

| pyboard| bmp180 |
|:------:|:------:|
| VIN    | VIN    |
| 3V3    | 3Vo    |
| GND    | GND    |
| SCL    | SCL    |
| SDA    | SDA    |

### Quickstart

Example:
```python
from bmp180 import BMP180
bmp180 = BMP180()
bmp180.oversample_sett = 2
temp = bmp180.get_temperature()
baseline = bmp180.baseline(1000)
p = bmp180.get_pressure()
altitude = bmp180.altitude_above_ref()
print(temp, baseline, p, altitude)
```
Note that in this example the altitude will be around zero, because the pressure measurment is done immediatly after the baseline setting.

Classes
-------
``BMP180``  
Module for the BMP180 pressure sensor.  
![UML diagramm](https://raw.githubusercontent.com/turbinenreiter/micropython-bmp180/master/classes_BMP180.png "UML diagramm")


Methods
--------------


``altitude_above_ref(self)``  
Calculates and returns the altitude relative to a reference pressure.  

``baseline(self, dt=None)``  
Measures the pressure for a given time and returns the mean of the  
measurements.

``gauge_pressure(self)``  
Starts the pressure measurement and returns the time it will be  
finished.

``gauge_temperature(self)``  
Starts the temperature measurement and returns the time it will be  
finished.

``get_pressure(self, t_ready)``  
Waits until the pressure measurement is finished, then returns the  
uncompensated temperature.

``get_temperature(self, t_ready)``  
Waits until the temperature measurement is finished, then returns the  
uncompensated temperature.

Instance variables
------------------
``chip_id``  
ID of chip is hardcoded on the sensor.

``oversample_sett``  
Sets the accuracy.
* 0 lowest accuracy, fastest
* 1
* 2
* 3 highest accuracy, slowest

``baseline``  
Pressure at Main Sea Level. The default is 101325 Pa, but you can use your local QNH in Pa.  
When evoked, the method ``baseline(dt)`` sets this variable to the local pressure.
To get different altitudes, use this as baselines:

| altitude |       baseline |  
|:--------:|:--------------:|  
| absolute |       baseline |  
| true     |        QNH*100 |  
| pressure | 101325 or None |  

