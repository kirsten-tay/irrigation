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



void setup() {
	
  dht.begin();
  lcd.begin();
  lcd.backlight();
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Auto Irrigation");
  lcd.setCursor(0,1);
  lcd.print("System");
  delay(5000);

   pinMode(moist_sensor,INPUT);
   pinMode(level_sensor,INPUT);
   pinMode(light_sensor,INPUT);

   pinMode(0,OUTPUT); //5v Relay
   pinMode(2,OUTPUT); //R on LED
   pinMode(3,OUTPUT); //G on LED
   pinMode(4,OUTPUT); //B on LED
}

void loop() {
   lcd.clear();

   float temp = dht.readTemperature(); //Read Temperature as Celsuis
   
   int moisture = analogRead(moist_sensor);   //Read Moisture Sensor
   int water_level = analogRead(level_sensor);   //Read Water Level Sensor 
   int light = analogRead(light_sensor);   //Read Light Sensor
 
   digitalWrite(0,LOW);
   digitalWrite(2,LOW);
   digitalWrite(3,LOW);
   digitalWrite(4,LOW);
   
  
   
  lcd.setCursor(0,0);
  lcd.print("MOIST LVL:");
  lcd.setCursor(10,0);
  lcd.print(moisture);
  
  if (moisture < 400) // for Wet Soil
    { 
      lcd.setCursor(0,1);
      lcd.print("Wet SOIL");
      digitalWrite(0,LOW);
      }
      else if (moisture >= 400) // for Dry Soil
        {
        lcd.setCursor(0,1);
        lcd.print("Dry SOIL");
        if (water_level > 500)
            {
              if (temp < 30)
                  {
                    digitalWrite(0,HIGH); //irrigate     
                  } else if (temp > 30)
                           {
                            lcd.clear();
                            lcd.setCursor(0,0);
                            lcd.print("Temp is High");
                            delay(3000);
                            }
             } else if(water_level < 500)
                      {
                        lcd.clear();
                        lcd.setCursor(0,0);
                        lcd.print("Water is Low");
                        delay(3000);
                       }
        }

    if (digitalRead(0) == HIGH) //status of system
        {
          digitalWrite(3, HIGH);
        }
          else {
                 digitalWrite(2, HIGH);        
               }          
  delay(3000);
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("Air Temp:");
  lcd.setCursor(9,0);
  lcd.print(temp);
  lcd.setCursor(0,1);
  lcd.print("Water Level:");
  lcd.setCursor(12,1);
  lcd.print(water_level);
     
 delay(5000);       
}
 
			    
			      
