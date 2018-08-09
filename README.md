# Automatic-Car-Parking-System

 //AUTOMATIC CAR PARKING SYSTEM//


#include <Stepper.h>


//steps per revolutions of the motors

const int stepsPerRevolution1 = 200; //motor1 (rotation)
const int stepsPerRevolution2 = 200; //motor2 (lead screw)
const int stepsPerRevolution3 = 200; //motor3 (pick place)


// initialize the steppers library on pins 4-7 && 8-11 && 22-25

Stepper myStepper1(stepsPerRevolution1, 4,5,6,7); //motor for lead screww// cw+ = pin9 and clk+ = pin 10
Stepper myStepper2(stepsPerRevolution2, 8,9,10,11);    //motor for rotation// cw+ = pin5 and clk+ = pin 6
Stepper myStepper3(stepsPerRevolution3, 22,23,24,25); //motor for pick and place// 22 23 24 25

int a=0,b=0,c=0,d=0,e=0,f=0,g=0,h=0;//variable declaration end for each slot from 1 to 8 respectively

//to get signals from labview 
int var;

void setup() {
   // put your setup code here, to run once:
   
   // set the speed of motors as your requirement
  myStepper1.setSpeed(150);   //rotation motor
  myStepper2.setSpeed(2500);   //lead screw motor
  myStepper3.setSpeed(700);   //lead screw motor

  // initialize the serial port:
  Serial.begin(9600);

}

void loop() {
   // put your main code here, to run repeatedly:

if (Serial.available())
{
  var=Serial.read();
  
//if else statements to check the input signals
if (var=='p'){
  parking();}    //call for the parking function
else{
  all_stop();}
if (var=='1'){
  ss_pick();
  a=0;}
else{
  all_stop();}
if (var=='2'){
  st_pick();
  b=0;}
else{
  all_stop();}
if (var=='3'){
  sfo_pick();
  c=0;}
else{
  all_stop();}
if (var=='4'){
  ff_pick();
  d=0;}
else{
  all_stop();}
if (var=='5'){
  fs_pick();
  e=0;}
else{
  all_stop();}
if (var=='6'){
  ft_pick();
  f=0;}
else{
  all_stop();}
if (var=='7'){
  ffo_pick();
  g=0;}
else{
  all_stop();}
if (var=='s'){
  all_stop();}
else{
  all_stop();}
}


}
//function for PARKING algorithm
void parking()
{
  if (a==0)//check 1st slot
  {
    ss_place();
    a=1;
  }
  else if (a==1)
  {
    if (b==0)//check 2nd slot
    {
      st_place();
      b=1;
    }
    else if (b==1)
    {
      if (c==0)//check 3rd slot
      {
        sfo_place();
        c=1;
      }
      else if (c==1)
      {
        if (d==0)//check 4th slot
        {
          ff_place();
          d=1;
        }
        else if (d==1)
        {
          if (e==0)//check 5th slot
          {
            fs_place();
            e=1;
          }
          else if (e==1)
          {
            if (f==0)//check 6th slot
            {
              ft_place();
              f=1;
            }
            else if (f==1)
            {
              if (g==0)//check 7th slot
              {
                ffo_place();
                g=1;
              }
//              else if (g==1)//rerurn the message of no space
//              {
//                 Serial.print("NO MORE SPACE <SORRY>");
//              }
            }       
          }
        }
      }
    }
  }
}



//functions for PLACING the car on the first floor//

void ff_place()//f(first floor)f(first slot)
{
  pick_car1();//upward(1)
  all_stop();
  s_floor();//move one floor up(2)
  place_car2();//downward(1)
  fb_floor();//move one floor down(0)
//  all_stop();
}

void fs_place()
{
  pick_car1();//upward(1)
  s_floor();//move one floor up(2)
  all_stop();
  s_slot();//rotate to second slot
  place_car2();//downward(1)
  all_stop();
  sb_slot();//rotate back
  fb_floor();//move one floor down(0)
//  all_stop();
}

void ft_place()
{
  pick_car1();//upward(1)
  s_floor();//move one floor up(2)
  all_stop();
  t_slot();//rotate to third slot
  place_car2();//downward(1)
  all_stop();
  tb_slot();//rotate back
  fb_floor();//move one floor down(0)
//  all_stop();
}

void ffo_place()
{
  pick_car1();//upward(1)
  s_floor();//move one floor up(2)
  all_stop();
  fo_slot();//rotate to fourth slot
  place_car2();//downward(1)
  all_stop();
  fob_slot();//rotate back
  fb_floor();//move one floor down(0)
//  all_stop();
}


//functions for PLACING the car on the same floor//

void ss_place()//s(same floor)f(second slot)
{
  pick_car1();//upward(1)
  all_stop();
  s_slot();//rotate to second slot
  place_car1();//downward(2)
  all_stop();
  sb_slot();//rotate back
//  all_stop();
}

void st_place()
{
  pick_car1();//upward(1)
  all_stop();
  t_slot();//rotate to third slot
  place_car1();//downward(2)
  all_stop();
  tb_slot();//rotate back
//  all_stop();
}

void sfo_place()
{
  pick_car1();//upward(1)
  all_stop();
  fo_slot();//rotate to fourth slot
  place_car1();//downward(2)
  all_stop();
  fob_slot();//rotate back
//  all_stop();
}


//functions for RETREIVING the car from the first floor//

void ff_pick()
{
  f_floor();//move one floor up(1)
  pick_car2();//upward(2)
  sb_floor();//move one floor down(1)
  place_car1();//downward(0)
//  all_stop();
}

