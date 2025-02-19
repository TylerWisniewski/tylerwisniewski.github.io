---
layout: posts
title: "Fast Robots - Lab 2"
permalink: /lab2/
author_profile: true

---
### IMU Setup and Data Analysis

#### Setting Up the IMU
- Connected the IMU to the Artemis board using QWIIC connectors.
- Installed the "SparkFun 9DOF IMU Breakout_ICM 20948_Arduino Library".
- Ran the example code to verify proper setup.

#### AD0_VAL Definition Discussion
- The default setting of 1 worked fine, as the IMU correctly responded to data requests. I tried setting the value to 0 and the IMU stopped working. 

#### Accelerometer Data
- Calculated pitch and roll using the equations:
  - `pitch_a = atan2(accY, accZ) * 180 / π`
  - `roll_a = atan2(accX, accZ) * 180 / π`
- Captured images of output at {-90, 0, 90} degrees.
- Implemented a low-pass filter to smooth noise.

#### Noise in Frequency Spectrum
- Conducted Fourier Transform analysis on accelerometer data.
- Determined a suitable cutoff frequency for noise filtering.

#### Gyroscope Data
- Measured pitch, roll, and yaw.
- Compared complementary filter output to accelerometer results.
- Evaluated drift effects and stability.

#### Data Transmission and Sampling
- Captured and transmitted 5 seconds of IMU data over Bluetooth.
- Optimized sampling rate by removing delays and redundant print statements.

#### Stunt Recording
- Recorded video of RC car movement and analyzed IMU data trends.

![Stunt](/images/portfolio/fast.gif)  
*Tornado!*
---

## Conclusion
- Successfully set up and validated IMU functionality.
- Processed and analyzed accelerometer and gyroscope data.
- Improved data transmission efficiency and stability.

---

