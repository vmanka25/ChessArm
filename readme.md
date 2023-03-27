# Chess Arm
## Planning
https://docs.google.com/document/d/1w128rAIOWzhnq0nkOWPgndezun0yfNx6PoaC3awFNQs/edit?usp=sharing
## Onshape Document
https://cvilleschools.onshape.com/documents/aae0fa9e1b02e080af2f278c/w/f677a802c177e5583c740454/e/9a0adf56d347956ab28b4bb6?renderMode=0&uiState=64079963477be604b36eb6c1

## Materials Used
+ Acrylic
+ PLA
+ 2 stepper motors
+ Arduino uno
+ Battery pack
+ Solenoid
+ magnets

## Wiring Diagram
![image](https://user-images.githubusercontent.com/71350243/224154497-09df0b6e-61dd-4b60-9a46-b2c5e3361fe6.png)

## Code

```C++
#include <Stepper.h>
#include <math.h>

const int RSteps = 200; // change this value to match your rack stepper motor
const int AngleSteps = 200; // change this value to match your pinion stepper motor
Stepper RStepper(RSteps, 8, 9, 10, 11); // connect the rack stepper motor to pins 8, 9, 10, and 11
Stepper AngleStepper(AngleSteps, 12, 13, 14, 15); // connect the pinion stepper motor to pins 12, 13, 14, and 15

int y;
int x;
double radius;
double angle;

void setup() {
  RStepper.setSpeed(60); // set the speed of the rack stepper motor in revolutions per minute
  AngleStepper.setSpeed(60); // set the speed of the pinion stepper motor in revolutions per minute
  Serial.begin(9600);
}

void loop() {
  // read in the two variables from the user
  y = analogRead(A0);
  x = analogRead(A1);
  y = map(y, 0, 0, 8);
  x = map(x, 0, 0, 8);

  
  Serial.print("y: ");
  Serial.print(y);
  Serial.print("\t x:");
  Serial.print(x);
  
  radius = sqrt((27.5 * y) * (27.5 * y) + (27.5 * x) * (27.5 * x));
  angle = (atan2(y,x) * 180 / M_PI);
  RStepper.step(int((radius*200)/360));
  AngleStepper.step(int((angle*200)/360));
  ```
  
 ### Codev2
 
 ```C++
#include <Stepper.h>

const int stepsPerRevolution = 200;  // change this to fit the number of steps per revolution for your motor
const int topStepperPin1 = 2;
const int topStepperPin2 = 3;
const int topStepperPin3 = 4;
const int topStepperPin4 = 5;
const int bottomStepperPin1 = 6;
const int bottomStepperPin2 = 7;
const int bottomStepperPin3 = 8;
const int bottomStepperPin4 = 9;
const int gearTeeth = 20;  // number of teeth on the gear
const float armLength = 218.65240;  // length of the arm in mm
const float maxPos = 138.59293;  // maximum position in mm
const float minPos = -138.59293;  // minimum position in mm
const int xPotPin = A0;  // analog input pin for the x potentiometer
const int yPotPin = A1;  // analog input pin for the y potentiometer
const int solenoidPin = 10;  // digital output pin for the solenoid
const int buttonPin = 11;  // digital input pin for the button

Stepper topStepper(stepsPerRevolution, topStepperPin1, topStepperPin2, topStepperPin3, topStepperPin4);
Stepper bottomStepper(stepsPerRevolution, bottomStepperPin1, bottomStepperPin2, bottomStepperPin3, bottomStepperPin4);

int lastXPotVal = -1;
int lastYPotVal = -1;

void setup() {
  // set up the solenoid and button pins
  pinMode(solenoidPin, OUTPUT);
  pinMode(buttonPin, INPUT);
  
  // set the speed of the motors
  topStepper.setSpeed(60);
  bottomStepper.setSpeed(30);
}

void loop() {
  // read the potentiometer values
  int xPotVal = analogRead(xPotPin);
  int yPotVal = analogRead(yPotPin);
  
  // check if the potentiometer values have changed
  if (xPotVal != lastXPotVal || yPotVal != lastYPotVal) {
    // update the last potentiometer values
    lastXPotVal = xPotVal;
    lastYPotVal = yPotVal;
    
    // convert the potentiometer values to Cartesian coordinates
    float xPos = map(xPotVal, 0, 1023, minPos, maxPos);
    float yPos = map(yPotVal, 0, 1023, minPos, maxPos);
    
    // calculate the distance the arm should move
    float armDist = sqrt(xPos * xPos + yPos * yPos) / 10;  // convert mm to cm
    
    // calculate the angle to move the arm to
    float armAngle = atan2(yPos, xPos);
    
    // rotate the bottom stepper to move the top mount
    bottomStepper.step(stepsPerRevolution/4);
    
    // calculate the number of steps for the top stepper
    int topSteps = int(armDist / (2 * M_PI * armLength / gearTeeth) * stepsPerRevolution);
    
    //
```
  
  ## Photos
 
  ![GoodShot](https://user-images.githubusercontent.com/71350243/224200159-39b65a96-70e8-4f7f-bd1b-6f4d997a2ca9.png)

  ![GoodShot2](https://user-images.githubusercontent.com/71350243/224200193-7347c6ab-93db-44d7-b885-f2e1d252ac86.png)
  
  ## Video
  
  [link](https://github.com/vmanka25/ChessArm/blob/main/ChessBoard2.gif)
 
  ## Milestones
  
  + 11/29/22 started Onshape Document
  + 01/24/23 Finished Code
  + 01/24/23 Tested code
  + 02/12/23 Remade Onshape document
  + 03/9/23 Finished revised Onshape document
  + 03/9/23 made Code V2
  + 03/10/23 printed and laser cut
  
  ## Obstacles
  
  There were many obstacles as we designed and assembled our project. The Onshape document took a lot of time to make, after we realized we made it too big and we had to make a redesigned version. This version was a lot smaller and utilized a shelf to hoist the rack and pinion. Coding was also an obstacle because neither me or Joshua are very good at coding. The Code was the singular most infuriating part of the project, we've spent weeks trying new things but eventually we found a code that worked. Vincent got sick like 3 times over the winter while I (Joshua) got sick once (not a flex or anything).
  
  ## Reflection
  
  This project took a lot of time and was very stressful. If we were to redo this we would have used a rough prototysooner. I would not make it a polar arm but a cartesian arm as the math and code are difficult. I would have tried to stick with the timeline and done an easier project if I could do it again. 
