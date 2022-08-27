---
title: "Combining the two circuits"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 



::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Combining the two circuits
- Combine the code for the two circuits

::::::::::::::::::::::::::::::::::::::::::::::::

## Combine the two circuits on one breadboard

### ToDo

Explain how to combine the two circuits according to the image below


![Circuit with the DHT22 temperature sensor and and LDR light for measuring light intensity](fig/DHT22_LDR.png)


## Combine the code for the two circuits


```c
#include <DHT.h>
#include <WiFi.h>
#include <PubSubClient.h>

#define DHT_SENSOR_TYPE DHT22
#define DHT_SENSOR_PIN 32 // ESP32 pin connected to DHT sensor
#define LIGHT_SENSOR_PIN 36 // ESP32 pin GIOP36 (ADC0)

const int readingdelay = 3000;


DHT dht_sensor(DHT_SENSOR_PIN, DHT_SENSOR_TYPE);

void setup() {
  Serial.begin(9600); // initialize serial
  Serial.println(F("Starting ..."));
  dht_sensor.begin(); // initialize the DHT sensor
}

void loop() {

  float temperature = dht_sensor.readTemperature();   // read temperature in Celsius
  int lightlevel = analogRead(LIGHT_SENSOR_PIN);     // read light level


  if (isnan(temperature)) {

    Serial.println("Failed to read from DHT sensor!");

  } else {
    /* When reading the temperature and light levels, we get the values
       as numbers, but when we send it to the MQTT broker the values
       have to be converted to strings. We therefore have to declare
       variables of type char into which the number values can be transferred
    */
    char tempString[8];
    char lightString[8];

    // Print the temperature to the monitor
    Serial.print("Temperature: ");
    Serial.println(temperature);

    // Print the light level to the monitor
    Serial.print("Light: ");
    Serial.println(lightlevel);

    // wait a 3 seconds between readings
    delay(readingdelay);
  }
}


```

:::: Discussion

Explain the code

::::

::::::::::::::::::::::::::::::::::::: keypoints 

- Keypoint

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
