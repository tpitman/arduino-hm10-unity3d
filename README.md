# arduino-hm10-unity3d
Example project Unity example bridging Unity with Bluetooth Low Energy to HM10 and Arduino

# Introduction

The most common request that I get about the plugin I wrote for Unity3D is whether it will work with the Arduino and an HC05 or HC06 serial bluetooth module.

The HC05 and HC06 are bluetooth classic modules. The plugin I created for Unity only works with Bluetooth Low Energy or BLE.

There are 2 other modules that I know of that are serial and work with the Arduino and my plugin. They are the HC08 and HM10.

If you are new to Bluetooth and/or Bluetooth Low Energy specifically, you might find this article helpful. It explains some of the basics about Bluetooth Low Energy. If you continue with this example it is assumed that you know at least these basics:

https://learn.adafruit.com/introduction-to-bluetooth-low-energy

This project requires Unity3D (www.unity3d.com) and a plugin call Bluetooth LE for iOS, tvOS and Android:

https://assetstore.unity.com/packages/tools/network/bluetooth-le-for-ios-tvos-and-android-26661

This plugin is not part of this GitHub repo because it is sold on the Unity Asset Store. This repo will not build without getting that plugin and following the steps outlined in this readme file.

You should be able to use the free community edition of Unity. Please check the requirements for it before you use it. There are also licensing restrictions based on your company's size and revenue.

The plugin for BLE is licensed for a single developer. I am the author of this plugin and am happy to answer any questions about it.

# What Does It Do?

In this example you can enter some text and hit a button and the text will be transmitted to the HM10 over bluetooth which then sends the text over serial to the Arduino. The Arduino sketch then sends that same text back to the HM10 over serial which transmits it back to the Unity app over bluetooth.

There is another button in the Unity app that when pressed sends a single byte (0x01) to the HM10 over bluetooth. The HM10 sends that over serial to the Arduino. In this case when the Arduino receives this single byte it toggles the LED that is on the Arduino Uno board connected to pin 13.

# Bluetooth Low Energy Module

There are many companies that make versions of this module. For this example I went on Amazon and purchased one from a company called DSD Technologies. Here is a link on Amazon to purchase this module:

https://www.amazon.com/gp/product/B06WGZB2N4/ref=oh_aui_detailpage_o06_s00?ie=UTF8&psc=1

I like this particular module because it comes with a 4 pin header and a plastic case installed.

There are many other similar modules and they all should work as long as they are BLE and use the same service and characteristic UUID values.

Here is a link to an HC08 you can purchase on Amazon. This one has 2 more pins for enable and status:

https://www.amazon.com/DSD-TECH-SH-HC-08-Transceiver-Compatible/dp/B01N4P7T0H

The wiring I did in this example post is for the HM10 specifically, but the Arduino sketch and Unity example should work with either one.

If you use a different brand of HC08 or HM10 you will have to change the Device Name property in the Unity Inspector. Here is a screen shot of where to make this change:

