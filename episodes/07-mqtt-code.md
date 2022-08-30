---
title: "Using MQTT for the Internet of Things"
teaching: 10
exercises: 2
---
:::: questions 

- What is MQTT?
- How does MQTT work?
- What is an MQTT publisher?
- What is an MQTT broker?
- What is an MQTT subscriber?
- What is a client?
- What is a server?

::::

:::: objectives

- Explain how MQTT works

:::::

## Adding MQTT to our sketch

### What is MQTT?

[Message Queuing Telemetry Transport](https://en.wikipedia.org/wiki/MQTT)

MQTT (originally an initialism of MQ Telemetry Transport[a]) is a lightweight, publish-subscribe, machine to machine network protocol. It is designed for connections with remote locations that have devices with resource constraints or limited network bandwidth. 

There are three components to the MQTT architecture, publishers, subscribers and a broker. Publishers are devices on the Internet of Things that publish messages with a specific topic to a broker. Subscribers subscribe to specific topics on the broker. When a new value for a specific topic is published by a publisher, then all the subscribers to that topic will receive it. 


![MQTT Architecture](fig/mqtt_architecture.png){alt='MQTT Architecture' width="50%"}

:::: challenge

Discuss, with the person next to you the following:

1. What kind of devices we are likely to find on the Internet of things?
2. Who would typically publish information from such devices?
3. Who would typically subscribe to the data?

::::

### Importing the MQTT library into the Arduino IDE

To be able to enable MQTT functionality we need to add yet another library to our Arduino IDE. As before select `Tools` on the menu and then `Manage Libraries`. Enter `PubSubClient` into the text box for searching. Scroll down until you find `PubSubClient by Nick O'Leary` and click the Install button:

![Installing the PubSubClient library for MQTT messaging](fig/PubSubClient.png)

There are three pieces of information that we will need before we enter the code for sending and messages via MQTT:

1. The `ssid` of your Internet access point
2. The password for the access point
3. The IP address or hostname of your MQTT broker

There are several free MQTT servers available on the Internet. Alternatively one can set up one's own server running an MQTT broker. In the diagrams below two alternative setups are shown. In the first case there is an MQTT server on the Internet that devices can publish or subscribe to. In the second case a local MQTT broker is used which has the advantages that all network traffic can be private, it doesn't have to be shared to the Internet. For the purposes of this workshop for instance we might want to set up our own server so that we are not dependent on the Internet.

![An MQTT broker on the Internet](fig/mqtt_internet.png){ alt="An MQTT broker on the Internet" width="50%"}

![An MQTT broker on a private network](fig/mqtt_local_network.png){ alt="An MQTT broker on a private network" width="50%"}


In the sketch below you will have to replace the values assigned to the variables `ssid`, `password` and `mqtt_server`. Your instructor should provide you with the appropriate values which will depend on the MQTT broker you are going to use.

:::: instructor

To avoid the requirement of Internet access for this lesson it is possible to set up and access point and MQTT broker using a Raspberry Pi. It could be done with any available computer, really, but the RPi makes for a simple solution that one can prepare beforehand and keep just for this purpose. It is small and easy to take along to a workshop.

This has only been tested with a RPi 4 with 2GB of RAM. Older RPis should work.

1. Using the Raspberry Pi Imager, select the Raspberry Pi OS Lite version of the operating system.
2. Using the settinngs menu, enable SSH and configure the wireless LAN. The settings are very much determined by your Internet connectivity and network setup so it might be worth looking at documentation on the Internet for your specific circumstances. Make sure you set the Wireless LAN coutry and the locale settings.
3. Write the operating system to a micro-SD card and start up the RPi. You should be able to ssh into the RPi at this point
4. To create an access point we will use the software developed by the open-source project RaspAP (https://raspap.com/)
5. To install RaspAP:
```bash
sudo apt-get update
sudo apt-get full-upgrade
sudo reboot
```
After the reboot log into the RPi again
```bash
curl -sL https://install.raspap.com | bash
```
6. You should now be able to connect to the Access point with the following information:
```
    IP address: 10.3.141.1
    Username: admin
    Password: secret
    DHCP range: 10.3.141.50 — 10.3.141.255
    SSID: raspi-webgui
    Password: ChangeMe
```
7. Using a browser navigate to 10.3.141.1 and log in using as user `admin` with password `secret`.  Change the IP address to `192.168.0.1` and the DHCP range to `192.168.0.50 - 192.168.0.255`. You don't have to do this but if you don't you'll have to adapt the IP address for the MQTT server in the lesson.
8. Reboot the RPi
9. Log into the RPi again. We now have to install Mosquitto which is the MQTT broker and client
10. In the terminal type the following command:
```bash
sudo apt install -y mosquitto mosquitto-clients
```
11. Reboot again just to be sure
12. Your RPi should now be running RaspAP for students to connect to for an Access Point and it should be running the Mosquitto broker.

::::

For now we will continue as if your instructor has set up a local network that includes an access point to which your computer has to connect and an MQTT broker. You might, or might not, be connected to the Internet once you connect to the access point, but that won't matter.



```c
#include <DHT.h>
#include <WiFi.h>
#include <PubSubClient.h>

#define DHT_SENSOR_TYPE DHT22
#define DHT_SENSOR_PIN 32 // ESP32 pin connected to DHT sensor
#define LIGHT_SENSOR_PIN 36 // ESP32 pin GIOP36 (ADC0)

const int readingdelay = 3000;
const char* ssid = "raspi-webgui";
const char* password = "ChangeMe";
const char* mqtt_server = "192.168.0.1";
const String your_name = "/JohnSmith";

DHT dht_sensor(DHT_SENSOR_PIN, DHT_SENSOR_TYPE);
WiFiClient espClient;
PubSubClient client(espClient);

String topic_temperature = your_name + "/dht22/temperature";
String topic_light = your_name + "/ldr/light/";

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

:::::::::::::::::::::::::::: discussion

Explanation of the code

::::::::::::::::::::::::::::

:::: keypoints 

- Keypoint

:::::

[r-markdown]: https://rmarkdown.rstudio.com/
