# MKR1010-WiFi-cloud-temperature-datalogger

Instructions:
Download the Arduino IDE and connect the usb from the PC to the microusb slot on the MKR1010 board 
Go to Tools->Board manager->and select Arduino SAMD boards
Once installed, select MKR1010 as the board
In the tools, library manager. Install WiFiNINA library.
Next, in the Arduino IDE go to files->Examples->WiFiNINA->Tools->FirmwareUpdater.
Next ensure the latest firmaware is installed by going to Tools-> Wifi101 /WiFiNINA Firmware Updater. Select the latest firmware and Upload the Sketch to the board
To finish setting up, compile and upload the sketch SerialNINAPassthrough. You can find this sketch by going to File -> Examples -> WiFiNINA -> Tools -> SerialNINAPassthrough 

My simple datalogger uses a TMP 36GZ temperature sensor, the left pin (from th flat face) connected to positive, the center pin connected to A5 on the MKR1010 board and the right pin to ground.
An LED is also added that can be turned on/off via the cloud just for testing things work ok but could be a useful function on its own if required. The short leg of the LED is connected to ground via a 220Ohm resistor and the long leg connected to digital pin 5 on the MKR1010 board. 
Now create a cloud account at https://create.arduino.cc/iot
You'll need to create a 'thing' which is your MKR1010 device.
Next set variables. Here we add a variable, I named it temp and choose floating point number. Final since it is just to send temperature reading it only is required to be read only and update is set to 'on change'.
Create anther variable, named LED and this time it's a bool since it will be either on or off, and this time read and write is required and again update is set to on change.
Next your network setting needs adding by scrolling down the page to network and adding your WiFi name and password.
A sketch will be automatically generated with your variables and settings but a bit of code needs adding to get it to work.

Under #include "thingProperties.h"
add these two lines of code:

int myled = 5;
const int sensorpin=A5;

change the void loop to:
void loop() {
  ArduinoCloud.update();
  // Your code here 
  delay(2000);
  
  temp=(((analogRead(sensorpin)*0.00322581)-0.5)*100);
}

and the void on LED change to:
void onLedChange()  {
  // Add your code here to act upon Led change
  Serial.println(led);
  if (led){
    digitalWrite(myled,HIGH);
    
  }else {
    digitalWrite(myled,LOW);
  }
}

Then verify and upload to the board.

To set up your IOT dashboard go to dashboard then click the edit button to add wigets. This can be however you prefer but I added a switch, link variable- thing- LED
I also added a Value button, linked with temp and finally added a chart and linked to the temp variable.
If you like, you can download the arduino IOT app to your phone, sign in and control it from your phone!
Full code is attached along with a demo video