![alt text](http://www.shatalmic.com/home/arduino-hm10-test/ChangingDeviceName.png?attredirects=0)

Notice that you click on Main Camera as shown by the yellow circle in the upper left corner of the image. Then find the Arduino HM10 Test script and change the Device Name as show by the yellow circle in the lower right of the image. The name must match whatever the device advertises as its name.

# Arduino Uno rev 3

For the Arduino I chose the Arduino Uno rev 3. As long as your Arduino has a serial port the sketch example should work. You may have to modify the wiring.

Here is a picture of the Arduino, HM10 and wiring harness I created:

![alt text](http://www.shatalmic.com/home/arduino-hm10-test/Parts.jpg?attredirects=0)

# Wiring Harness

To begin with you need to connect the HM10 to the Arduino. I decided to use some simple single row headers and wire one directly into the RX/TX header pins of the Arduino and then run wires to the other side where the +3.3v and Ground are located. This allows the HM10 to be fairly ridged.

Here is a picture of the HM10 plugged into the wiring harness:

![alt text](http://www.shatalmic.com/home/arduino-hm10-test/HM10-WiringHarness.jpg?attredirects=0)

Note that the 2 pins that extend are the serial lines. When you plug the wiring harness into the HM10 make sure you don't get it backwards.

The red and black power wires are just soldered to the other 2 pins at an angle so that they don't touch each other.

Here is a picture of everything put together from the power connector side:

![alt text](http://www.shatalmic.com/home/arduino-hm10-test/PowerConnectorSide.jpg?attredirects=0)

Note that the red power wire is connected to 3.3v. I think the HM10 could handle 5v, but didn't want to try it. I recommend just connecting to 3.3v.

Here is a picture from the serial connector side:

![alt text](http://www.shatalmic.com/home/arduino-hm10-test/SerialConnectorSide.jpg?attredirects=0)

If you look closely at that image you can see inside the plastic case of the HM10 that the TX line goes to the RX line on the Arduino and the RX line of the HM10 goes to the TX line of the Arduino.

At this point if you plug the Arduino into USB or power the HM10 should also turn on and the red light on it should start blinking. If this does not happen you will want to check the wiring.

# Arduino Sketch

The sketch that runs on the Arduino is very simple. It basically sets up the serial port and the LED output.

NOTE: On the Arduino Uno there is only one serial port and it is shared with the USB for programming and monitoring. This means that you must disconnect the HM10 when you want to program the Arduino and I don't think you can debug while it is plugged in. Someone with more Arduino experience can probably give some advise here. There are other Arduino models that have multiple serial ports.

Here is a link to information about Arduino and serial: https://www.arduino.cc/reference/en/language/functions/communication/serial/

There is also a serial library that can be used to turn any of the digital IO pins into serial pins.

Here is a link to that library: https://www.arduino.cc/en/Reference/softwareSerial

The code for the sketch that is running on the Arduino is in the ArduinoHM10Test.ino file in this repo.

This code is very verbose. I did this on purpose so that those new to Arduino could follow it.

# Unity Project

To use the Unity project you will need to start with a new project. I would make it a 2D project because the example includes a simple Unity UI based scene.

If you create a new Unity 2D project and then import your licensed copy of the Bluetooth LE for iOS, tvOS and Android into the project you should then be able to download the following source files and save them in the project. I recommend creating a folder under the Example folder like this:

![alt text](http://www.shatalmic.com/home/arduino-hm10-test/FolderStructure.png?attredirects=0)

It shouldn't matter where the files are as long as they have the same names. Notice there is a Unity scene file and the Unity script file.

Those files are in this repo.

Once you have imported the plugin and placed those files in the Unity project, you can open the ArduinoHM10Test scene file. The first thing you will want to do is check the references of the script by selecting the Main Camera object in the scene and look at the ArduinoHM10Test script reference.

For some reason when you add files like this including a scene file the references do not save correctly. You will have to fix these problems.

The first thing to do is click on the Main Camera object in the hierarchy and notice that the script shows an error that it can't be loaded. Just find the ArduinoHM10Test script and drag it into the Script Text Input box that says: Missing script.

Then you will have to drag each of the components from the hierarchy to their proper properties in the script. You will have to open up the hierarchy to find these items.

This include:

HM10_Status -> Canvas/Panel/PanelTop/Text HM10 Status (1)
Bluetooth Status -> Canvas/Panel/PanelBottom/Text Bluetooth Status
Panel Middle -> Canvas/Panel/PanelMiddle
Text To Send -> Canvas/Panel/PanelMiddle/InputField/Text

# Arduino HM10 Test Script

This test script gives a pretty basic idea of how to use the plugin.

You will notice that the script implements a state machine. Since all of the operations in Bluetooth are asynchronous, you can't just call a method and then expect that it has occurred when the method returns. All methods take one or more callbacks.

This is why I used a state machine. This allows you to call a method and have it return right away and then when the callback gets called you can set a new state in the state machine.

The Update loop method processes the current state after the _timeout has expired. This allows you to also introduce time delays that are needed by many of the bluetooth operations.

I have heard of users also using CoRoutines and async / await, but I have not implemented that in any of my examples because this plugin has been available for a long time and I wanted to keep it as backwards compatible as possible.

The main pattern used in this example is to do the following in each state in turn:

1. Scan for any devices with the DSD TECH advertised name
2. Connect to the first one it finds
3. Subscribe to the HM10 characteristic that sends serial data to the bluetooth

After that the script simple waits for input from the subscribe callback or user input in the app.

You will notice in the script a couple of important things:

1. The string values for the UUIDs at the top of the script are 16 bit UUID values. As you already know or read in the article mentioned at the top of this post, 16 bit UUID values must be part of the full default 128 bit UUID that the Bluetooth Special Interest Group (SIG) defined.

The HM10 uses these types of addresses.

For an example of how to use custom UUID values you will find a FullUUID method in this example. Notice how it takes the SIG defined 128 bit address an inserts the 16 bit address into it. You can use this same method by replacing the standard UUID in that method to your custom one.

I also created some helper methods for comparing UUID values and sending a byte or a string of bytes.

# Building the Target

You will need to set the project build target to either iOS or Android. The plugin only works with those platforms. It does not work in the Editor or on any other target.

If you are using Android 6+ you will want to check the documentation for a line that needs to be added to the android manifest xml file. This allows bluetooth to run on Android.

If you are running the example on iOS you will need to choose a signing entity or set up the signing for developer signing.

# Example Running on iOS

I have built for iOS an am running it on my iPhone X. Here is a screen capture movie of it:

https://drive.google.com/open?id=1gjNOvFwXE2Bm9pB_oHcxuJndSMEPI-WU
