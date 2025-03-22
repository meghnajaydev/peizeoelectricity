# peizeoelectricity
Electricity generation from peizeoelectric sensor
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

#define PIEZO_PIN 34   
#define LOAD_RESISTANCE 1000.0  
#define PRESSURE_THRESHOLD 50  

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {
    Serial.begin(115200);

    Wire.begin(21, 22);

    lcd.init();
    lcd.backlight();  
    lcd.setCursor(0, 0);
    lcd.print("Piezo Sensor Init");
    delay(2000);
    lcd.clear();
}

void loop() {
    int sensorValue = analogRead(PIEZO_PIN);
    float voltage = (sensorValue * 3.3) / 4095.0;  
    float power = (voltage * voltage) / LOAD_RESISTANCE;

    lcd.setCursor(0, 0);
    if (sensorValue > PRESSURE_THRESHOLD) {
        lcd.backlight();
        lcd.print("V: "); lcd.print(voltage, 2); lcd.print("V  ");  
        lcd.setCursor(0, 1);
        lcd.print("P: "); lcd.print(power, 6); lcd.print("W  ");  
    } else {
        lcd.backlight();
        lcd.print("No Pressure!   ");  
        lcd.setCursor(0, 1);
        lcd.print("Waiting...     ");
    }

    Serial.print("Voltage: "); Serial.print(voltage, 2);
    Serial.print(" V, Power: "); Serial.print(power, 6);
    Serial.println(" W");

    delay(500);  
}
