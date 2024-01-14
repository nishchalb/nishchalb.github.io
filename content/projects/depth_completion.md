---
title: "Plane-Based Depth Image Completion"
date: 2024-01-13T12:10:53-06:00
type: "page"
---

![](/depthmodel.png)

# Abstract

The Kinect has encountered widespread use in the areas of 3D mapping and computer vision thanks to its low cost, 
portability, and depth sensing capability. Unfortunately, its depth sensor has a maximum range of eight meters, 
and past four meters it tends to be very noisy. This work presents an approach to augment the Kinect with the ability 
to estimate the data that is missing from its sensors, providing a more complete picture of what is present in a given 
scene. In contrast to much of what has been done, our work takes an approach more suited to the three-dimensional 
structure of the world rather than relying on image processing techniques. We develop a model that utilizes image position, existing 
depth, color, and surface normal measurements and derive an algorithm that can estimate missing data in a depth image using 
only sensor data as input. Our results show that the generated estimates have a root mean square error of about 25 centimeters.

Link: [Full PDF](/BhandariDepthEstimation.pdf)

![](/depth-header.png)
