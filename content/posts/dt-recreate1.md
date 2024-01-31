---
title: "Darktable: Recreate this look #1"
#date: 2024-01-14T15:31:24.771466+00:00
#date: 2021-04-15T23:39:49+05:30
date: 2024-01-14
tags: ["darktable", "photography"]
type: "post"
image: "/dt1/pB2L52j.jpeg"
draft: false
comments:
  host: mastodon.gamedev.place
  username: nish
  id: 111756754865209427
---

![](/dt1/Dqo7xDO.jpeg)

A while ago, someone asked online how to recreate the look of [sanghan](ttps://www.instagram.com/sanghan_).
Here is my attempt at showing how to do this using darktable 3.6.

![](/dt1/pB2L52j.jpeg)

Here is the end product of the edit. I found this image on [Signature Edits](https://www.signatureedits.com/free-raw-photos/)

![](/dt1/EBhTfXv.jpeg)

Here is our starting point. Notice that the conditions are similar to the images we are using as inspiration. It's a lot easier to achieve a look when your lighting conditions are close.

![](/dt1/cfWb4yk.jpeg)

First, we adjust our color calibration so that our whites are similar in hue to the target image. I measured that hue to be about 184 degrees (we set the opposite hue in this module, which would be 184 - 180 = 4).

![](/dt1/q1ZNoeS.jpeg)

Next, I use the filmic rgb module to set my white and black relative exposures.

![](/dt1/fFxijvG.jpeg)

I raise the "target black luminace" in filmic rgb to raise and crush the darker tones in the image.

![](/dt1/nczOcVR.jpeg)

Using the new "Color balance RGB" module introduced in darktable 3.6, I massage the colors a bit. This gives us 3 different ways to control colorfulness. I mainly use the "perceptual saturation grading" to help adjust the colors in a way that doesn't look unrealistic. I keep most of the colorfulness in the shadows (which also has the side effect of darkening them), and decrease saturation in the midtones and highlights (which also brightens them). I bring "brilliance "down for both after to keep things from being too bright.

![](/dt1/P6CwTAh.jpeg)

For the above image, I also make some luminance adjustments. I raised the global offset and reduce shadows to further crush the darker tones. Then I decrease highlights and increase power to reduce contrast.

![](/dt1/fM4JWlP.jpeg)

Next, I add film grain and add some yellow-orange to the shadows (a hue I found in the source images).

![](/dt1/9fQNxIV.jpeg)

Using a parametric mask to select non-cool tones, I add some red to the image.

![](/dt1/qiPIOHa.jpeg)

I add some color back to the yellows with a similar technique. And we're done!
