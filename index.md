# Gesture Controlled Car
 ---
The gesture controlled car uses an arduino micocontroller to control the direction the car drives in. The car communnicates with the remote using the HC05 bluetooth module. The remote uses an gyroscope to determine the angle the remote is held at, which lets the user control the car by tilting the remote in the direction they want it to go. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Jason C. | Monta Vista High School | Computer Engineering, Computer Science | Rising Junior

# Project Picture
---
![image](https://user-images.githubusercontent.com/30334711/183264182-da81ecbe-636f-4730-b22f-13f3f1f667a8.png)
  
# Final Milestone
---
My final milestone is the increased reliability and accuracy of my robot. I ameliorated the sagging and fixed the reliability of the finger. As discussed in my second milestone, the arm sags because of weight. I put in a block of wood at the base to hold up the upper arm; this has reverberating positive effects throughout the arm. I also realized that the forearm was getting disconnected from the elbow servo’s horn because of the weight stress on the joint. Now, I make sure to constantly tighten the screws at that joint. 

[![Final Milestone](https://res.cloudinary.com/marcomontalbano/image/upload/v1612573869/video_to_markdown/images/youtube--F7M7imOVGug-c05b58ac6eb4c4700831b2b3070cd403.jpg )](https://www.youtube.com/watch?v=F7M7imOVGug&feature=emb_logo "Final Milestone"){:target="_blank" rel="noopener"}

# Second Milestone
---
For my second milestone I got the remote to actualy control the car. This was a big milestone as I had to do many things at once like getting the bluetooth working, the remote working, and writing all the code. For the remote I wired everything using the breadboard as it is much easier to use, and if I made any mistakes I could unplug the wires. Eventually I wanted to solder the remote onto a perf board but I used the breadboard as a prototype. For the remote I got the gyroscope to work which lets the user control the car by tilting it in the direction you want it to go. I also had to get the bluetooth working so the remote and the car could communicate with each other wirelessly. 

[![Milestone 2](https://res.cloudinary.com/marcomontalbano/image/upload/v1660204386/video_to_markdown/images/youtube--4Ifjy3sYAq4-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=4Ifjy3sYAq4 "Milestone 2")
# First Milestone
  ---
My first milestone was setting up the Arduino Uno, the motor driver, and the motors. To do this I screwed all the motors on to the chassis, then I soldered wires onto the motors so I could connect them to the motor driver. The motor driver is the component that directly controls the motors by sending electrity to it. On the other hand, the arduino is the component that sends commands to the motor driver so it knows which direction to spin the motors. Initially I try to power the motor driver and the arduino with 9v batteries but I end up using a battery pack to power the motor driver because the 9v battery isn't powerful enough. To turn the wheels I wrote some basic code that turns the wheels forwards then backwards. 
[![Milestone 1](https://res.cloudinary.com/marcomontalbano/image/upload/v1659716794/video_to_markdown/images/youtube--rDIB7zLfi5A-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://youtu.be/rDIB7zLfi5A "Milestone 1"){:target="_blank" rel="noopener"}

# Schematics 
---
## Remote Unit
![image](https://user-images.githubusercontent.com/30334711/183838571-bb3990fb-723e-4f79-a0b6-ecf31166aef1.png)

## Car Unit 
![image](https://user-images.githubusercontent.com/30334711/183839426-8d9206e7-d2e4-4454-b1cf-cfd2f716b9d2.png)

# Code

<details>
 <summary> # Remote Unit </summary>
 
 ```java
 
 // I2Cdev and MPU6050 must be installed as libraries, or else the .cpp/.h files
// for both classes must be in the include path of your project
#include "I2Cdev.h"
#include "MPU6050_6Axis_MotionApps20.h"
// Arduino Wire library is required if I2Cdev I2CDEV_ARDUINO_WIRE implementationis used in I2Cdev.h
#include "Wire.h"

#include <SoftwareSerial.h>
SoftwareSerial BTSerial(10, 11); // CONNECT BT RX PIN TO ARDUINO 11 PIN | CONNECT BT TX PIN TO ARDUINO 10 PIN

#define OUTPUT_READABLE_YAWPITCHROLL
#define INTERRUPT_PIN 7  // use pin 2 on Arduino Uno & most boards
#define LED_PIN 13 // (Arduino is 13, Teensy is 11, Teensy++ is 6)

MPU6050 mpu;
bool blinkState = false;

// MPU control/status vars
bool dmpReady = false;  // set true if DMP init was successful
uint8_t mpuIntStatus;   // holds actual interrupt status byte from MPU
uint8_t devStatus;      // return status after each device operation (0 = success, !0 = error)
uint16_t packetSize;    // expected DMP packet size (default is 42 bytes)
uint16_t fifoCount;     // count of all bytes currently in FIFO
uint8_t fifoBuffer[64]; // FIFO storage buffer

// orientation/motion vars
Quaternion q;           // [w, x, y, z]         quaternion container
VectorFloat gravity;    // [x, y, z]            gravity vector
float ypr[3];           // [yaw, pitch, roll]   yaw/pitch/roll container and gravity vector
float pitch = 0;
float roll = 0;
float yaw = 0;
int x;
int y;

// ================================================================
// ===               INTERRUPT DETECTION ROUTINE                ===
// ================================================================

volatile bool mpuInterrupt = false;     // indicates whether MPU interrupt pin has gone high
void dmpDataReady() {
  mpuInterrupt = true;
}



// ================================================================
// ===                      INITIAL SETUP                       ===
// ================================================================

void setup() {
  // join I2C bus (I2Cdev library doesn't do this automatically)
#if I2CDEV_IMPLEMENTATION == I2CDEV_ARDUINO_WIRE
  Wire.begin();
  Wire.setClock(400000); // 400kHz I2C clock. Comment this line if having compilation difficulties
#elif I2CDEV_IMPLEMENTATION == I2CDEV_BUILTIN_FASTWIRE
  Fastwire::setup(400, true);
#endif

  // initialize serial communication
  // (115200 chosen because it is required for Teapot Demo output, but it's
  // really up to you depending on your project)
  Serial.begin(115200);
  BTSerial.begin(38400);  // HC-05 default speed in AT command more
  
//  b while (!Serial); // wait for Leonardo enumeration, others continue immediately

  // NOTE: 8MHz or slower host processors, like the Teensy @ 3.3V or Arduino
  // Pro Mini running at 3.3V, cannot handle this baud rate reliably due to
  // the baud timing being too misaligned with processor ticks. You must use
  // 38400 or slower in these cases, or use some kind of external separate
  // crystal solution for the UART timer.

  // initialize device
  Serial.println(F("Initializing I2C devices..."));
  mpu.initialize();
  pinMode(INTERRUPT_PIN, INPUT);

  // verify connection
  Serial.println(F("Testing device connections..."));
  Serial.println(mpu.testConnection() ? F("MPU6050 connection successful") : F("MPU6050 connection failed"));

  // load and configure the DMP
  Serial.println(F("Initializing DMP..."));
  devStatus = mpu.dmpInitialize();

  // supply your own gyro offsets here, scaled for min sensitivity
  mpu.setXGyroOffset(126);
  mpu.setYGyroOffset(57);
  mpu.setZGyroOffset(-69);
  mpu.setZAccelOffset(1869); // 1688 factory default for my test chip

  // make sure it worked (returns 0 if so)
  if (devStatus == 0) {
    // turn on the DMP, now that it's ready
    Serial.println(F("Enabling DMP..."));
    mpu.setDMPEnabled(true);

    // enable Arduino interrupt detection
    Serial.println(F("Enabling interrupt detection (Arduino external interrupt 0)..."));
    attachInterrupt(digitalPinToInterrupt(INTERRUPT_PIN), dmpDataReady, RISING);
    mpuIntStatus = mpu.getIntStatus();

    // set our DMP Ready flag so the main loop() function knows it's okay to use it
    Serial.println(F("DMP ready! Waiting for first interrupt..."));
    dmpReady = true;

    // get expected DMP packet size for later comparison
    packetSize = mpu.dmpGetFIFOPacketSize();
  } else {
    // ERROR!
    // 1 = initial memory load failed
    // 2 = DMP configuration updates failed
    // (if it's going to break, usually the code will be 1)
    Serial.print(F("DMP Initialization failed (code "));
    Serial.print(devStatus);
    Serial.println(F(")"));
  }

  // configure LED for output
  pinMode(LED_PIN, OUTPUT);
}



// ================================================================
// ===                    MAIN PROGRAM LOOP                     ===
// ================================================================

void loop() {
  // if programming failed, don't try to do anything
  if (!dmpReady) return;

  // wait for MPU interrupt or extra packet(s) available
  while (!mpuInterrupt && fifoCount < packetSize) {
    // other program behavior stuff here
    // .
    // .
    // .
    // if you are really paranoid you can frequently test in between other
    // stuff to see if mpuInterrupt is true, and if so, "break;" from the
    // while() loop to immediately process the MPU data
    // .
    // .
    // .
  }

  // reset interrupt flag and get INT_STATUS byte
  mpuInterrupt = false;
  mpuIntStatus = mpu.getIntStatus();

  // get current FIFO count
  fifoCount = mpu.getFIFOCount();

  // check for overflow (this should never happen unless our code is too inefficient)
  if ((mpuIntStatus & 0x10) || fifoCount == 1024) {
    // reset so we can continue cleanly
    mpu.resetFIFO();
    Serial.println(F("FIFO overflow!"));

    // otherwise, check for DMP data ready interrupt (this should happen frequently)
  } else if (mpuIntStatus & 0x02) {
    // wait for correct available data length, should be a VERY short wait
    while (fifoCount < packetSize) fifoCount = mpu.getFIFOCount();

    // read a packet from FIFO
    mpu.getFIFOBytes(fifoBuffer, packetSize);

    // track FIFO count here in case there is > 1 packet available
    // (this lets us immediately read more without waiting for an interrupt)
    fifoCount -= packetSize;

#ifdef OUTPUT_READABLE_YAWPITCHROLL
    // display Euler angles in degrees
    mpu.dmpGetQuaternion(&q, fifoBuffer);
    mpu.dmpGetGravity(&gravity, &q);
    mpu.dmpGetYawPitchRoll(ypr, &q, &gravity);

    yaw = ypr[0] * 180 / M_PI;
    pitch = ypr[1] * 180 / M_PI;
    roll = ypr[2] * 180 / M_PI;

    if (roll > -100 && roll < 100)
      x = map (roll, -100, 100, 0, 100);

    if (pitch > -100 && pitch < 100)
      y = map (pitch, -100, 100, 100, 200);

//    Serial.print(x);
//    Serial.print("\t");
//    Serial.println(y);

    if((x>=45 && x<=55) && (y>=145 && y <=155)){
      BTSerial.write('S');
      //Serial.println('S');
    }else if(x>60){
      BTSerial.write('R');
      //Serial.println('R');
    }else if(x<40){
      BTSerial.write('L');
    //  Serial.println('L');
    }else if(y>160){
      BTSerial.write('B');
     // Serial.println('B');
    }else if(y<140){
      BTSerial.write('F');
    //  Serial.println('F');
    }
#endif

    // blink LED to indicate activity
    blinkState = !blinkState;
    digitalWrite(LED_PIN, blinkState);
  }
}
 ```
</details>
 
<details>
 <summary> Car Unit </summary>
 
``` java
int lm1=2; //left motor output 1
int lm2=4; //left motor output 2
int rm1=7;  //right motor output 1
int rm2=8;  //right motor output 2

 char d=0;

void setup()
{
  pinMode(lm1,OUTPUT);
  pinMode(lm2,OUTPUT);
  pinMode(rm1,OUTPUT);
  pinMode(rm2,OUTPUT);
  Serial.begin(38400);
 sTOP();
}
void loop()
{
  if(Serial.available()>0)
  {
    d=Serial.read();
  if(d=='F')
  {
   ForWard();
    }
    if(d=='B')
  {
   BackWard();
    }
    
  if(d=='L')
  {
   Left();
    }
  if(d=='R')
  { 
  Right();
   }
     if(d=='S')
  {
   sTOP();
    }
}
}

 void ForWard()
  {
   digitalWrite(lm1,HIGH);
   digitalWrite(lm2,LOW);
   digitalWrite(rm1,HIGH);
   digitalWrite(rm2,LOW);
  } 
  void BackWard()
  {
   digitalWrite(lm1,LOW);
   digitalWrite(lm2,HIGH);
   digitalWrite(rm1,LOW);
   digitalWrite(rm2,HIGH);
  }
  void Left()
  {
   digitalWrite(lm1,LOW);
   digitalWrite(lm2,HIGH);
   digitalWrite(rm1,HIGH);
   digitalWrite(rm2,LOW);
  } 
  void Right()
  {
   digitalWrite(lm1,HIGH);
   digitalWrite(lm2,LOW);
   digitalWrite(rm1,LOW);
   digitalWrite(rm2,HIGH);
  }  

    void sTOP()
  {
   digitalWrite(lm1,LOW);
   digitalWrite(lm2,LOW);
   digitalWrite(rm1,LOW);
   digitalWrite(rm2,LOW);
  }  
```
</details>

# Bill of Materials 
---

|    | Part Name              | Quanity | Total Cost |
| -- | ---------------------- | ------- | ---------- |
| 1  | Arduino Uno            | 1       | $22.49     |
| 2  | Arduino Micro          | 1       | $24.99     |
| 3  | HC-05 Bluetooth Module | 2       | $13.49     |
| 4  | L298N Motor Driver     | 1       | $7.00      |
| 5  | MPU-6050               | 1       | $5.79      |
| 6  | 9v Battery             | 2       | $7.00      |
| 7  | 9v to DC Jack          | 2       | $5.00      |
| 8  | 4 AA Battery Holder    | 1       | $5.00      |
| 9  | 4 Wheel Car Chassis    | 1       | $17.00     |
| 10 | Jumper Wires           | 30      | $3.50      |
| 11 | Perf Board             | 1       | $1.65      |
