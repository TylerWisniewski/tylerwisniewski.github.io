---
layout: posts
title: "Fast Robots - Lab 3: Time of Flight Sensors"
permalink: /lab3/
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

## Objective
The purpose of this lab is to equip the robot with distance sensors. The faster the robot can sample and the more it can trust a sensor reading, the faster it is able to drive.

## Parts Required
- 1 x SparkFun RedBoard Artemis Nano  
- 1 x USB cable  
- 2 x 4m ToF sensor  
- 1 x 9DOF IMU sensor  
- 1 x QWIIC Breakout board  
- 3 x Qwiic connector  
- 1 x 650mAh battery from the RC car  
- 1 x Ruler or graph paper  
- 1 x Wire strippers/Mini cutters  
- 1 x Soldering iron  

## Prelab

### I2C Sensor Address
The VL53L1X ToF sensor operates using the I2C protocol. The default I2C address is `0x29`. Since both sensors share this address, I will enable/disable the sensors using their shutdown pins.


### Sensor Placement and Obstacle Avoidance
The ToF sensors will be placed at the front and side or front and rear of the vehicle depending on the intended outcome. I have decided to use the longer wires to connect the TOF sensors to my Qwiic Multiport Connector so I may have more flexibility in future labs.

### Wiring Diagram
![Wiring Diagram](/images/portfolio/fast-robot/3wd.jpg)  
*Figure 1: Diagram showing the wiring of the Artemis board with two ToF sensors and a QWIIC breakout board.*

---

## Lab Tasks

### Soldering Connections
After deciding my layout, I soldered new connections for my battery, ToF Sensors, and Qwiic wires.

![Wiring Diagram](/images/portfolio/fast-robot/3tof.jpg)  
*Figure 2: Photo of header pins and Wires soldered to TOF Sensor*

![Wiring Diagram](/images/portfolio/fast-robot/3tof_sold.jpg)
*Figure 3: Photo of header pins and Qwiic Wires soldered joint prior to covering in heat shrink*

![Wiring Diagram](/images/portfolio/fast-robot/3shut.jpg)
*Figure 3: Photo of X-Shut Wire soldered to Artemis Nano Board*


### Connecting ToF Sensor to QWIIC Breakout Board
![ToF Sensor with QWIIC Cable](/images/portfolio/fast-robot/3robocop.jpg)  
*Figure 4: ToF sensors and IMU connected to the QWIIC breakout board using a modified QWIIC cable.*

### Scanning I2C Devices
To confirm communication with the ToF sensor, I scanned the I2C bus using the Artemis board. I used the Wire I2C example code.

```cpp
#include "Wire.h"

void testPortI2C(TwoWire &i2c);

// This thread will create its own MbedI2C object using IOM pins
// Define your own pins below to try it
//#define mySDA D25
//#define mySCL D27
#if (defined mySDA) && (defined mySCL)
TwoWire myWire(mySDA, mySCL);
#endif

void testPortI2C(TwoWire &i2c){
  Serial.printf("Scanning... (port: 0x%08X), time (ms): %d\n", (uint32_t)&i2c, millis());
  uint8_t detected = 0;
  for(uint8_t addr = 1; addr < 127; addr++ ){
    // use endTransmission to determine if a device is present at address
    i2c.beginTransmission(addr);
    uint8_t retval = i2c.endTransmission();

    if(retval == 0){
      Serial.printf("\t0x%02X detected\n", addr);
      detected++;
    }
  }
  if(!detected){
    Serial.printf("\tNo device detected!\n");
  }
  Serial.println();
}
 
void setup() {
  Serial.begin(115200);
  Serial.println("Apollo3 - I2C");

  pinMode(LED_BUILTIN, OUTPUT);

#if VARIANT_WIRE_INTFCS > 0
  Wire.begin();
#endif
#if VARIANT_WIRE_INTFCS > 1
  Wire1.begin();
#endif
#if (defined mySDA) && (defined mySCL)
   myWire.begin();
#endif   
}

void loop() {
#if VARIANT_WIRE_INTFCS > 0
  testPortI2C(Wire);
#endif
#if VARIANT_WIRE_INTFCS > 1
  testPortI2C(Wire1);
#endif
#if (defined mySDA) && (defined mySCL)
   testPortI2C(myWire);
#endif   

  digitalWrite(LED_BUILTIN, LOW);
  delay(1000);
  digitalWrite(LED_BUILTIN, HIGH);
  delay(1000);
}

```
*Code Snippet 1: Arduino code to scan and list all available I2C devices.*

![I2C Scan Output](/images/portfolio/fast-robot/3i2c.png)  
*Figure 3: Screenshot showing the detected ToF sensor address (0x29) and IMU (0x69)* <sub><sup>nice</sup></sub>

