---
title: "MQTT"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is MQTT?
- How does MQTT work?
- What is an MQTT publisher?
- What is an MQTT broker?
- What is an MQTT subscriber?
- What is a client?
- What is a server?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Explain how MQTT works

::::::::::::::::::::::::::::::::::::::::::::::::

## Add MQTT

[Message Queuing Telemetry Transport](https://en.wikipedia.org/wiki/MQTT)

MQTT (originally an initialism of MQ Telemetry Transport[a]) is a lightweight, publish-subscribe, machine to machine network protocol. It is designed for connections with remote locations that have devices with resource constraints or limited network bandwidth. 

![Circuit with the DHT22 temperature sensor and and LDR light for measuring light intensity](fig/DHT22_KY-018_MQTT.png)

```c
#include <DHT.h>
#include <WiFi.h>
#include <PubSubClient.h>

#define DHT_SENSOR_TYPE DHT22
#define DHT_SENSOR_PIN 32 // ESP32 pin connected to DHT sensor
#define LIGHT_SENSOR_PIN 36 // ESP32 pin GIOP36 (ADC0)

const int readingdelay = 3000;
const char* ssid = "raspi-webgui";
const char* password = "w0rksh0p";
const char* mqtt_server = "10.3.141.1";

DHT dht_sensor(DHT_SENSOR_PIN, DHT_SENSOR_TYPE);
WiFiClient espClient;
PubSubClient client(espClient);

String topic_temperature = "/dht22/temperature";
String topic_light = "/ldr/light/";

void setup() {
  Serial.begin(9600); // initialize serial
  Serial.println(F("Starting ..."));
  dht_sensor.begin(); // initialize the DHT sensor
  setup_wifi();
  client.setServer(mqtt_server, 1883);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();

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

    /**
     * Transfer the number values into strings
     */
    dtostrf(temperature, 1, 2, tempString);
    dtostrf(lightlevel, 1, 2, lightString);

    // Print the temperature to the monitor
    Serial.print("Temperature: ");
    Serial.println(temperature);
    // Publish the temperature to the MQTT broker
    publishMQTT(topic_temperature, tempString);

    // Print the light level to the monitor
    Serial.print("Light: ");
    Serial.println(lightlevel);
    // Publish the light level to the MQTT broker
    publishMQTT(topic_light, lightString);

    // wait a 3 seconds between readings
    delay(readingdelay);
  }
}

void setup_wifi() {
  delay(10);
  // We start by connecting to a WiFi network
  Serial.println();
  Serial.print("Connecting to ");
  Serial.println(ssid);

  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.println("WiFi connected");
  Serial.println("IP address: ");
  Serial.println(WiFi.localIP());
}

void callback(char* topic, byte * message, unsigned int length) {
  Serial.print("Message arrived on topic: ");
  Serial.print(topic);
  Serial.print(". Message: ");
  String messageTemp;

  for (int i = 0; i < length; i++) {
    Serial.print((char)message[i]);
    messageTemp += (char)message[i];
  }
  Serial.println();
}

void reconnect() {
  // Loop until we're reconnected
  while (!client.connected()) {
    // Create a random client ID
    String clientId = "ESP32Client-";
    clientId += String(random(0xffff), HEX);
    Serial.print(clientId + " attempting MQTT connection...");
    // Attempt to connect
    if (client.connect(clientId.c_str(), "jannetta", "f0r3v3rl1n8x")) {
      Serial.println("connected");
    } else {
      Serial.print("failed, rc=");
      Serial.print(client.state());
      Serial.println(" try again in 5 seconds");
      // Wait 5 seconds before retrying
      delay(500);
    }
  }
}

void publishMQTT(String topicString, char payload[8]) {
  unsigned int length = topicString.length() + 1;
  char topic[length];
  topicString.toCharArray(topic, length);
  Serial.print(topic);
  Serial.print(": ");
  Serial.println(payload);
  client.publish(topic, payload, true);
}

String byteArrayToString(byte * byteArray, unsigned int length) {
  String messageTemp;
  for (int i = 0; i < length; i++) {
    messageTemp += (char)byteArray[i];
  }
  return messageTemp;
}
```

::::::::::::::::::::::::::::::::::::: keypoints 

- Keypoint

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
