---
title: "walltime"
date: 2024-01-13T12:10:53-06:00
type: "page"
---

![](/walltime-header.png)

[Project Page](https://github.com/nishchalb/walltime)

# Concept

walltime is a small python script with a command line interface that can set the desktop wallpaper to an image 
based on the time of day. walltime chooses an image semi-randomly that fits with the time of day. walltime works 
by mapping the 24 hour clock to a color gradient (currently the viridis colormap in Matplotlib), and then finding images 
who's mean color is most similar to the color corresponding to the current time of day. Color distance is measured 
in the LAB colorspace, where Euclidean distance closely matches perceptual differences in color.

![](/viridis.png)

# Technologies Used

walltime is written in Python and takes advantage of Numpy, OpenCV, SciPy, and sqlite3 libraries. A local database is used 
to index mean color values for images so that close matches can be calculated quickly and image statistics only need 
to be found once. A command line interface allows users to manage the images they want to be considered and easily update their wallpaper.

![](/OpenCV_Logo.png)
