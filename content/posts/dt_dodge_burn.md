---
title: "Darktable: Painting With Light (or dodging and burning)"
date: 2024-03-19T16:57:11-05:00
draft: false
tags: ["darktable", "photography"]
type: "post"
image: "/dt_dodge_burn/final.jpg"
comments:
  host: mastodon.gamedev.place
  username: nish
  id: 112130987837845513
---

![Starting point image](/dt_dodge_burn/start.jpg)

The above image is alright, but it has a big issue: the water, which should be the main subject, does not stand out. Instead, the diffuse lighting of the image draws attention to the sky and rocks. How can we draw more focus to the waterfall?

One technique we can use is "dodging and burning", where we make selective tonal adjustments to parts of our image. In this post, I will show
one such technique for doing this in darktable which I call "painting with light". We will use darktable's scene referred workflow and powerful masking features to improve the focus of our image.

## Steps

The first step is to make sure you are using darktable's scene-referred workflow. You can check this in your darktable preferences:

![Processing preferences in darktable](/dt_dodge_burn/scene_ref.png)

Note that there are two different scene-referred, and you can use whichever you prefer. The advantage of a scene-referred workflow is that
the data in our image will be kept linear and unbounded in most of our processing pipeline, usually leading to more natural looking results.
You can read more about the scene-referred workflow in the [darktable documentation.](https://docs.darktable.org/usermanual/development/en/overview/workflow/process/#scene-referred-workflow-a-new-approach)

Now, let's move on to working on our starting point image.

### Sky

The first thing I would like to do is darken the sky so that it doesn't draw as much attention. There are many ways to do this in darktable,
but I will use a new instance of the exposure tool with a simple gradient mask:

![Gradient mask](/dt_dodge_burn/sky.jpg)
![Image after darkening sky](/dt_dodge_burn/step1.jpg)

### Burning

Next, I would like to darken the rocks to draw less attention to them. We can use another new instance of the exposure tool to do this. Now, let's get to painting! 

In the masking options for this new exposure instance, there is a brush masking option. `Ctrl` clicking this will let you paint multiple brush strokes easily.

![Brush masking tool](/dt_dodge_burn/brush.png)

![Masked image for burning](/dt_dodge_burn/burn_brush.png)

You can use whatever size brush best fits the areas you want to darken. The `feathering radius` is incredibly helpful to make your selection follow the edges in the image.

Notice that in my mask on the rocks, I include a parametric mask on the `Jz` channel to exclude the highlights. For this image, this lets me
mostly darken the rocks but let some of their highlights still shine through. This is one of the main benefits of this technique: because you are 
using the simple exposure module (which just adds or subtracts exposure), you can entirely use masks to tightly control your effect (as opposed to using something like a contrast control with a fulcrum).

With this, our image is already looking much improved.

![Image after darkening rocks](/dt_dodge_burn/step2.jpg)

### Dodging

Lastly, we can apply a similar process to brighten the water in the scene to help draw more attention to it. Like darkening the rocks, we can use one more instance of the exposure tool and add a drawn mask with the brush option. 

![Masked image for dodging](/dt_dodge_burn/dodge_brush.png)

Notice that here, I reverse `Jz` channel mask I used for burning. I remove the darker values in the brushed selection to also add contrast during my brightening. 

And we're done! Here is our final image:
![Final image](/dt_dodge_burn/final.jpg)

And here is what we started with:
![Starting image](/dt_dodge_burn/start.jpg)

Mission accomplished! The water stands out a lot more now, and we just needed a few instances of the exposure tool. If you have more parts of your image that need separate adjustments, you can simply add more instances of the exposure tool to focus on each part.

Here is another after/before example of one my photos where I also used this technique:

![Additional example 1](/dt_dodge_burn/after_before_2.jpg)
