# Light-Sensing-Watch
A watch that detects if you are outside based on the light intensity of the surroundings, it tracks the total amount of time spent outdoors, based on this time a red, yellow or green led lights up when a button is pressed

light intensity, greater than or equal to 1500 lux, assumes outdoors and timer starts
light intensity, less than or equal to 1500 lux, assumes indoorss and timer stops

when button pressed... red light flashes if total time outdoors has been less than 60 mins
                       yellow light flashes if total time outdoors has been 60-90 mins
                       green light flashes if total time outdoors has been 90 mins +
                       
  CODING: (THIS DOES NOT YET WORK SO ANY SUGGESTIONS WOULD BE HUGELY APPRECIATED)                     


#include <BH1750.h> //Sensor Library
#include <Wire.h> //12C Library
BH1750 lightMeter;

const int buttonPin = 7; //changed to 7 from 2

const int RedLedPin = 13;
const int YellowLedPin =12;
const int GreenLedPin = 11;

int buttonState = 0;

uint16_t outdoors = 0; ///now they (2) are global variables
int x = 10000;

void setup() {
   
    Serial.begin(9600);
    Wire.begin();
    lightMeter.begin();
    
    uint16_t outdoors = 0;    ///local variable?! (these two lines are junk code because i have rewritten them as global variables)
    int x = 10000;
    
    pinMode(buttonPin, INPUT);
    
    pinMode(RedLedPin, OUTPUT);
    pinMode(YellowLedPin, OUTPUT);
    pinMode(GreenLedPin, OUTPUT);
}

void loop() {
   
    uint16_t lux = lightMeter.readLightLevel();
    buttonState = digitalRead(buttonPin);
    int buttonpushed = 0 ;       //container to track if the "status checking" button was pressed, if it reduces the delay, as led HIGH takes up 5/10 secs of the end delay
   
   while ( lightMeter.readLightLevel()>= 1500)                   // PERSON IS OUTDOORS
   {
    
    outdoors += x;                                 //compound addition (equivalently: outdoors = outdoors + x, would work too)
    
    if (buttonState == HIGH)                       //push button pressed while outdoors
      
      {int buttonpushed = 1;
      
      if (outdoors>=5400000){                     //5,400,000milliseconds=1.5hours    total time outdoors>=1.5HRS
      digitalWrite(GreenLedPin,HIGH);
        delay(5000);                              //LED on for 5 secs
        digitalWrite(GreenLedPin,LOW);                           
      }
      else if (3600000<=outdoors<5400000){        //3,600,000milliseconds=1hour     1HR<= total time outdoors <1.5HRS
      digitalWrite(YellowLedPin,HIGH);
        delay(5000);                              //LED on for 5 secs
        digitalWrite(YellowLedPin,LOW);}      
      else (outdoors<3600000);{                    //                                total time outdoors<1HR
      digitalWrite(RedLedPin,HIGH);
        delay(5000);                              //LED on for 5 secs
        digitalWrite(YellowLedPin,LOW);}
      
     }
    
   if (int buttonpushed = 1){
    delay(5000);}                                //5 second delay
    else (buttonpushed = 0);{
   delay(10000);}                                //10 second delay
   
     
   } uint16_t outdoors = outdoors +0;           //???                                 //OTHERWISE PERSON IS INDOORS
  
    if (buttonState == HIGH) {                    //push button pressed whilst indoors
      
      if (outdoors >= 5400000){                     //5,400,000milliseconds=1.5hours,    total time outdoors>=1.5HRS
      digitalWrite(GreenLedPin,HIGH);
        delay(5000);                              //LED on for 5 secs
        digitalWrite(GreenLedPin,LOW);}
      else if (3600000<=outdoors<5400000){        //3,600,000milliseconds=1hour,      1HR<= total time outdoors <1.5HRS
      digitalWrite(YellowLedPin,HIGH);
        delay(5000);                              //LED on for 5 secs
        digitalWrite(YellowLedPin,LOW);}      
      else (outdoors<3600000);{                    //                                    total time outdoors<1HR
      digitalWrite(RedLedPin,HIGH);
        delay(5000);                              //LED on for 5 secs
        digitalWrite(RedLedPin,LOW);}
      
     }
}
 