void fs_pick()
{
  f_floor();//move one floor up(1)
  all_stop();
  s_slot();//rotate to second slot
  pick_car2();//upward(2)
  sb_slot();//rotate back
  all_stop();
  sb_floor();//move one floor down(1)
  place_car1();//downward(0)
//  all_stop();
}

void ft_pick()
{
  f_floor();//move one floor up(1)
  all_stop();
  t_slot();//rotate to third slot
  pick_car2();//upward(2)
  all_stop();
  tb_slot();//rotate back
  sb_floor();//move one floor down(1)
  all_stop();
  place_car1();//downward(0)
//  all_stop();
}

void ffo_pick()
{
  f_floor();//move one floor up(1)
  all_stop();
  fo_slot();//rotate to fourth slot
  pick_car2();//upward(2)
  all_stop();
  fob_slot();//rotate back
  sb_floor();//move one floor down(1)
  all_stop();
  place_car1();//downward(0)
//  all_stop();
}


//functions for RETRIEVING the car from the same floor//

void ss_pick()
{
  s_slot();//rotate to second slot
  pick_car1();//upward(3)
  all_stop();
  sb_slot();//rotate back
  place_car1();//downward(0)
//  all_stop();
}

void st_pick()
{
  t_slot();//rotate to third slot
  pick_car1();//upward(3)
  all_stop();
  tb_slot();//rotate back
  place_car1();//downward(0)
//  all_stop();
}

void sfo_pick()
{
  fo_slot();//rotate to fourth slot
  pick_car1();//upward(3)
  all_stop();
  fob_slot();//rotate back
  place_car1();//downward(0)
//  all_stop();
}


//functions for UPWARD motion for different floors//

void f_floor()
{
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
  myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
//      myStepper2.step(-stepsPerRevolution2*50);
//  delay(10);
      myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
      myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
      myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
}
void s_floor()
{
      myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
  myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
    myStepper2.step(-stepsPerRevolution2*100);
  delay(10);
      myStepper2.step(-stepsPerRevolution2*50);
  delay(10);
}


//functions for DOWNWARD motion for different floors//

void fb_floor()

{
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
    myStepper2.step(stepsPerRevolution2*100);
    delay(10);
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
        myStepper2.step(stepsPerRevolution2*100);
    delay(10);
        myStepper2.step(stepsPerRevolution2*100);
    delay(10);
          myStepper2.step(stepsPerRevolution2*100);
  delay(10);
          myStepper2.step(stepsPerRevolution2*100);
    delay(10);
//        myStepper2.step(stepsPerRevolution2*50);
//  delay(10);
}
void sb_floor()
{
     myStepper2.step(stepsPerRevolution2*100);
    delay(10);
    myStepper2.step(stepsPerRevolution2*100);
    delay(10);
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
      myStepper2.step(stepsPerRevolution2*100);
    delay(10);
        myStepper2.step(stepsPerRevolution2*100);
    delay(10);
}

//Rotate functions for different SLOTS//

void s_slot()//s(second)
{
  myStepper1.step(stepsPerRevolution1*8);//rotate for 2nd slot
  delay(50);
}
void t_slot()
{
  myStepper1.step(stepsPerRevolution1*16);//rotate for 3rd slot
  delay(50);
}
void fo_slot()
{
  myStepper1.step(stepsPerRevolution1*24);//rotate for 4th slot
  delay(50);
}

//Rotate Back functions for different SLOTS//

void sb_slot()//s(secon)b(back)
{
  myStepper1.step(-stepsPerRevolution1*8);//rotate back for 2nd slot
  delay(50);
}
void tb_slot()
{
  myStepper1.step(-stepsPerRevolution1*16);//rotate back for 3rd slot
  delay(50);
}
void fob_slot()
{
  myStepper1.step(-stepsPerRevolution1*24);//rotate back for 4th slot
  delay(50);
}


//PICK PLACE Functions//

void pick_car1()
{
  myStepper3.step(-stepsPerRevolution3*32);//forward motion
  delay(20);
    myStepper1.step(stepsPerRevolution1*2);//forward motion
  delay(20);
  all_stop();
  f_floor();
  delay(20);
  myStepper3.step(stepsPerRevolution3*32);//backward motion
  delay(20);
}
void pick_car2()
{
  myStepper3.step(-stepsPerRevolution3*32);//forward motion
  delay(20);
   myStepper1.step(stepsPerRevolution1*2);//forward motion
  delay(20);
  all_stop();
  s_floor();
  delay(20);
  myStepper3.step(stepsPerRevolution3*32);//backward motion
  delay(20);
}

void place_car1()
{
  myStepper3.step(-stepsPerRevolution3*32);//forward motion
  delay(20);
  all_stop();
  fb_floor();//1floor downward place the car
  delay(20);
  myStepper3.step(stepsPerRevolution3*32);//backward motion
  delay(20);
}
void place_car2()
{
  myStepper3.step(-stepsPerRevolution3*32);//forward motion
  delay(20);
  all_stop();
  sb_floor();//1floor downward place the car
  delay(20);
  myStepper3.step(stepsPerRevolution3*32);//backward motion
  delay(20);
}




//STOP all stepper motors//

void all_stop()
{
  digitalWrite(5,LOW);
  digitalWrite(6,LOW);
  digitalWrite(9,LOW);
  digitalWrite(10,LOW);
  digitalWrite(23,LOW);
  digitalWrite(24,LOW);
  delay(200);
}
