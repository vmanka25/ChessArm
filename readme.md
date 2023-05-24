# Chess Arm
### What we were trying to accomplish
This project was born off of one question.. "What if instead of using a cartisean arm for a chess robot, we use a cylendrical arm instead!"
So we were off trying to make a project that many have done, but with a twist this time. Our goal was to have a fully functioning robot that could
play chess with you, with a full coordinate system that would be used in tandem with the code. Needless to say this project was far too ambitious.

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
  
  This is the entire assembly, it involves two steppers which creates a full area of reach for the magnet. Along with 
  multiple layers seving a different function.

  ![GoodShot2](https://user-images.githubusercontent.com/71350243/224200193-7347c6ab-93db-44d7-b885-f2e1d252ac86.png)
  
  The first stepper motor is firmly cemented in the base (bottom part). Then a second stepper resides on top of that
  so that it can move the rack in which the magnet holder resides on.
  ![Shelf](https://github.com/vmanka25/ChessArm/assets/112979207/a77577c4-dbfb-4645-8441-94e69e36b113)

  The shelf is designed as a resting place for the rack in which the magnet holder resides on. 
  
  ![Pillar](https://github.com/vmanka25/ChessArm/assets/112979207/c0b435bd-7d63-4ceb-9729-258e669044a4)

  This is the pillar, it is mean't to encase each side with support as the project works on multiple horizontal layers
  horizantally seperated from each other. Each pillar has two holes for the yellow piece below.
 
 ![BLARRRGGH!!!!](https://github.com/vmanka25/ChessArm/assets/112979207/8a209d9d-55bb-48d7-b670-f50adbbbfa53)

This is the shelf it is the very thing that the magnet holder resides on, it acts as a second layer, it has a large
hole in the middle for the stepper to fully function.
 
 ![Yellow brick](https://github.com/vmanka25/ChessArm/assets/112979207/4ec6096c-9167-4fc6-addf-5eb79cbb366b)

This piece is mean't to hold the shelf up, it is the piece mean't to go into those very two holes on the pillar.
  
  ![Screenshot 2023-05-24 133723](https://github.com/vmanka25/ChessArm/assets/112979207/e14289c0-58ad-4332-846d-031c8d2b2e63)

This is the design of the chess grid with our names on it.  
  
  
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
  
  There were many obstacles as we designed and assembled our project. The Onshape document took a lot of time to make, after we realized we made it too big and we had to make a redesigned version. This version was a lot smaller and utilized a shelf to hoist the rack and pinion. Coding was also an obstacle because neither me or Joshua are very good at coding. The Code was the singular most infuriating part of the project, we've spent weeks trying new things but eventually we found a code that worked. Vincent got sick like 3 times over the winter while I (Joshua) got sick once (not a flex or anything). (add more stuff)
  
  ## Reflection
  Write a better reflection.
  
