# SpotMicro AI 
**This Project is heavily work in progress and documenting my progress on the goal of making a four-legged robot walk.
It is NOT a working or even finished Project you might want to use! **

![PyBullet Simulation](/Images/SpotMicroAI_pybullet_lidar3.png)

I started this Project because i got inspired by some very smart People/Companies and Projects out there and want to 
understand and adapt their work on my personal wish to create a small "clone" of a "BostonDynamics   SpotMini"-kind-of-looking but self-learning Robot.

Goal of this Project is to
1. build a working physical Robot with cheap components everyone can build
2. create a simulated Environment and be able to control the Robot 
3. to do RL training to make it learn how to stand/walk/run

There are other ways of achiving this, i think. The use of InverseKinematics only, combined with a robust ground detection could also solve the problem and might be "more straight-forward" / yet already very challenging. 

I want to try a combination of both, because i believe that a well trained RL-Model could be move stable and
robust in different situations where the physical robot leaves controlled environments or parts like Servos or Legs become unstable or may even break.

Nevertheless we will have to create a precise IK-Model to be able to have some kind of guided training. 

You can find [my first thoughts on SpotMicro's IK here](https://github.com/FlorianWilk/SpotMicroAI/tree/master/Kinematics). There is also a [Jupyter Notebook explaining the Kinematics](https://github.com/FlorianWilk/SpotMicroAI/tree/master/Kinematics/Kinematic.ipynb) and a [YouTube-Video](https://www.youtube.com/watch?v=VSkqhFok17Q).

## 1. The physical Robot

![SpotMicroAI](/Images/SpotMicroAI_1.jpg)

First of all thanks to Deok-yeon Kim aka KDY0523 who made [this incredible work on Thingiverse](https://www.thingiverse.com/thing:3445283)

This basically is the physical Robot. It will take some Days to print and assemble all the Parts, but it's worth all the effort. I also sanded, primed and painted all the Parts to give it a nicer Look.

[Here is my Thingiverse-Make](https://www.thingiverse.com/make:654812)

Since my setup required some additional Hardware, i recreated some parts using FreeCAD - see /Parts-Directory.

![Parts](/Images/SpotMicroAI_FreeCad.png)

### NVIDIA Jetson Nano

To have some Protection for the NVIDIA Jetson Nano i printed [this Case from Thingiverse](https://www.thingiverse.com/thing:3603594).

![JetsonNano-Case](/Images/jetsonNanoCase.jpg)

The Jetson Nano is connected to a 16 Channel PCA9685 I2C-Servo Driver which controls all the Servos. 
I will also try to connect the I2C OLED Display and the LED-Circle to it.

[Here you can find the all the Code for the Jetson Nano](/JetsonNano)

### Sensors

Since i am not a robotics company, i can't easily invest some hundreds of Euros/Dollars. So i will use Sonar Sensors instead of visual sensors like RGB or RGBD-Cams. Maybe i will try with ESPEyes or something in the near future.

Sensors used:
- 4 x HC-SR04-Sensors. 2x as in the original model in the front looking forward/down. 2x at the bottom (front/back) looking down to measure the ground-distance. 
- An IMU MPU-6050 is used to measure pitch,roll and velocities. Yaw will be ignored since it drifts quickly. 

Also i have a SSD1306 OLED-Display and a NeoMatrix LED-Circle i want to include for the Style.
In a first version i used an Arduino Mega as kind of Servo/Sensor-Controller and a Raspberry PI as Locomotion-Controller (communication via UART). But it showed up that the Arduino is too slow to handle Sensor-Signals and Servo-PWM properly at the same time. 

I am not sure if the Hardware i use now will be enough to finally have a very smooth walking Robot like for Example the real SpotMini. See this more as a Research-Project where I try to use cheap Hardware and other People's Work to learn more about how this all works. 

## 2. Simulation

I try to implement the Ideas of [this Paper](https://arxiv.org/pdf/1804.10332.pdf) by
Jie Tan, Tingnan Zhang, Erwin Coumans, Atil Iscen, Yunfei Bai, Danijar Hafner, Steven Bohez, and Vincent Vanhoucke
Google Brain,Google DeepMind

Here you can see the first version of the URDF-Model.

![urdf](/Images/SpotMicroAI_urdf2.png)

And here the Model with working Kinematics in a PyBullet-Simulation.

![PyBullet](/Images/SpotMicroAI_stairs.png)

The URDF Model is very basic and work in progress. Masses and Inertias are guesses and not correct. I will have to disassemble the Robot to have correct weights. 

You can find a [first Video on YouTube](https://www.youtube.com/watch?v=VSkqhFok17Q).

This example can be found in the Repository. You need a GamePad for this to work:
```
pip3 install numpy
pip3 install pybullet
pip3 install inputs

cd Core/
python3 example_gamepad.py
```

## 3. Training

There is no real Training-Code yet. I am still unsure about the optimal Architecture.

## Credits and thanks

- Deok-yeon Kim creator of SpotMicro
- Boston Dynamics who built this incredible SpotMini,
- Ivan Krasin - https://ivankrasin.com/about/ - thanks for inspiration and chatting
- Jie Tan, Tingnan Zhang, Erwin Coumans, Atil Iscen, Yunfei Bai, Danijar Hafner, Steven Bohez, and Vincent Vanhoucke
Google Brain,Google DeepMind 

