# co2ampel

![](https://git.unhb.de/smash/co2ampel/raw/branch/master/IMG_20200831_221038.jpg)

## idea
higher concentrations of CO2 inside will make you sleepy and have an impact on your wellbeing. during the current pandemic higher CO2 concentrations inside are also a pretty good indication for higher levels of aerosol and as such a higher risk for infection. the 'CO2 Ampel' (CO2 traffic light) will give you a visual representation of the current situation and remind you to open windows to keep you happy and healthy

## parts list

| description          | part        |
| -------------------- | ----------- |
| Infrared CO2 Sensor  | mhz-19      |
| Active piezzo buzzer | KY-012      |
| OLED Display         | SSD1306     |
| LED-Ring             | WS2812      |

## build

just solder everything according to the connection schema.

### schema

| ESP32 PIN     | peripherals       | note                           |
| ------------- | ----------------- | ------------------------------ |
| VIN           | mhz-19 (VCC)      |                                |
| GND           | mhz-19 (GND)      |                                |
| GND           | KY-012 (GND)      | passive buzzer will do as well |
| D12           | KY-012 (Signal/+) |                                |
| 3V3           | SSD1306 (VCC)     |                                |
| 3V3           | WS2812 (VCC)      |                                |
| GND           | WS2812 (GND)      |                                |
| GND           | SSD1306 (GND)     |                                |
| D4            | WS2812 (DI)       |                                |
| RX2 / D16     | mhz-19 (TX)       |                                | 
| TX2 / D17     | mhz-19 (RX)       |                                |
| I2C SDA / D21 | SSD1306 (SDA)     |                                |
| I2C SCL / D22 | SSD1306 (SCL)     |                                |

### ws2812
if your ws2812 don't work on 3.3V power (most will do) you can try to use 5V instead. there are plenty different ws2812 builds so ymmv... if the leds don't work with 5V try to add a pullup resistor to your data line and a diode towards D4 to raise your high level while protecting your GPIO (https://forum.arduino.cc/index.php?topic=578735.msg3941756#msg3941756)
looks like this:
![](https://git.unhb.de/smash/co2ampel/raw/branch/master/levelshifter.png)

either way it might be a good idea to have a capacitor between VCC and GND. my prototype worked without any of these, but from time to time some LEDs just randomly turn on...





## readings for inside rooms


| CO2 measure | light  | buzzer | meaning                                                                           |
| ----------- | ------ | ------ | --------------------------------------------------------------------------------- |
| 400 ppm     | none   | no     | more or less the baseline of the sensor hardware and means 'fresh air quality'    |
| 400-800ppm  | none   | no     | safe zone and can be considered good air quality                                  |
| 800-1500ppm | yellow | no     | warning zone (consider to open windows) and is between medium and bad air quality |
| > 1500ppm   | red    | yes       | bad air quality, ppl in the room will be affected in a negative way               |

 


## todo
 * cooldown time for buzzer (alarm only once every n minutes) -> lambda/interval/on_time?
 * add openings for mh-z19 to the case
 * adjust light tunnel to use frosted acrylic glass
 * add a silent switch for meeting rooms and such (no buzzer, but flash at a regular interval to attract attention)