# Asshole-Robot

//IDEAL SEQUENCE OF EVENTS
  //lights are on
  // lights TURN OFF
  //eyeballs turn on
  //eyeballs move sideways
  //lights TURN ON
  //eyes turn off 
  //return eyes to first position
  //INCREASE arbitrary counter by one
  //I grab keys
  //lights TURN OFF
  //eye balls turn on (don't move)
  //hand slowly moves and pushes mug off table
  
//CONSTANTS
const int greenLED2 = 2; //set LEDS to pin 2 and 3
const int greenLED3 = 3;

const int lightSensor = A0; //set light sensor to pin A0


//VARIABLE INTEGERS
int sensorValue = 0; //the value of the light sensor

#include <Servo.h> //adding servo to the program
Servo servoHead; //naming the head servo motor
Servo servoArm; //naming the arm servo motor

int lightsOn = 1;
int previousLightState = 1; //holds pervious sensor value to test if it has changed
int sensorStateCounter = 0;

//SETUP
void setup() {

servoHead.attach(9); //setting the head servo to pin 9
servoArm.attach(10); //setting arm servo to pin 10
servoHead.write(130); //set initial servo head position to 130 degrees
servoArm.write(60); //set intial servo arm position to 60 degrees

Serial.begin(9600); //opens up serial port to view sensor values on comp
  

pinMode(2, OUTPUT); //set greenLED2 and greenLED3 pins to output mode
pinMode(3, OUTPUT);
 
}

//PROGRAM
void loop() {

  sensorValue = analogRead(lightSensor); //read the input from the sensor
  
  delay(100); //just to not overload things too much i guess

  
  Serial.println("Light Sensor Value: "); //print words "Light Sensor Value" in serial viewer
  Serial.println(sensorValue); //print the sensor value between 0-1023
  Serial.println("sensorStateCounter"); //print text "sensorStateCounter"
  Serial.println(sensorStateCounter); //print the count of how many times the light come on
  
  
  if (sensorValue < 12 && sensorStateCounter >= 3) { //LIGHTS OFF, ON, OFF
    delay(2000);
    
    digitalWrite(greenLED2, HIGH);
    digitalWrite(greenLED3, HIGH);
    servoHead.write(130); // keep head looking straight ahead

    delay(3000); //pause for comedic timing

    servoArm.write(170); // move arm to knock over vase or w/e
  }

  
  if(sensorValue > 12) { // LIGHTS ON
    digitalWrite(greenLED2, LOW);
    digitalWrite(greenLED3, LOW);
    
    servoHead.write(130); //set servo head angle to intial position
    lightsOn = 0;
  }
  
  if (sensorValue < 12 && sensorStateCounter < 3) { //FIRST LIGHTS OFF
    delay(4000);
    
    digitalWrite(greenLED2, HIGH);
    digitalWrite(greenLED3, HIGH);

    delay(1000);
    
    servoHead.write(179); //set head to look at door

    lightsOn = 1;
  } //if the light sensor input is low, turn on LEDs and set eye postition to final
  
  if (previousLightState != lightsOn) {
    sensorStateCounter++;
  } //if the state has just changed from the light on to light off, the counter goes up one

  previousLightState = lightsOn;    //set previousLightState to equal current lights on to test 
                                    //change in state
}
