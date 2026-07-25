# Temperaturanzeige auf der Arduino-LED-Matrix

> Dieses Projekt misst die Umgebungstemperatur mit einem analogen Temperatursensor und zeigt den ermittelten Wert auf der integrierten LED-Matrix des Arduino UNO R4 WiFi an.

## Funktionsweise

Der Temperatursensor ist mit dem analogen Eingang `A0` verbunden.

Der Arduino liest den Sensorwert ein und rechnet ihn zuerst in eine Spannung um:

## Code

```cpp
#include "ArduinoGraphics.h"
#include "Arduino_LED_Matrix.h"

ArduinoLEDMatrix matrix;

const int sensorPin = A0;

void setup() 
{
  // put your setup code here, to run once:
  Serial.begin(9600);

  matrix.begin();
}

void loop() 
{
  // put your main code here, to run repeatedly:
  int sensorValue = analogRead(sensorPin);

  float voltage = (sensorValue / 1024.0) * 5.0;

  float temperature = (voltage - 0.5) * 100;

  showTemperature(temperature);

  delay(500);
}

void showTemperature(float temperature)
{
    // Temperatur auf eine ganze Zahl runden
    int roundedTemperature = round(temperature);

    // Beispiel: "23C"
    char temperatureText[8];
    snprintf(
        temperatureText,
        sizeof(temperatureText),
        "%dC",
        roundedTemperature
    );

    matrix.beginDraw();

    // Vorherige Anzeige löschen
    matrix.background(0x000000);
    matrix.stroke(0xFFFFFF);

    // Der kleine Font passt besser auf die 12x8-Matrix
    matrix.textFont(Font_4x6);
    matrix.beginText(0, 1, 0xFFFFFF);
    matrix.print(temperatureText);
    matrix.endText();

    matrix.endDraw();
}


```

## Schaltplan:

![Schaltplan](./Assets/Schaltplan.svg)

## Beispiel:

![Beispiel](./Assets/Arduino_Buch_Projekt_3.gif)

