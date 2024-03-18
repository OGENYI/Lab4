
### Part 4: Using Wi-Fi on your Arduino
```c
#include <Arduino.h>
#include <SoftwareSerial.h>
#define DEBUG true
#define serialCommunicationSpeed 115200
SoftwareSerial esp8266(2,3); //TX TO PIN 2, RX TO PIN 3
//int wifiEnablePin = 8;

String sendData(String command, const int timeout, boolean debug)
{
    String response = ""; //initialize a String variable named "response". we will use it later.
if(debug) //if the "debug" variable value is TRUE, print the response on the Serial monitor.
    {
    Serial.print(command);
    }
    esp8266.print(command); //send the AT command to the esp8266 (from ARDUINO to ESP8266).
    long int time = millis(); //get the operating time at this specific moment and save it inside the "time" variable.
    while( (time+timeout) > millis()) //excute only whitin 1 second.
    {
        while(esp8266.available()) //is there any response came from the ESP8266 and saved in the Arduino input buffer?
        {
          char c = esp8266.read(); //if yes, read the next character from the input buffer and save it in the "response" String variable.
          response+=c; //append the next character to the response variabl. at the end we will get a string(array of characters) contains the response.
          }
        }  
        if(debug) //if the "debug" variable value is TRUE, print the response on the Serial monitor.
        {
        Serial.print(response);
        }
        return response; //return the String response.
}
void InitWifiModule()
{
    sendData("AT+RST\r\n", 2000, DEBUG); //reset the ESP8266 module.
    delay(1000);
    sendData("AT+CWJAP=\"Emeoha\",\"emeoha11\"\r\n", 2000, DEBUG); //connect to the WiFi network.
    delay (3000);
    sendData("AT+CWMODE=3\r\n", 1500, DEBUG); //set the ESP8266 WiFi mode to station mode.
    delay (1000);
    sendData("AT+CIFSR\r\n", 1500, DEBUG); //Show IP Address, and the MAC Address.
    delay (1000);
    sendData("AT+CIPMUX=1\r\n", 1500, DEBUG); //Multiple connections.
    delay (1000);
    sendData("AT+CIPSERVER=1,80\r\n", 1500, DEBUG); //start the communication at port 80, port 80
    //used to communicate with the web servers through the http requests.
}
void setup() {
  Serial.begin(serialCommunicationSpeed);
  esp8266.begin(serialCommunicationSpeed);
  InitWifiModule();
}
void loop() {
// put your main code here, to run repeatedly:
}

```
