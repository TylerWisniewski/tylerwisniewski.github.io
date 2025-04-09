---
layout: posts
title: "Fast Robots - Lab 8: Stunts!"
permalink: /lab8/
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
The goal of this lab was to improve on past controllers in order to be able to perform a stunt. I chose to do the flipping stunt, utilizing the kalman filter and PID control I created last week. 

## Robot Alterations

In order to flip the robot successfully, I had to my existing robot setup. I didn't have access to the traction pad in lab, so I initially tried to tape the floor or my robot, but this had poor results. Next, I tried adding various weights to the top of my robot to achieve a higher Center of Mass to make flipping easier. Eventually, removed the top shell and taped some endmills to the top of my robot and this seemed to work well!

## Stunts

In this lab I used the Time of Flight and Kalman Filter code I wrote last week to collect distance information. However, I didn't implement any of the PID control aspect since I wanted to go very fast forward, and then reverse once I reaced some distance threshold. Since the robot slips signifigantly when flipping, I tested iteratively and found a good distance threshold to be ___ for a success flip ~1ft away from the wall.

I had 5 primary states for my setup. In state 1, I drive forward at max PWM and leave for state 2 once I cross my distance threshold. In state 2 I am reversing at max PWM for 500ms to flip my car. In state 3, I set my PWM to 0 so I don't spin out of control while I'm flipping. In state 4, I travel backwards at max PWM again for 600 ms, and then I stop driving and begin sending data in state 5.

```c
...
                if (distance < 620){
                  dir=1;
                  pwm=255;
                  timer0= millis();              
                }
                else if ((timer-timer0)<500){                 
                  dir=-1;
                  pwm=255;
                }
                else if ((timer-timer0)<600){
                  dir=1;
                  pwm=0;
                }
                else if ((timer-timer0)<1200){
                  dir=-1;
                  pwm=255;
                }
                else {
                  driveStraight(0, 0);  //stop driving
                  dir=1;
                  pwm=0;
                  stop=1;
                }
...
```
*Figure 1: Flip Code*


[![Stunt 1](https://img.youtube.com/vi/gCnoebWyYc4/0.jpg)](https://youtu.be/gCnoebWyYc4)

*Figure 2: Video for Stunt 1*

![kalman filter](/images/portfolio/fast-robot/8p1.png)  
*Figure 3: Distance and PWM Plots for Stunt 1*


[![Stunt 2](https://img.youtube.com/vi/IDwuLByJJRE/0.jpg)](https://youtu.be/IDwuLByJJRE)

*Figure 2: Video for Stunt 2*

![kalman filter](/images/portfolio/fast-robot/8p2.png)  
*Figure 3: Distance and PWM Plots for Stunt 2*


[![Stunt 3](https://img.youtube.com/vi/FogO7yJx5O8/0.jpg)](https://youtu.be/FogO7yJx5O8)

*Figure 2: Video for Stunt 3*

![kalman filter](/images/portfolio/fast-robot/8p3.png)  
*Figure 3: Distance and PWM Plots for Stunt 3*

Overall, this lab was pretty fun. Everything worked well and I spent signifigantly less time debugging than previous labs which was nice!