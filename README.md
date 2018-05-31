gg we will learn how to use the EEPROM on Arduino. EEPROM is a very common word in microcontroller. The full meaning of EEPROM is electrically erasable programmable read-only memory. 
Arduino (ATmega328p) EPIMI size 2KB However, if there is a need to conserve more data, then external EEPROM can be used.


At the beginning it is good to say that I am not a student of electronics / electrical engineering. General students Doing so works better with electronics. And the road to travel in digital electronics is not too long. So, writing in the normal way can lead to misunderstandings. If you have received something like this, then let me know if I updated the text.

EPROM is used to permanently store data in MicroController. Because of Arduino's own EEPROM library, it is much easier to use EEPROM in Arduino than other microcontrollers.

Use of EEPROM

You can see or use many of the circuits to control the AC load with remote. Where the power goes again and again the loads are in the previous state.

Immediately explain, think you bought a remote control device that has room fans, lights on off. The fan is running, suddenly the electricity went away, came back after a while. Then the fan continues again. Remember that the device was running before the power was gone, and when the power came, the fan started again ... This work is mainly used by EEPROM. However, this device does not have all the devices in the market. 😛

As soon as the power goes away, the data is stored in the EEPROM as per the requirement so that the device can return to the previous state when the power is in place.

EEPROM library function

There are 4 functions of the EPROM library (according to my knowledge). However, two of these functions are widely used and are highly needed. Two functions are:

 1. EEPROM.write ()
 2. EEPROM.read ()

The EEPROM.write () function is written on the EPROM data. This function's parameter two.

    EEPROM.write (address, value)


The first parameter is address is to define an address here, which is the data that will be written in the address. And the second parameter is to give value to what value it will be stored here. But it is good to remember that the values ​​are stored in bytes. The value of which should be between 0 and 255. Let's look at an example.

    EEPROM.write ('led', 1) 

In the example above, we have define an address named led and its value is 1.

Note: It takes only 3.3 milliseconds to record data on EEPROM and EEPROM Life Cycle is 100,000 That is, data is written / deleted from 1 target email. Therefore, it must be careful about this. 🙂

Now let us know the function of the EEPROM.read () function. I can understand the name of the name. The data is read from EEPROM with this function. The EEPROM.read () function is one of the parameters.

   EEPROM.read (address) 

The parameter has to enter the address from which the function will pull data. If an address is given where the data is not written, then the function returns 255. Example:

  EEPROM.read ('led') 

So how many returns will this data from this (led) address say? Hmm said exactly 1 return. I hope you understand. Let's look at the Practical Project.

Practical Project

We will create a project that can be accessed through Android mobile through Bluetooth. Very simple From the Android phone to the Arduino1 data as an LED, an LED will get in the water and 0 will send the LEDs off. You can also control the AC load using the relay or the tray in the LED's place. Let's write about this later.

Whatever it takes

Arduino UNO R3
HC5 Bluetooth Module
Red LED
220 Ohm Resistor
Some wire
According to the below diagram, give connection between Arduino, LED and Bluetooth modules. Connection properly. Otherwise the Bluetooth module will be lost.


Connection:

Arduino RX (Pin 0) -> Bluetooth Module TX
Arduino TX (Pin 1) -> Bluetooth Module RX
Arduino Pin 13 -> 220 Ohm Resistor -> LED +
Arduino GND -> LED -



Coding


