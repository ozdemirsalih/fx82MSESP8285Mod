# Casio Cheat Calculator
## Modding a Casio fx-82MS calculator with an ESP8285 to make it controllable over WiFi.

***I'm not able to find a job over online applications. They said uploading some of the things I've created onto GitHub could help, so here we go with the first one.***

***I've originally created this mod to cheat on my exams during my collage years(back in 2017), but never actually used it for cheating. So first and foremostly, I need to disclose that I don't condone cheating in academia or in any educational environment, neither do I encourage anyone using this mod to cheat on their exams. This project is here simply for the sake of sharing and educational purposes.***

The working principle might be simplified as a "physical virus", since the ESP8285 is not controlling the calculator over a digital interface, but it's rahter simulating physical button pushes. The fx-82MS has exposed pads marked as ***KI1, KI2, KI3, KI4, KI5, KI6, KI7, KI8, KO1, KO2, KO3, KO4, KO5, KO6, and KO7*** as it can be seen in the picture below;

![Casio fx-82MS PCB](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/Casio%20fx82MS%20Circuit.jpg)

If you connect any one of the ***KI*** pads to any one of the ***KO*** pads, you'll see that calculator's CPU will register it as a button push and it'll display a character. For instance, if you connect ***KI1*** to ***KO1***, the calculator will register it as if it is a physical push on the button "1" of the calculator and it'll show the number "1" on its display.

By exploiting this feature, we're able write anything we want onto the calculator display by just creating connections between these pads.

And to create these connections at our will, we can use a microcontroller, even better we can use a WiFi cabaple microcontroller which enables us to control our calculator over WiFi and even over internet if you want to go to that length. So with this purpose, I've designed a PCB which includes an ***ESP8285***, 2 ***74HC4051*** ICs and a ***SN74LVC1G3157*** to create the desired connections between the exposed pads. As it can be seen in the picture below;

![Assembled Circuit](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/Casio-Cheating-Calculator-Assembled.jpg)

You might be asking yourself, "Okay, the calculator is controllable over WiFi now, which can help you cheat on an exam, but how're you gonna send the question to your friend on their computer while you're in an exam?", good thing you asked! There's also a ***BMX055*** from ***Bosch Sensortec***, which is a 9-Axis IMU, which we can use its accelerometer as a Morse Code median. So you can send the questions to your friend by tapping Morse Code onto your calculator.

Here's the top and bottom images of the PCB design with its pin map to the exposed calculator pads, along with pinout for programming, debugging, and charging the Li-Ion battery(there's no battery charging IC is involved in the design, only a DW01A for battery protection, so an external charger is needed, e.g. TP4056);

![PCB Top](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/PCB%20Top%20Image.png)
![PCB Bottom](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/PCB%20Bottom%20Image.png)

You can find the Eagle project files and the gerber files in the repository.

The ESP8285 acts as an Access Point and as a websocket server to serve a webpage, in which you can control the calculator via its UI. Here's the screenshot of the UI;

![UI](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/Cheating-Calculator-UI%20.jpg)

As you can see, I've also created a font for this project which is a pixel perfect copy of the font that's used in the Casio fx-82MS. You can find the .ttf file in the repository. This font also enables controlling the calculator directly from your keyboard. Here's the key mapping;

![Font Key Map](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/fx-82MS%20Font%20Table%20-%201.png)
![Font Key Map](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/fx-82MS%20Font%20Table%20-%202.png)
![Font Key Map](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/fx-82MS%20Font%20Table%20-%203.png)
![Font Key Map](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/fx-82MS%20Font%20Table%20-%204.png)

I also wrote a whole calibration tool for the ***BMX055***'s accelerometer. In which you can control every single function and every single register of the accelerometer, including Self-Test, Fast Compensation, Slow Compensation(High Pass Filter), I2C Watchdog, Bandwidth, Range, Offsets, and NVM(Non Volatile Memory). You can also observe the the acceleration values, the temperature, the roll, and the pitch of the accelerometer in real time(up to 2000fps). As it can be seen in the screenshot below;

![Accelerometer UI](https://github.com/ozdemirsalih/Casio-Cheat-Calculator/blob/main/Cheating-Calculator-Accelerometer-UI.jpg)

As of now, the gyroscope and the magnetometer haven't been implemented into the code yet, but they'll be added soon.

#### Note: In order to get 2000fps from the accelerometer, the ESP8285's I2C buffer size needs to be increased to 192 bytes, as explained below;

* On MacOS: /Users/{username}/Library/Arduino15/packages/esp8266/hardware/esp8266/3.0.1/libraries/Wire/Wire.h
* On Windows: C:\Users\{username}\AppData\Local\Arduino15\packages\esp8266\hardware\esp8266\3.0.1\libraries\Wire\Wire.h
* On Linux: /home/{username}/.arduino15/packages/esp8266/hardware/esp8266/3.0.1/libraries/Wire/Wire.h

Replace 
```C
#define BUFFER_LENGTH 128
```
with
```C
#define BUFFER_LENGTH 192
```

#### Note: The default SSID & password of the Access Point is as listed below;

```C++
const char* ssid = "my_calculator";
const char* password = "123456789";
```
