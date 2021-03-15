# sim900a_send_receive
Code for sending and receiving SMS messages from the SIM900a device

The send and receive code sends SMS to my phone.



https://miliohm.com/sim900a-module-with-arduino-tutorial/


![image](https://user-images.githubusercontent.com/14288989/111183894-66da2500-85d6-11eb-9f15-f2a9067ba792.png)

```
Change mode to sms :

AT+CMGF=1


Read SMS in text mode :

AT+CNMI=2,2,0,0,0

Make a call :

ATD+1123456789; //replace with number and country code you like

Disconnect / hangup call :

ATH

Receive a phone call :

ATA
```




```

#include <SoftwareSerial.h>
SoftwareSerial SIM900A(10,11); // RX | TX
// Connect the SIM900A TX to Arduino pin 2 RX. 
// Connect the SIM900A RX to Arduino pin 3 TX. 
char c = ' ';
void setup() 
{
// start th serial communication with the host computer
Serial.begin(9600);
while(!Serial);
Serial.println("Arduino with SIM900A is ready");
// start communication with the SIM900A in 9600
SIM900A.begin(9600); 
Serial.println("SIM900A started at 9600");
delay(1000);
Serial.println("Setup Complete! SIM900A is Ready!");
}
void loop()
{
// Keep reading from SIM800 and send to Arduino Serial Monitor
if (SIM900A.available())
{ c = SIM900A.read();
Serial.write(c);}
// Keep reading from Arduino Serial Monitor and send to SIM900A
if (Serial.available())
{ c = Serial.read();
SIM900A.write(c); 
}
}
```


Code to send and receive


```
#include <SoftwareSerial.h>

SoftwareSerial SIM900A(3,2);

void setup()
{
  SIM900A.begin(9600);   // Setting the baud rate of GSM Module  
  Serial.begin(9600);    // Setting the baud rate of Serial Monitor (Arduino)
  Serial.println ("SIM900A Ready");
  delay(100);
  Serial.println ("Type s to send message or r to receive message");
}


void loop()
{
  if (Serial.available()>0)
   switch(Serial.read())
  {
    case 's':
      SendMessage();
      break;
    case 'r':
      RecieveMessage();
      break;
  }

 if (SIM900A.available()>0)
   Serial.write(SIM900A.read());
}


 void SendMessage()
{
  Serial.println ("Sending Message");
  SIM900A.println("AT+CMGF=1");    //Sets the GSM Module in Text Mode
  delay(1000);
  Serial.println ("Set SMS Number");
  SIM900A.println("AT+CMGS=\"+91xxxx\"\r"); //Mobile phone number to send message
  //SIM900A.println("AT+CMGS=\"+919xxxx\"\r"); //Mobile phone number to send message

  delay(1000);
  Serial.println ("Set SMS Content");
  SIM900A.println("Hello Shashi, 9:23PM?");// Messsage content
  delay(100);
  Serial.println ("Finish");
  SIM900A.println((char)26);// ASCII code of CTRL+Z
  delay(1000);
  Serial.println ("Message has been sent ->SMS Shashi Kiran");
}


 void RecieveMessage()
{
  Serial.println ("SIM900A  SMS");
  delay (1000);
  SIM900A.println("AT+CNMI=2,2,0,0,0"); // AT Command to receive a live SMS
  delay(1000);
  Serial.write ("Unread Message done");
 }

```
