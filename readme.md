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
  
  ## Photos
 
  ![GoodShot](https://user-images.githubusercontent.com/71350243/224200159-39b65a96-70e8-4f7f-bd1b-6f4d997a2ca9.png)

  ![GoodShot2](https://user-images.githubusercontent.com/71350243/224200193-7347c6ab-93db-44d7-b885-f2e1d252ac86.png)

  ## Milestones
  
  + 11/29/22 started Onshape Document
  + 01/24/23 Finished Code
  + 01/24/23 Tested code
  + 02/12/23 Remade Onshape document
  + 03/9/23 Finished revised Onshape document
  + 03/10/23 printed and laser cut
  
  ## Obstacles
  
  There were many obstacles as we designed and assembled our project. The Onshape document took alot of time to make and after we realized we made it too big we had to make a new redesigned version. this version was alot smaller and better balanced. coding was also an obstacle because neither me or joshua are very good at coding. I still can't get the code to work.
  
  ## Reflection
  
  This project took alot of time and was very stressful. if I were to redo this I would have prototyped and started printing sooner. I would not make it a polar arm but a cartesian arm as the math and code are difficult. I would have tried to stick with the timeline and done an easier project if I could do it again. 
