# Arduino-Obstacle-Avoidance-Robot
Using HC-SR04 Arduino module to measure distance and move away from the obstacles.

<br>
<center><b>Arduino-Obstacle-Avoidance-Robot</b></center>
<br/>


<br>
<img src="https://github.com/S0undWav3s/Arduino-Obstacle-Avoidance-Robot/blob/main/Media/picture_01.jpg" width=360 HEIGHT=540> 
<img src="https://github.com/S0undWav3s/Arduino-Obstacle-Avoidance-Robot/blob/main/Media/picture_02.jpg" width=360 HEIGHT=540>
<br/>

<b>Arduino Code:</b>

```C
#include <Servo.h>  //servo library included
int ServoPin = 2;   //servo Pin declared
Servo servo;        //servo object created

int Trigger_Pin = 10;
int Echo_Pin = 11;
float signalTime;
float Time;
float D0;
float D1;
float D2;
// speed of sound=343 m/s

int IN1 = 6;
int IN2 = 7;
int IN3 = 8;
int IN4 = 9;
int ENA=3;
int ENB=5;


void Forword(){          //Function Front
analogWrite(ENA,85);
analogWrite(ENB,95);
delay(10);
digitalWrite(IN1,HIGH);
digitalWrite(IN2,LOW); //FRONT
digitalWrite(IN3,LOW);
digitalWrite(IN4,HIGH); //FRONT
delay(25);
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW);
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);

}

void Backwords(){
analogWrite(ENA,85);
analogWrite(ENB,85);
delay(10);
digitalWrite(IN1,LOW);
digitalWrite(IN2,HIGH); //back
digitalWrite(IN3,HIGH);
digitalWrite(IN4,LOW); //back
delay(100);
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
delay(100);
}

void BACK_LEFT(){
analogWrite(ENA,85);
analogWrite(ENB,85);
digitalWrite(IN1,LOW);
digitalWrite(IN2,HIGH); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,HIGH); 
delay(500);
}

void BACK_RIGHT(){
analogWrite(ENA,85);
analogWrite(ENB,85);
digitalWrite(IN1,HIGH);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,HIGH);
digitalWrite(IN4,LOW); 
delay(500);
}

void CENTER_MEASUREMENT(){
digitalWrite(Trigger_Pin,LOW);             //Low Trigger
delayMicroseconds(20);                     //delay in uS
digitalWrite(Trigger_Pin,HIGH);            //High Trigger
delayMicroseconds(20);                     //delay in uS
digitalWrite(Trigger_Pin,LOW);             //Low Trigger
delayMicroseconds(20);                     //delay in uS
signalTime = pulseIn(Echo_Pin,HIGH);       //measuring time in uS when the Signal is received
delayMicroseconds(20);
//Serial.println(signalTime);
Time = (signalTime/1000000/2); //Convert Micro-Seconds to Seconds(divide by 1,000,000).Divide by 2 beceause the signal goes from the HC-SR04 to the object and back to the sensor(we need to measure the distance only to the object).
delayMicroseconds(20);
D0=(34300*Time);  //  Distance = Speed * Time, Speed_of_sound = 343,000 cm/s and the time we have it from the 25 code line.
}

void RIGHT_MEASUREMENT(){
digitalWrite(Trigger_Pin,LOW);             //Low Trigger
delayMicroseconds(10);                     //delay in uS
digitalWrite(Trigger_Pin,HIGH);            //High Trigger
delayMicroseconds(10);                      //delay in uS
digitalWrite(Trigger_Pin,LOW);             //Low Trigger
delayMicroseconds(1);                     //delay in uS
signalTime = pulseIn(Echo_Pin,HIGH);       //measuring time in uS when the Signal is received
delayMicroseconds(10);  
//Serial.println(signalTime);
Time = (signalTime/1000000/2); //Convert Micro-Seconds to Seconds(divide by 1,000,000).
                               //Divide by 2 beceause the signal goes from the HC-SR04 to the object and back to the sensor(we need to measure the distance only to the object).
delayMicroseconds(10); 
D1=(34300*Time);  //  Distance = Speed * Time, Speed_of_sound = 343,000 cm/s and the time we have it from the 25 code line.
}

void LEFT_MEASUREMENT(){
digitalWrite(Trigger_Pin,LOW);             //Low Trigger
delayMicroseconds(10);                     //delay in uS
digitalWrite(Trigger_Pin,HIGH);            //High Trigger
delayMicroseconds(10);                     //delay in uS
digitalWrite(Trigger_Pin,LOW);             //Low Trigger
delayMicroseconds(10);                     //delay in uS
signalTime = pulseIn(Echo_Pin,HIGH);       //measuring time in uS when the Signal is received
delayMicroseconds(10);
//Serial.println(signalTime);
Time = (signalTime/1000000/2); //Convert Micro-Seconds to Seconds(divide by 1,000,000).
                               //Divide by 2 beceause the signal goes from the HC-SR04 to the object and back to the sensor(we need to measure the distance only to the object).
delayMicroseconds(10);
D2=(34300*Time);  //  Distance = Speed * Time, Speed_of_sound = 343,000 cm/s and the time we have it from the 25 code line.
}


void setup() {
Serial.begin(9600);
servo.attach(ServoPin);
pinMode(Trigger_Pin,OUTPUT);
pinMode(Echo_Pin,INPUT);  
pinMode(IN1,OUTPUT);
pinMode(IN2,OUTPUT);
pinMode(IN3,OUTPUT);
pinMode(IN4,OUTPUT);
pinMode(ENA,OUTPUT);
pinMode(ENB,OUTPUT);
}

void loop() {
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW);
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
servo.write(55); //Center Distance Measurement
delay(1000);

CENTER_MEASUREMENT();
if(D0 > 15 ) { 
while(D0>=15){ 
CENTER_MEASUREMENT();
D0;
Forword(); //goes forward
} 
}

else if(D0 <= 15 ){
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);




servo.write(20); //servo goes to 20 degrees (Right)
delay(500);
RIGHT_MEASUREMENT();


servo.write(55); // Servo Center
delay(500);




servo.write(100); //servo goes to 100 degrees
delay(500);
LEFT_MEASUREMENT();



if(D1>D2)
{
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
delay(100);
Backwords();
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
BACK_LEFT();
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
delay(100);
}
else if(D1<D2){
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
delay(100);
Backwords();
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
delay(100);
BACK_RIGHT();
digitalWrite(IN1,LOW);
digitalWrite(IN2,LOW); 
digitalWrite(IN3,LOW);
digitalWrite(IN4,LOW);
delay(100);
}
}
}




```

