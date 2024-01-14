---
title: "img-ditto"
date: 2024-01-13T12:10:53-06:00
type: "page"
---

![](/imgditto-header.jpg)
[Project Page](https://github.com/nishchalb/img-ditto)

# Concept

img-ditto is a small python script with a command line interface that allows you to transfer the color style from 
one image to another. It works by using the histogram matching technique on the channels of an image. Typically the 
technique is used to equalize the intensity of an image to improve contrast, but I've found that the method works 
decently well for color transfer as well. This implementation work well when the "to" and "from" images are similar in 
content (both images are of a city, forest, etc.). The best transfers were typically in the LAB colorspace, but the 
program lets you choose other colorspaces as well.

![](/hist.png)

# Technologies Used

img-ditto is written in Python and takes advantage of the Numpy, SciPy, and OpenCV libraries. The program works by building 
a histogram of pixel values for each channel of a color image, and remapping pixels in the target image so 
that it's histogram matches that of the desired image.

![](/OpenCV_Logo.png)


