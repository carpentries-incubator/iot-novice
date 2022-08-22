---
title: "Connecting the first sensor"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- Question

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Objective
::::::::::::::::::::::::::::::::::::::::::::::::

## Connecting the temperature sensor

![Temperature Sensor (DHT22)](fig/DHT22.png)

```c
#include <DHT.h>

#define DHT_SENSOR_TYPE DHT22
#define DHT_SENSOR_PIN 32 // ESP32 pin connected to DHT sensor

const int readingdelay = 3000;

DHT dht_sensor(DHT_SENSOR_PIN, DHT_SENSOR_TYPE);

void setup() {
  Serial.begin(9600); // initialize serial
  Serial.println(F("Starting ..."));
  dht_sensor.begin(); // initialize the DHT sensor
}

void loop() {

  float temperature = dht_sensor.readTemperature();   // read temperature in Celsius


  if (isnan(temperature)) {
    Serial.println("Failed to read from DHT sensor!");
  } else {
    char tempString[8];
    dtostrf(temperature, 1, 2, tempString);
    Serial.print("Temperature: ");
    Serial.println(temperature);
  }

  // wait a 3 seconds between readings
  delay(readingdelay);
}

```

::::::::::::::::::::::::::::::::::::: keypoints 

- Keypoint

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
