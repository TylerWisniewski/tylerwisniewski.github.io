---

layout: posts
title: "MAE 5190: Fast Robots"
background: '/images/portfolio/cev/cev-1.jpg'
permalink: /fast/
excerpt: "Fast Robots"
author_profile: true
gallery:
  - url: /lab1/
    image_path: /images/portfolio/fast-robot/lab1.png
    alt: "lab 1"
    title: "Lab 1 A/B"
  - url: /lab2/
    image_path: /images/portfolio/fast.gif
    alt: "Lab 2"
    title: "Lab 2"
  - url: /lab3/
    image_path: /images/portfolio/fast-robot/3tof.jpg
    alt: "Lab 3"
    title: "Lab 3"
gallery2:
  - url: /lab4/
    image_path: /images/portfolio/fast-robot/4md.jpg
    alt: "Lab 4"
    title: "Lab 4"
  - url: /lab5/
    image_path: /images/portfolio/fast-robot/5pid.jpg
    alt: "lab 5"
    title: "lab 5"
  - url: /lab6
    image_path: /images/portfolio/fast-robot/6yawt.png
    alt: "lab 6"
    title: "lab 6"
gallery3:
  - url: /lab7/
    image_path: /images/portfolio/fast-robot/7g.jpg
    alt: "kalman filter"
    title: "Lab 7"
  - url: /lab8/
    image_path: /images/portfolio/fast-robot/8c.png
    alt: "lab8"
    title: "lab 8"
  - url: /lab9
    image_path: /images/portfolio/fast-robot/9s2.png
    alt: "lab 9"
    title: "lab 9"
gallery4:
  - url: /lab10/
    image_path: /images/portfolio/fast-robot/10b.png
    alt: "bayes filter"
    title: "Lab 10"
  - url: /lab11/
    image_path: /images/portfolio/fast-robot/11sim.png
    alt: "lab 11"
    title: "lab 11"
  - url: /lab12
    image_path: /images/portfolio/fast-robot/12fin.jpg
    alt: "lab 12"
    title: "lab 12"

---
In MAE 5190: Fast Robots, I engineered a platform capable of mapping and navigating constrained environments at high speeds. Because the hardware constraints caused significant sensor latency, I couldn't rely on raw data alone. I wrote the core navigation firmware in C, using a Kalman Filter to blend noisy Time-of-Flight ranges with snappy 9-DoF IMU data. This allowed the robot to maintain accurate localization even when the time of flight scans were lagging. I verified the system performance using Python to analyze run data and optimize the control algorithms.

## Lab Reports
{% include gallery caption="Labs 1-3." %}
{% include gallery id="gallery2" caption="Labs 4-6." %}
{% include gallery id="gallery3" caption="Labs 7-9." %}
{% include gallery id="gallery4" caption="Labs 10-12." %}

[YouTube Playlist](https://www.youtube.com/playlist?list=PLBO_ctcwR7rnDA0wUdHClEHHy3owbuQsL)
