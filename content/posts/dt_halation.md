---
title: "Darktable: Emulate Film Halation"
date: 2024-01-30
tags: ["darktable", "photography"]
type: "post"
image: "/dt-halation/h1.jpg"
draft: false
comments:
  host: mastodon.gamedev.place
  username: nish
  id: 111848462658114096
---

![](/dt-halation/h1.jpg)

I recently came across an article about the [halation effect](https://blog.dehancer.com/articles/halation/) in film (check out that article if you are interested in a description of the effect). I wanted to 
emulate the effect using darktable, and thankfully the `Diffuse or Sharpen` module that was released in darktable 3.8 makes this pretty easy.

The method I will describe below is heavily based on one by [one of the authors of the module](https://youtu.be/fG0gD96TSdc?si=vNQ9R8p-_Ku3WZhJ&t=2480).

I use the `IMG_9759` image from [Signature Edits](https://www.signatureedits.com/free-raw-photos/) to demonstrate.

## Steps

First, enable an instance of the `Diffuse or Sharpen` module, and increase the 1st and 4th order speed to your liking. These will control the strength of the diffusion effect. The 1st order speed will affect lower frequencies in the image, while the 4th order speed will affect higher frequencies.

![](/dt-halation/diff1.jpg)

Next, we will use the masking features of darktable to restrict the effect to areas and colors that we want. We set the Blend Mask mode to `RGB red channel` to restrict the diffusion effect to the Red channel of the image. We use `details threshold` to restrict the effect to higher contrast boundaries, and apply some blur to the mask to ensure the diffusion effect isn't cut off by it.

![](/dt-halation/diff1mask.png)

This alone will create a red halation effect. You can use the properties, speed, and masking sliders to tune the effect.
To change the color of the effect, you can duplicate the module and change the Blend mode to the RGB green or blue channel, and adjust opacity to get the desired color.

And thats it!

## Comparison

Here is a zoomed version of the above image without and with the effect:

![](/dt-halation/diff_without.jpg)
![](/dt-halation/diff_with.jpg)

If you would like to load the actual edits into darktable (I am using version 4.6.0), here is the [XMP file](/dt-halation/IMG_4255.CR3.xmp)

And here is another example of applying the method to a different image also from Signature Edits, `@wutkaicheung tag @signatureeditsco - free raws from signatureedits.com_DSC00107(2).ARW` (before, then after)

![](/dt-halation/diff2_without.jpg)
![](/dt-halation/diff2_with.jpg)

