---
title: "Subscribing to an MQTT topic"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What does one need to get the information published by sensors on the IoT?
- What is meant by "topic" in terms of MQTT?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Subscribing to an MQTT broker
::::::::::::::::::::::::::::::::::::::::::::::::

## Our current status

In the previous episodes we have:

1. Installed the Arduino IDE on our computers
2. Added the URL in the IDE's preferences to detect the configurations for the ESP32 board 
3. Installed the library required for the DHT sensors
4. Connected a DHT11 or DHT22 sensor to an ESP32 microcontroller board
5. Entered the code in a sketch to get temperature readings from the DHT sesions
6. Connected a Light Dependent Resistor to the same ESP32 microcontroller board
7. Entered the code in a sketch to get light level readings from the LDR
8. Entered the code in the sketch to connect to our local network and publish the sensor readings to an MQTT broker

## Subscribing to an MQTT topic

At this point your ESP microcontroller board should have a temperature and a light level sensor on it. It should have connected to your local network and to an MQTT broker and it is publishing the readings to a topic to the MQTT broker.

We now need client software to connect to the MQTT broker and subscribe to the topic to which your sensor readings are published.


::::::::::::::::::::::::::::::::::::: keypoints 

- Subscribing to an MQTT topic

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
