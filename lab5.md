---
layout: posts
title: "Fast Robots - Lab 5: Linear PID"
permalink: /lab5/
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

The purpose of this lab was to implement closed loop control through PID controllers. The state I controlled was speed, and I used the TOF sensors to stop a set distance from the wall. PID stands for proportional, integral, derivative control. 

$$ u(t)= k_p * e(t) + k_i * \int_0^t e(t) dt + k_d * e(t)*\frac{d}{dt}$$