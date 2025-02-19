---
layout: posts
title: "Fast Robots - Lab 3 TOF"
permalink: /lab3/
author_profile: true
---

# WIP DONT GRADE THIS YET!!!!
## Purpose
The purpose of this lab was to configure our TOF (Time-of-Flight) sensors, which are intended for measuring distances. We also began soldering components to the Artemis and started planning our future wiring.

---

## Prelab
Before beginning the lab, we first considered how we would wire the entire robot. This included the IMU, two TOF sensors, and two motor drivers alongside the batteries and QWIIC connect. A wiring diagram is shown below.

### TOF Sensor Addressing
By default, the two TOF sensors have the same I2C address. This means that unless this address is artificially changed, even if both sensors are wired to the QWIIC connect, only one will measure data. To resolve this, we will be using the XSHUT pin on one of the sensors to configure our `setup()` function accordingly. The plan is:
- Record the default address of the first sensor.
- Shut down power to it.
- Manually change its address.
- Initialize the second sensor with a unique address.

The first default address is shown below and represents the number 84 but bit-shifted.

### TOF Sensor Placement
For sensor placement, it would be useful to position the sensors perpendicular to each other—one at the front and one on either the right or left side. This will aid in path planning and object detection by detecting obstacles both in front and to the side of the robot.

---

## Soldering
We soldered:
- A connector to the battery.
- QWIIC connect wires to the TOF sensors.
- The XShutdown pin only on one of the sensors, as only one address needs to change.

The longer wires were chosen for the TOF sensors, as the IMU’s exact position is less critical.

---

## XShutdown & Configuring Two TOFs
As discussed in the pre-lab, only one XShut pin was soldered and wired to pin 2 on the Artemis. By switching between HIGH and LOW states, we could turn the first TOF off, change its address, and then turn it back on.

### Code Implementation
```cpp
pinMode(SHUTDOWN_PIN, OUTPUT);
digitalWrite(SHUTDOWN_PIN, 0);

Wire.begin();

distanceSensor1.init();

// Shut down first sensor, change address
distanceSensor1.setI2CAddress(0x54);

if (distanceSensor1.begin() != 0) {
    Serial.println("First sensor failed to begin...");
    while (1);
}
Serial.println("Sensor 1 online!");

digitalWrite(SHUTDOWN_PIN, 1);

if (distanceSensor2.begin() != 0) {
    Serial.println("Second sensor failed to begin...");
    while (1);
}
Serial.println("Sensor 2 online!");

distanceSensor1.setDistanceModeShort();
distanceSensor2.setDistanceModeShort();
```

---

## Sensor Precision and Accuracy
With the sensors configured, we tested their precision and accuracy by:
- Setting the distance mode to short (max 1.3 meters, 50 Hz sampling rate).
- Taping the sensors to a laptop.
- Using a tape measure to set known distances from a wall.
- Recording 100 data points per distance.

### Data Collection Code
```cpp
distanceSensor1.startRanging();
distanceSensor2.startRanging();

clear();

int startT = millis();
int i = 0, j = 0;
while (i < NUM_DATA_POINTS && j < NUM_DATA_POINTS) {
    time_data[i] = millis();
    Serial.println(time_data[i]);
    if (distanceSensor1.checkForDataReady()) {
        distance1_data[i] = distanceSensor1.getDistance();
        i++;
    }
    if (distanceSensor2.checkForDataReady()) {
        distance2_data[j] = distanceSensor2.getDistance();
        j++;
    }
}
```

### Observations
- At 24 inches (2 feet), both sensors had high standard deviation (~10 mm).
- At distances under 1-2 inches, data was unreliable. One TOF sensor consistently read 0 mm under 1 inch.
- Sensors showed a few mm offset from each other, likely due to the experimental setup.

### Adjusting Ranging Time Budget
Using `distanceSensor.setTimingBudgetInMs(x)`, we tested 20, 66, and 100 ms. The smallest standard deviation occurred at 100 ms for short distances (~6 inches), but accuracy decreased. Experimental flaws included:
- A darker testing environment.
- Unstable sensor positioning (hand-held).

---

## Speed and Robustness

### Collecting Data Over Time
Following previous labs, we collected and sent data over 5 seconds. The main difference was checking for available sensor data before reading, as sampling occurs at 50 Hz.

### Notification Handler for Bluetooth Transmission
```python
def notif_handler(uuid, notif):
    global time_data, dist_1, dist_2
    s = ble.bytearray_to_string(notif)
    sep_notif = s.split(" | ")
    time_data.append(float(sep_notif[1]))
    dist_1.append(float(sep_notif[2]))
    dist_2.append(float(sep_notif[3]))
```

### Speed Optimization
We optimized data collection by:
- Moving `.startRanging()` to `setup()` to avoid unnecessary calls.
- Counting loop iterations between readings.

```c
int start = millis();
Serial.print("T:");
Serial.print(start);

if (distanceSensor1.checkForDataReady()) {
    int distance1 = distanceSensor1.getDistance();
    Serial.print(" | Distance 1 (mm): ");
    Serial.print(distance1);
}
if (distanceSensor2.checkForDataReady()) {
    int distance2 = distanceSensor2.getDistance();
    Serial.print(" | Distance 2 (mm): ");
    Serial.print(distance2);
}
int end = millis();
Serial.print("\tLoop Time:");
Serial.println(end - start);
```

### Results
- 8-13 ms between TOF sensor measurements.
- 4-8 ms between loop iterations.
- Actual sampling rate is much lower than 50 Hz due to delays.

---

## Discussion
This lab reinforced:
- Proper soldering techniques.
- Evaluating sensor accuracy and precision.
- Improving efficiency in sensor data collection.

For future improvements:
- Implement `StartRecording` and `StopRecording` commands for efficient data collection.
- Optimize sensor filtering for better robot motion modeling.

---

