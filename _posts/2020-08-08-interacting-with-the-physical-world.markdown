---
layout: post
title: "interacting with the physical world"
date: 2020-08-08
comments: true
---

<p class="intro">
Throughout my programming carrer, nothing had excited me more than when the code i write interacts with the physical world.
</p>

This could be something as simple as lighting up a led on some PIC, somehow the idea of writing some code, 
and that reaching out of the software plane into the physical plane has always been very enticing to me.

Now, i also got into sim-racing lately due to the ongoing pandemic and the need for more indoors hobbies (as if i had any outdoors hobbies to being with).

This led me through a path of curiosity on how exactly these racing simulators communicated information to the peripherals like steering wheels,
where you can see how fast or what gear you are using on a LED screen, or even a motion platform.

I started with a very quirky idea in mind, what if... i could somehow get data from how fast i am going in a game, and control a PWM fan with that.

Now this led me through a rabbit whole of protocol implementations, most notably implementing the code masters UDP telemetry protocol as you can see on [cm-telemetry](https://github.com/ozkar99/cm-telemetry).

During my college years i did a fair amount of arduino coding and embeded systems coding.

I still remember the first time i managed to light up a led, or hookup a wii nunchuck to control some motors.

So it was only natural that when i had the need to interact with the physical world i gravitated towards arduino.

One small problem tho, the code i wrote for telemtry is in Rust, and arduino runs on something that kinda looks like C++.

So i started thinking: "maybe its easier to implement a serial protocol to issue commands from rust to arduino, rather than implementing the telemetry protocol on arduino".

[So i did just that](https://github.com/gpioduino/spec), designed and implemented a protocol (a very simple one) for arduino communication, be wary this is something super experimental and i know it wont be fast enough for serious applications, but it was a good excuse to learn about serial ports and doing some more Rust, while also building something i always wanted:

The simplicity of arduino, but the power of a full size desktop along with the liberty to use any language (as long as you implement the protocol for it).
