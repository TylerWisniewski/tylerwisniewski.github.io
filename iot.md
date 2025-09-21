---
layout: posts
title: "iot"
permalink: /iot/
excerpt: "Machine Learning Traffic Tracker"
author_profile: true
---

## Executive Summary 

The vision of our project was to create a truck tracking system which encompasses electronics that can successfully support the YOLO version 8 algorithm designed for tracking specifically trucks among other vehicles in traffic. Furthermore, our goal was to integrate the hardware into a waterproof and durable enclosure resistant to any weather conditions. What is more, the system ought to be able to support LoRaWAN capabilities, and transmit data wirelessly via LoRaWAN to The Things Network (TTN). The retrieved data from TTN will contain packets of vehicle identifiers, particularly the vehicle type identifier, which will differentiate between different truck types (for the purposes of our community partner’s data interpretation): an assigned vehicle ID which will enable maintaining a count of vehicles and particularly trucks in real time, as well as a timestamp at which a truck was detected. The successfully transmitted data will only be provided to our community partner Blueprint Geneva, and they will be responsible for interpreting the truck traffic data.

The primary motivation behind this project was to help eradicate the issue of pollution in Geneva, which is impacted by emission of bad smells and dangerous chemicals, ultimately affecting the health of Geneva’s residents. The environmental group has a dedicated website [It Stinks](https://www.itstinks.org/map?limit=500&c=5762273213677568), logging resident concerns in hopes to draw attention to the severity of the issue. Therefore, the primary challenge we have completed throughout this semester is demonstrating a functional system which can track truck driving by running the computer vision (CV) algorithm on the integrated electronics in real time, so that BluePrint Geneva has clear and digestible data on the fundamental pollutants. According to our last communication with Blueprint Geneva, the truck trucking system will be integrated with the rest of their air quality equipment, which will help determine the correlation between air quality monitor readings and our obtained truck traffic readings. Hence, our community partner will be able to gauge the air pollution within the City of Geneva with a more informed perspective.

The contributions we have made to the first iteration of this project are being able to transmit all the detected vehicles in traffic in real-time, by performing testing on the Cornell University campus. The camera focus is adjustable for a variety of distances, and the electronics are integrated in a robust and compact enclosure which ought to be able to resist any weather conditions. The system’s design is adaptable to the ranges of local powerlines, and converts the input voltage to the desired 5 V for our system’s operation. The current limitations of the project are the unknown powerline source, which is dependent on Blueprint Geneva’s decision with regards to City of Geneva regulations. The next steps of the project ought to be performing fieldwork in Geneva, alongside our community partners, once the location is confirmed in order to ensure operation with the provided input voltage as well as the performance of the integrated enclosure and its electronics.

# Full Report
