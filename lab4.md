---
layout: posts
title: "Fast Robots - Lab 4: Motor Drivers & Open Loop Control "
permalink: /lab4/
author_profile: true
gallery:
  - url: /lab1/
    image_path: /images/portfolio/fast-robot/lab1.png
    alt: "cev image"
    title: "Lab 1 A/B"
  - url: /lab2/
    image_path: /images/portfolio/fast.gif
    alt: "placeholder image 2"
    title: "Image 2 title caption"
  - url: /lab3/
    image_path: /images/portfolio/fin.png
    alt: "placeholder image 3"
    title: "Image 3 title caption"
---


## Prelab  

### Motor Driver Wiring  
I began by wiring the H-Bridge motor drivers according to the datasheet, ensuring each pin was correctly connected. My previous wiring diagram from Lab 3 contained an error—it used the VMM pins, which do not serve a functional purpose. Below is the corrected wiring setup:

- **Ground Pins**: 1 to Artemis, 1 to negative battery lead  
- **Vin**: Connected to the positive battery lead  
- **AIN1 & BIN1**: Linked in series, then connected to Artemis pins **0 and 4**  
- **AIN2 & BIN2**: Linked in series, then connected to Artemis pins **1 and 5**  
- **AOUT1 & BOUT1**: Linked in series, then connected to the **positive motor lead**  
- **AOUT2 & BOUT2**: Linked in series, then connected to the **negative motor lead**  

The A and B input/output pins were coupled to provide sufficient current to the motors. Each board powers one motor, which in turn drives two wheels. Without daisy-chaining the pins, the current would be halved, reducing motor performance.  

**Circuit Diagram:**  
![Corrected Motor Driver Wiring](path/to/circuit-diagram.png)  
*Figure 1: Updated motor driver wiring diagram*  

### Battery Discussion  
Each motor driver and the Artemis are powered separately by **650 mAh batteries**. The motors require significantly more current than the Artemis, and separate power paths reduce signal noise.  

---

## PWM Tests  

Before soldering, I tested the motor drivers to verify bidirectional functionality. Applying **HIGH to one input and LOW to the other** should drive the motor in either direction.  

I supplied **3.7V** to the drivers, although they accept up to **10V**. The OUT1 and OUT2 pins were probed using an oscilloscope while running a PWM loop. I referenced code from **Bhadra Bejoy (2023)** and **Robby Huang (2022)** to increment the PWM from **0 to 255**.  

```cpp
void setup() {
    pinMode(4, OUTPUT);
    pinMode(5, OUTPUT);
    pinMode(0, OUTPUT);
    pinMode(1, OUTPUT);
}

void loop() {
    analogWrite(4, 0);
    for (int i = 0; i < 255; i++) {
        analogWrite(4, i);
        analogWrite(5, 0);
        analogWrite(0, 255 - i);
        analogWrite(1, 0);
        delay(10);
    }
}
```
*Figure 2: PWM incrementing test code*  

---

## Testing Individual Motors  

After soldering, I confirmed that each motor functioned properly by applying power and running the following test script:  

```cpp
void setup() {
    pinMode(4, OUTPUT);
    pinMode(5, OUTPUT);
    pinMode(0, OUTPUT);
    pinMode(1, OUTPUT);
}

void loop() {
    analogWrite(4, 0);
    analogWrite(5, 200);
    delay(5000);
    analogWrite(4, 0);
    analogWrite(5, 0);
}
```
*Figure 3: Motor verification code*  

**Observation:**  
- The robot required **40-45 PWM** to overcome static friction with a fresh battery.  
- A worn battery increased the minimum required PWM to **50-65**.  
- **Turning required ~70 PWM** to start from rest.  

---

## Open Loop Control  

### Installation  
I integrated all components, ensuring **braided cables** to reduce noise and securing wires with zip ties. I mounted **one TOF sensor in the front and one in the back**, allowing 360° coverage with a **180° rotation**.  

![Robot Wiring](path/to/robot-wiring.png)  
*Figure 4: Fully wired robot*  

### System Drift  
To evaluate drift, I ran the robot **forward at 120 PWM for ~3 seconds**. The expected distance was **6ft (2m)**, but the robot veered **right by ~1ft** at the **4.5-5ft mark**.  

### Adding a Calibration Constant  
To compensate for drift, I introduced a **calibration factor** to adjust the left motor's PWM. I manually tuned this value since using the IMU would make the system **closed-loop**. The final calibration factor was **1.6**, which significantly reduced drift.  

```cpp
#define motorR1 0
#define motorR2 1
#define motorL1 4
#define motorL2 5
#define calibFactor 1.6

void setup() {
    pinMode(motorR1, OUTPUT);
    pinMode(motorR2, OUTPUT);
    pinMode(motorL1, OUTPUT);
    pinMode(motorL2, OUTPUT);
}

void loop() {
    driveStraightCalib(1, 3000, 100);
}

void driveStraightCalib(int dir, int time, int pwm) {
    if (dir == 1) {
        analogWrite(motorR1, 0);
        analogWrite(motorR2, pwm);
        analogWrite(motorL1, pwm * calibFactor);
        analogWrite(motorL2, 0);
    }
    delay(time);
}
```
*Figure 5: Calibration code to correct drift*  

---

<!-- ## Challenges & Debugging  

### Pin 5 Issue  
One motor **stopped and stuttered** after **1 second** despite a 3-second command. I traced this to **Pin 5’s solder joint**, which had a **small gap** preventing a reliable connection.  

**Debugging Steps:**  
- Checked connections with a multimeter  
- Tested pins in isolation  
- Rewired motor drivers for reduced interference  
- Reheated solder joints to improve connections  

After fixing the solder, **the problem was resolved**.   -->

---

## Discussion  

This lab provided hands-on experience in **motor control, soldering, and debugging**. I initially underestimated open-loop control but faced challenges such as **drift compensation and inconsistent motor behavior**. Future improvements include **PID control for better precision**.  
