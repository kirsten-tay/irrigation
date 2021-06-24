# irrigation
a working irrigation system using Arduino uno

/*
ARDUINO PINS
Analog Pins
A0 - Moisture Sensor
A1 - Water level Sensor
A4 - LCD1
A5 - LCD2

Digital Pins
0 - 5V Relay -> Pump
2 - R on LED
3 - G on LED
4 - B on LED
5 - Temperature Sensor
*/

#include <DHT.h>
#include <Wire.h> 
#include <LiquidCrystal_I2C.h> //LCD Library
LiquidCrystal_I2C lcd(0x27, 16, 2);

int moist_sensor = A0;
int level_sensor = A1;
int light_sensor = A3;

#define DHTPIN 5
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE); //Temperature Sensor Config


