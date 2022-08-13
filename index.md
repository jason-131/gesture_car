# Gesture Controlled Car
 ---
The gesture controlled car uses an arduino micocontroller to control the direction the car drives in. The car communnicates with the remote using the HC05 bluetooth module. The remote uses an gyroscope to determine the angle the remote is held at, which lets the user control the car by tilting the remote in the direction they want it to go. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Jason C. | Monta Vista High School | Computer Engineering, Computer Science | Rising Junior

![unnamed](https://user-images.githubusercontent.com/30334711/184504052-85eefbe5-d281-48e9-9046-b60ec97c55d4.jpg)

<img src="https://user-images.githubusercontent.com/30334711/184504052-85eefbe5-d281-48e9-9046-b60ec97c55d4.jpg" width="48">

# Project Picture
---
![image](https://user-images.githubusercontent.com/30334711/183264182-da81ecbe-636f-4730-b22f-13f3f1f667a8.png)
  
# Final Milestone
---
My final milestone was to solder all of the components of the remote onto the perfboard. I had already gotten the remote to work with the car, now I just had to solder the components on to the perfboard so it's more compact. Soldering was a bit challenging at first as this was the first time I did it. Eventually I got the hang of it and I ended up finishing relatively quickly. 

[![Milestone 3](https://res.cloudinary.com/marcomontalbano/image/upload/v1660322472/video_to_markdown/images/youtube--jLW0eKtfv50-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=jLW0eKtfv50 "Milestone 3")

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
---
[Transmitter Code.zip](https://github.com/jason-131/jason_BSE_portfolio/files/9310776/Transmitter.Code-20220731T234147Z-001.zip)

[Car Code.zip](https://github.com/jason-131/jason_BSE_portfolio/files/9310790/car.zip)


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