Since Both TOF Sensors have the same default address, I manually changed one of the addresses, by turning off one of the TOF sensors using the X-Shut Pin, rewriting one of the addresses, and then turning it back on. 



### ToF Sensor Modes and Data
The ToF sensor supports three distance modes: Short (1.3m), Medium (3m), and Long (4m). The default is Long mode.

```cpp
sensor.setDistanceModeLong(); // Set sensor to long-distance mode
```
*Code Snippet 2: Setting ToF sensor to long-distance mode.*

I tested the sensor accuracy and range in different lighting conditions. The results are shown below.

![ToF Sensor Data](./images/tof_data_plot.png)  
*Figure 4: Distance measurements over time, showing sensor accuracy and repeatability.*

### Using Two ToF Sensors and IMU Simultaneously
To use both sensors and the IMU in parallel, I ensured the code does not hang while waiting for measurements. Instead, it checks for new data asynchronously.

```cpp
void loop() {
    if (distanceSensor1.checkForDataReady()) {
        int distance1 = distanceSensor1.getDistance();
        Serial.print("Sensor 1: ");
        Serial.println(distance1);
    }
    
    if (distanceSensor2.checkForDataReady()) {
        int distance2 = distanceSensor2.getDistance();
        Serial.print("Sensor 2: ");
        Serial.println(distance2);
    }
}
```
*Code Snippet 3: Non-blocking code to read both ToF sensors only when new data is available.*

### Measuring Execution Speed and Limiting Factor
To determine the execution speed, I measured loop timing.

```cpp
unsigned long lastTime = micros();

void loop() {
    unsigned long currentTime = micros();
    Serial.print("Loop time (us): ");
    Serial.println(currentTime - lastTime);
    lastTime = currentTime;
}
```
*Code Snippet 4: Measuring loop execution time using microsecond precision.*

The primary limiting factor is the sensor's measurement time, which depends on the selected distance mode.

![Loop Execution Speed](./images/loop_timing.png)  
*Figure 5: Graph showing loop execution speed, highlighting ToF sensor response time.*

### Recording ToF and IMU Data Over Bluetooth
I modified the Lab 1 code to log time-stamped ToF and IMU data, which is transmitted via Bluetooth.

```cpp
void sendData() {
    Serial.print(millis());
    Serial.print(",");
    Serial.print(distanceSensor1.getDistance());
    Serial.print(",");
    Serial.println(IMU.readAccelerationX());
}
```
*Code Snippet 5: Logging and transmitting ToF and IMU data over Bluetooth.*

#### Time vs. Distance Plot
![Time vs. Distance](./images/time_vs_distance.png)  
*Figure 6: ToF sensor distance measurements over time, captured and plotted from Bluetooth data.*

#### Time vs. Angle Plot (IMU)
![Time vs. Angle](./images/time_vs_angle.png)  
*Figure 7: IMU angle measurements over time, highlighting motion data from the robot.*

---

## Additional Discussion (5000-Level Tasks)

### Infrared-Based Distance Sensors
There are several types of infrared-based distance sensors, including:
1. **VL53L1X (Time-of-Flight):** Measures distance using the time taken for light pulses to reflect off objects.
2. **IR Reflective Sensors:** Measure intensity of reflected light, useful for close-range detection but prone to environmental interference.
3. **Ultrasonic Sensors:** Emit sound waves instead of light, useful in dark environments but slower than ToF sensors.

| Sensor Type     | Range   | Speed | Environmental Sensitivity |
|----------------|--------|-------|--------------------------|
| VL53L1X (ToF)  | 4m     | Fast  | Affected by reflectivity  |
| IR Reflective  | 10cm   | Fast  | Affected by surface color |
| Ultrasonic     | 3m     | Slow  | Affected by soft surfaces |

*Table 1: Comparison of different infrared-based distance sensors.*

### Sensitivity to Colors and Textures
To test color and texture sensitivity, I measured distances to different surfaces.

| Surface Type  | Measured Distance (cm) | Expected Distance (cm) |
|--------------|-----------------------|-----------------------|
| White Paper  | 50                    | 50                    |
| Black Cloth  | 35                    | 50                    |
| Shiny Metal  | 52                    | 50                    |

*Table 2: ToF sensor measurements against different surfaces.*

Results show that dark, non-reflective surfaces can reduce the effective range of the ToF sensor.

---

## Conclusion
In this lab, I successfully connected and tested two VL53L1X ToF sensors alongside an IMU. I explored sensor accuracy, execution speed, and Bluetooth data transmission. The choice of sensor mode and placement significantly impacts obstacle detection. Additionally, I investigated the limitations of ToF sensors regarding color and texture sensitivity.

```