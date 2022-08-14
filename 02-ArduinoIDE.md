---
title: "The Arduino IDE"
teaching: 10
exercises: 2
---

:::::::::::::::::::::::::::::::::::::: questions 

- What is an IDE?
- What is the purpose of the Arduino IDE?
- Which microcontrollers can be programmed with the Arduino IDE?
- How is the Arduino IDE used to program a microcontroller?
- How are microcontroller boards connected to the computer?
- How does the Arduino IDE connect to the microcontroller?
::::::::::::::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: objectives

- Explain what an IDE is.
- Explain what the Arduino IDE is used for.
- Identify microcontrollers that can be programmed with the Arduino IDE.
- Run the Arduino IDE.
- Connect the microcontroller board to the computer.
- Connect the microcontroller to the Arduino IDE.

::::::::::::::::::::::::::::::::::::::::::::::::

## Running the Arduino IDE

For this episode we have to assume that you were able to successfully install the Arduino IDE and that you are able to start the software from the start menu or the desktop of your computer. When you run the software you will first see the splash window:


![Arduino Splash Window](fig/ArduinoIDE_startup.png)
<figcaption align = "center">
 <b>The Arduino splash screen which is shown when the program starts</b>
 </figcaption>
 
 
After a while the IDE will open. It is actually quite a simple interface. At the top you should see the File, Edit, Sketch, Tools and Help menu items. Just below the menu there are five icons to the left and one on the far right. Below the icons you should see a tab with a filename and a white editing area. Below the editor is a black terminal:

![Arduino IDE](fig/ArduinoIDE.png)
<figcaption align = "center">
 <b>The Arduino IDE main window</b>
 </figcaption>
 
 
The white area is where you will be typing the code that will be uploaded to the microcontroller. In the black area you will see messages about the *compilation* of the code and the uploading process. Right at the bottom is a status bar where you will eventually be able to see whether the IDE could connect to your motorcontroller.

The first time you run the IDE the status bar will show the string `Arduino Uno on /dev/ttyAM0` and in the editor you will see some skeleton code:

```c
void setup() {
  // put your setup code here, to run once:

}

void loop() {
  // put your main code here, to run repeatedly:

}
```

## Configuring the IDE for use with boards other than an Arduino Uno

If you will be using an Arduino or an Arduino compatible device, you don't have to complete the steps in this section and you can skip to the secion on **compiling**.

By default the IDE is configured to program an Arduino Uno but we will be using an ESP32 WROOM 32 board so we first need to configure the IDE before we can start coding. Start by clicking on the `File` menu item and then on `Preferences`. Towards the bottom of the Preferences window you should find a text area with the label `Additional Boards Manager URLs:`:

![Preferences Window](fig/preferences.png)

To find the appropriate URL you need to open a web browser and navigate to: `https://github.com/arduino/Arduino/wiki/Unofficial-list-of-3rd-party-boards-support-urls`. For the ESP32-WROOM-32 you have to find the line that reads:

**Espressif ESP32:** https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json

Copy and paste the address into the text area of the preferences window:

![Enter additional boards manager URLs](fig/addURL.png)

Click on `OK`

What we have just done is to tell the IDE where to find more information about the ESP32 boards. Because the ESP32 series use a different microcontrollers to the Arduino Uno our programs have to be *compiled* differently. By default all the Arduino boards are immediately available for selection in the IDE but for other boards we have to give the IDE a place where it can find the information for *compiling* the code for those other boards.

## What is "compiling"?
We have used the term "compiling" a few times now. But what is that? You'll notice, that once we start programming, the programming language uses English words. But, computers do not understand English. Computers understand only 0 and 1. A compiler takes the human readable programming language that we entered into the editor and "translates" it into instructions consisting of 0 and 1 that makes complete sense to the microcontroller. Although all microcontrollers understand 0 and 1, the instructions for different microcontrollers differ. The 0s and 1s are like sounds are to us. But different human languages combine the sounds differently. Similarly the 0s and 1s need to be arranged differently for microcontrollers designed by different manufacturers or even different models of microcontrollers manufactured by the same manufacturer. For this reason we have to tell the IDE which microcontroller board we are using so that the compiler can translate our English code into the correct `dialect` of microcontroller language.

## Selecting the correct board


## Connecting the board to the computer


## Selecting the correct serial port in the IDE


We should now be connected.






::::::::::::::::::::::::::::::::::::: keypoints 

- Explain what an IDE is.
- Explain what the Arduino IDE is used for.
- Identify microcontrollers that can be programmed with the Arduino IDE.
- Run the Arduino IDE.
- Connect the microcontroller board to the computer.
- Connect the microcontroller to the Arduino IDE.

::::::::::::::::::::::::::::::::::::::::::::::::

[r-markdown]: https://rmarkdown.rstudio.com/