I have written a very simple code. Copy the code below and paste it into Arduino IDE. At the end of the post, download the .ino file from the download section and open Arduino IDE. Download Arduino IDE ( [For windows](https://www.arduino.cc/en/Main/Donate) from here.


 / *
  Contibuter: Skjoy
  Web: http://skjoy.info
  E-mail: joy2012bd@gmail.com
 * /

 // include the EEPROM library code:

 #include <EEPROM.h>

    
     int LED = 13;  
     char input;  
     byte value;
    
     void setup () {  
      
       Serial.begin (9600);  
      
       / * Set LED as output * /
       pinMode (LED, OUTPUT);
     }  
      
     void loop () {  
       if (serial.available ()> 0) {  
        
         / * Catch data from android via bluetooth module * /
         input = Serial.read ();  
        
         if (input == '1') {
           / * If received from 1 android, trun on led * /  
           digitalWrite (LED, HIGH);
           EEPROM.write ('led', 1);
           delay (200);  
         }  
         if (input == '0') {
            / * If received 0 from android, trun off led * /  
           digitalWrite (LED, LOW);
           EEPROM.write ('led', 0);  
           delay (200);  
         }
       }
       else {

        / * Read value from eeprom if serial unavailable * /
        value = EEPROM.read ('led');
        
         if (value == 1) {
            / * If receive 1 from eeprom trun on led * /
           digitalWrite (LED, HIGH);
          }
          if (value == 0) {
             / * If received 1 from eeprom trun off led * /
            digitalWrite (LED, LOW);
          }   
       }  
        
     } 



Code interpreting


Although code has been interpreted in the code of tuman form, then we are trying to give a brief explanation.

EEPROM Library has been added to line 9.

Line 12 on a variable called LED, it has been stored in Arduino 13 pins. Then the pin number 13 in the LED variable with the pinMode function is defined as output 13 pin as output.

If the condition is checked in line no.25, then no data is sent from Android to Bluetooth module.

Checking the number 30 is checking whether the data sent from Android is 1. If it is 1, then do the LED On line (Line No. 32) and store it as the value of the email addressed as 1.

Checking the number 36 is checking whether the data sent from Android is 0. If 0 is, then do the Leading Off (line 38) and Store 0 as the value of the email addressed in the email address.

43. Line is said to be else conditioned if the data does not come from Android, then code inside the else condition work. The codes inside the else condition will start working if you get it on the board.

Writing data from EEPROM in line 46.

Checking in line 48 is the data stored in the ecommerce store. If it is 1 then turn on the LED.

Checking in line 52 is the data that has been stuck in the EPIM 0. If 0 is the LED's off.


I hope to understand. Those who know the basics of programming do not have trouble understanding their code. Keeping in mind the newcomers, trying to give a little explanation. 😛

Now, compile the code and upload it to the board. But before that open the connection between the Arduino and Bluetooth modules. This action must be done before uploading the code every time. Otherwise, you and me will lose the Bluetooth module worth $4. 🙁

Open? Then upload the code to the board. After uploading, disconnect the power supply from the board and give the Arduino Connection to the Bluetooth module according to the above diagram. Now give the power supply to the board. See, there is a red LED on/off in the Bluetooth module.

Now you can search on android phone bluetooth. Get a device named HC-05. Pair with that. If you want to have ppassword 1234 Now download your Android phone [from here](https://www.dropbox.com/s/lfjtxakqpcqmg6s/LED.apk?dl=0) and download your Android phone (a little bit of experts can also work with a Bluetooth terminal).

Open the app then you can get a list of paired Bluetooth devices.


From there, select HC-05. Then your Android phone will be connected to Bluetooth modules. Understand how that has been connected? If connected, the Blue Module's red color LED signal speed will be decrease and the application will be connected to the new screen by showing the connected text. Where you get two buttons named ON and OFF. 😛

Now click on the ON button. If everything is okay, the LED connector with the 13 pin of Arduino will burn. Click on the OFF button to exit the LED.



Check the effectiveness of EEPROM

Now it is time to check the effectiveness of EEPROM. Disconnect the power supply from the Arduino board in the LED flame. Connect again. What are you watching The LED itself is not burning? That means you have succeeded and everything is working perfectly.🙂

Alright, everything is checked in real hardware. It works 100%. 



Download

You have given the download links together. Hopefully there will be no difficulty in downloading from Dropbox. 🙂

[Arduino](https://www.arduino.cc/download_handler.php?f=/arduino-1.8.2-windows.exe(
[LED.apk](https://www.dropbox.com/s/lfjtxakqpcqmg6s/LED.apk?dl=0)
[EEPROM.ino](https://www.dropbox.com/s/d8q2mcidegimdke/sketch_dec06a.ino?dl=0)


This was my writing about the use of eptim in Arduino. I do not know how much I could understand. But try to explain simply. If there is any difficulty in understanding, tell me by commenting. Everyone will be good. If you have time, you will see later.
