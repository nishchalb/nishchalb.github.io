---
title: "Building Better Movement in Godot"
date: 2024-02-28T21:18:17-06:00
draft: false
tags: ["godot", "gamedev"]
type: "post"
image: "/dt-pastel/main.jpg"
katex: true
markup: 'mmark'
comments:
  host: mastodon.gamedev.place
  username: nish
  id: 111905486151790316
---

# Vision

## Problem
Game feel is important for real time games
You want responsive controls. How to translate that to movement parameters?
You want expressive controls
whats wrong with standard physics equations

## approach
use fast funky 1d nonlinear functions
building better jump by deriving constants to care about

# Steps

## game feel: responsiveness
## why not constant velocity? expressiveness
## attack-sustain-release
### ASR of standard physics velocity control

### decide on parameters we care about (time to reach max speed, time back to zero vel, max speed)
### decide on curves we want to use, but in [0,1] domain+range
### derive
### code


[](break between outline and draft)

How do you create a responsive character controller? What does responsive even mean for this? What's wrong with using 
standard physics equation for this? This blog post will cover these questions and how you how to build responsive movement in Godot.

A lot of this post is inspired by great resources that you should also check out:

 - [Math for Game Programmers: Building a Better Jump](https://www.youtube.com/watch?v=hG9SzQxaCm8)
 - [Math for Game Programmers: Fast and Funky 1D Nonlinear Transformations](https://www.youtube.com/watch?v=mr5xkf6zSzk)
 - [Game Feel, by Steve Swink](http://www.game-feel.com/)

# Game Feel

In Swink's book, he notes that one of they keys to real-time character controller "feeling good" in a game is responsiveness.
To describe what responsiveness is, he borrows a concept from music: [Attack, Decay, Sustain, Release](https://en.wikipedia.org/wiki/Envelope_(music)) (shortened as ADSR). To look at how responsive a character controller is, we can look at the ADSR of how the player's velocity responds to input.

![ASDR Example](/godot_movement/adsr.png)

We have a few variables of interest to us:

 - $V_{max}$, the maximum velocity the player can reach
 - $T\_A$, the attack time e.g. the time it takes to reach the max speed from zero when the player is holding an input
 - $T\_R$, the release time e.g. the time it takes to get back to zero velocity from the max when the player releases input

Our movement can be considered responsive if it responds quickly to player input, both in terms of attack and release. This begs a question: why not set both $T\_A$ and $T\_R$ to zero? With a character controller like this, the player reaches $V_{max}$ as soon as the input is pressed, and goes back to zero velocity as soon as the input is released.

It's true that this controller would be responsive, but we lose another key aspect of game feel that Swink describes in his book: expressiveness.
This concept is also touched on in [this GDC presentation](https://www.gdcvault.com/play/1027580/Design-Sandbox-Analog-vs-Digital). Essentially, we can consider controls expressive if they allow the player to reach a wide range of values rather than just a single value. A good example with jumping is Castlevania vs. Mario. When the player jumps in Castlevania, it is always a fixed arc. But in Mario, the player can vary the jump depending on how long the input is pressed.

Our super responsive controller is like the Castlevania jump, the player can only move at one specific velocity. We want our attack and release times greater than zero, and our player's velocity to ramp up and down when inputs are pressed.

# Physics-based controller

The most natural way to add some ramping to our velocity is to add acceleration to our system using the standard kinemtic equations. The way this is typically done is by exposing a constant value of acceleration $a$ in a character controller. How would this look in our ASDR graph? Well, we know that a constant acceleration means a linear increase/decrease in velocity, so we end up with something like the trapezoid shaped graph from above.

This gives a pretty good controller, but I argue that we could do even better.

What are the limitations of the trapezoid shaped graph? If we want to reach the maximum velocity quickly, we have to set $T\_A$ and $T\_R$ to small amounts. But then the player doesn't have us much time to be expressive. How can we allow for larger values of attack and release time while still having things feel responsive?

# Curve your enthusiasm

There's no reason we have to limit ourselves to a constant acceleration (and therefore linear velocity changes). If we drew curves in the attack and release section of our ADSR graph, how should they look to accomplish our goals? 

We want velocity to react quickly when the player presses and releases an input, so the initial increase/decrease in velocity should happen quickly. The change in velocity can move more slowly after the input has been held/released for a while, giving the player more time before the maximum velocty for expressiveness. So, we want something like this:

![Responsive and expressive ASDR](/godot_movement/adsr_good.png)

It was easy enough to draw, but how can we implement it in Godot? We could try writing our character physics code with non-constant acceleration, but let's try something else...

## Normalized functions

Inspired by "Math for Game Programmers: Fast and Funky 1D Nonlinear Transformations", we're going to make things simpler for ourselves by restricting our input and output ranges to $[0, 1]$ and writing our curve equations that way.

What function should we use for the attack portion of our graph? I'm going to choose 
$$v=1-(1-t)^2$$, 
as it fits our criteria and will make some future things easier.

![Graph for the above equation](/godot_movement/graph_attack.png)

Notice that the graph starts out increasing quickly, but smoothly slows down to its maximum. Also notice that the domain and range of our function is $[0, 1]$.

We can slightly adjust this function to get a curve that fits our criteria for the release portion: 
$$v=(1-t)^2$$

![Graph for the release equation](/godot_movement/graph_release.png)

Just like our "attack" graph, our "release" graph starts quickly and slows down as it reaches the final value (zero, in the case of release).

# Building it

Now that we have our equations, we can finally move on to implementing things! How should we do it? To know what player's velocity should be, we need to knowhow long the player has held (or released) the input.

One option for for doing this is to track this as additional state, but this could get messy. Adding another state to track creates a new avenue for bugs. Is it possible to move on without it?

Take another close look at our graphs. The one state we need to track is the player's velocity, but if we know that, we can also figure out where we are in the x-axis (time) of the graph. Our functions are invertible! Let's solve our equations for $t$:

Attack equation:
$$t=1-\sqrt{1-v}$$

Release equation:
$$t=1-\sqrt{v}$$

Let's set up a simple scene in Godot to implement these into a character controller:
