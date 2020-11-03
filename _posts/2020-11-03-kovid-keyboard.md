---
layout: post
title: "Kovid Keyboard"
date: 2020-11-03 12:00:00 +0100
categories: digital-audio
author: Leigh Murray
image: \assets\image\2020_11_03_leigh_kovid_keyboard.png
excerpt: "A web based CSound Synth letting you play together online!"
keywords: digital-audio, midi, csound
---

![Kovid Keyboard](/assets/image/2020_11_03_leigh_kovid_keyboard.png)

# Play Together Now!

Check it out at the link below and send the link to a friend to play together!

You can use the lower two rows of your computer keyboard to play notes after you click the on screen keyboard, or use a USB keyboard if you have one.

<a href="https://midi.leighmurray.com" target="_blank">https://midi.leighmurray.com</a>

# Overview

Since people are home a lot more I decided to make an online synth to let people play together from home.

The Kovid Keyboard is a polyphonic midi synth that is synchronised between all users, that is, any changes you make to the Kovid Keyboard on your computer are applied to all the other Kovid Keyboard users in real time.  In this way, rather than sending audio data between users, only note on and note off events are sent between users and all sounds are generated locally for each user using CSound. USB Midi keyboards and sliders/knobs are automatically mapped to the knobs on screen, but MIDI learn can also be used by right clicking on a knob/slider and clicking `learn`.

The Kovid Keyboard has 12 individual CSound instruments which are each bound to their own MIDI channel (1-12). The user can select their instrument using the `Select an Instrument` dropdown at the top of the screen and all note-on/note-off events generated by the user will be sent to their selected instrument in CSound. Other users can select different instruments and their note-on/note-off messages will be sent to the instruments they have chosen.  You can see which users are playing what instruments in the table on the right.

All instruments are single oscillator instruments except for instrument `10: Subtractive Synth Wave` which has two oscillators.  Some instruments have more knobs than others which is determined by the amount of parameters than can be passed through to the oscillator's opcode in csound.

The `Start` button is only necessary on some web browsers to initialise midi but if your browser works without it you don't need to press it.

The `PANIC!` button turns off all MIDI notes, in case some note off messages get lost and sounds won't stop playing.

![Start PANIC!](/assets/image/2020_11_03_leigh_kovid_keyboard_start.png)

# Frontend

I'm using the CSound for Web library which is a standard Javascript library available from the [CSound downloads page](https://csound.com/download.html) under "Mobile and Web". It's [poorly documented](https://csound.com/docs/web/) but by using the [examples found here](https://waaw.csound.com/) I was able to piece it together.

I found a [web library](http://g200kg.github.io/webaudio-controls/docs/index.html) that has cool looking knobs and automatically binds to MIDI command messages from USB MIDI devices, and also provides a MIDI keyboard input, so I hooked that up.

I then send the values from these knobs into the different CSound instruments to change various parameters.

# Backend

I'm using Nodejs as my webserver and Socket.io for sending simplified MIDI and control data messages between all the users in realtime.

# Instruments

You can select an instrument using the `Select and Instrument` dropdown menu.

The Gain and Limiter are local parameters, they aren't sent to other users. The `Gain` lets you adjust the overall volume of the synth and the `Limiter` switch limits the output to -0.9, 0.9.

![Gain Limiter](/assets/image/2020_11_03_leigh_kovid_keyboard_gain.png)

All instruments have Amplitude `AMP`, Low Pass Filter `LP`, and Attack `aA`, Delay `aD`, Sustain `aS`, Release `aR` parameters.

![Amplitude Low Pass](/assets/image/2020_11_03_leigh_kovid_keyboard_amp.png)

![ADSR](/assets/image/2020_11_03_leigh_kovid_keyboard_adsr.png)

Instruments 1-7 all use the [vco2 opcode](http://www.csounds.com/manual/html/vco2.html) to generate different wave shapes, but Instruments 5 and 6 have a Pulse Width `PW` parameter to change the ramp or pulse width of the Triange and Square waves respectively.

![Pulse Width](/assets/image/2020_11_03_leigh_kovid_keyboard_pw.png)

Instrument 8 uses the [oscil opcode](http://www.csounds.com/manual/html/oscil.html) to generate a standard sine wave.

Instrument 9 uses the [gbuzz opcode](http://www.csounds.com/manual/html/gbuzz.html) to generate a sine wave with a variable amount of harmonics.

![Harmonics](/assets/image/2020_11_03_leigh_kovid_keyboard_harmonics.png)

Instrument 10 has two oscillators.  The user can choose the shape of each oscillator using the `Osc1` and `Osc2` sliders. The top position is a Square Wave with variable Pulse Width `PW`, the middle position is a Triangle wave with variable ramp `PW` and the third position is noise.  The signal from these oscillators goes through a filter with its own ADSR `fA`, `fD`, `fS`, `fR` and resonance value `fR`.  The Low Pass Frequency `LP` is now passed to the filter to act as its upper limit.  You can change the Octave of each oscillator using `oct1` and `oct2` and fine tune each oscillator using `cent1` and `cent2`.  I got most of the code for this instrument from the [floss manual here](https://write.flossmanuals.net/csound/b-subtractive-synthesis/).

![Harmonics](/assets/image/2020_11_03_leigh_kovid_keyboard_knobs.png)

Instrument 11 and 12 are simple Saw waves using the [vco2 opcode](http://www.csounds.com/manual/html/vco2.html).


# Resources

If you want to check it out yourself you can get the code here: [https://github.com/leighmurray/csound-socket](https://github.com/leighmurray/csound-socket)

You need to install [NodeJS](https://nodejs.org/en/download/). Then clone the code and you should only have to run:

```
npm install
node index.js
```

Then go to [http://localhost:3000](http://localhost:3000)