---
layout: post
title: "Funky Balls"
date: 2021-02-17 20:00:00 +0200
categories: audio programming
author: Alena Clim, Leigh Daniel Murray
image: /assets/image/...
keywords: csound, reverb, bit crusher, delay, moog diode, string resonator, harmoniser
excerpt: "ccc"
---

# Funky Balls
________________add an image in the beginning?___________

## The idea

We started by considering the effects of music on the human brain and all the amazing music therapy techniques discovered so far. Then we looked for some examples of studies about music therapy and stress to understand more about how they are correlated (and if?). We found the [MISTRESS project](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=&ved=2ahUKEwikwv23qPbuAhXwpIsKHfL7DlkQFjABegQIAhAD&url=https%3A%2F%2Fdanishsound.org%2Fwp-content%2Fuploads%2F2018%2F10%2FMISTRESS_Phase1_Report.pdf&usg=AOvVaw1s214Cl7_42xLTUh1_PFX5) by Rudi Agius, part of the [Danish Sound Cluster](https://danishsound.org/) where they analysed the effects of music intervention on stress reduction - and the results are that of course it helps! We also found a [paper](https://ieeexplore.ieee.org/document/7557197/authors#authors) about a study where they used music as a non-invasive technique of identifying stress in people (they used EEg and noticed that the brainwaves were substantially different before and after listening to music). 

Now, of course these studies are not enough for undebatable evidence, but we took it at face value that music is good for de-stressing and we extrapolated until we came up with the idea of creating a **de-stressing audio tool**. It would allow both people with and without (digital?) musical background to play around with some instruments / tracks and several effects, and in general _make_ music in an unconventional way (is this too much to say?). It would facilitate communication through sound, without semantic content, among people who would mix / play together.

To concretise, we decided to create a web interface for any type of user for the purpose of de-stressing through making / mixing / playing around with music. It would include blobs/shapes of different colors: some would represent effects (e.g., reverb, harmonise, delay) and other would represent instruments or tracks (e.g., drums, cellos, synth, piano) and the users would then be able to move these blobs around and they would interact with each other and voila! mix / make new music. See the next sections for more details about the further inspiration and implementation.


## Inspiration

These platforms inspired us:

YUME - HELIOS
Chrome Music Lab
Gøy
Finding Love
Bouncy Notes

## Implementation

### One effect and one track

We started by using sliders to implement a simple version. Maybe add something about the sliders code we took from Oyvind.

________________add an image or small video with the first version___________

### Many effects and tracks

Next step was to implement more tracks and keep the same possible effects for each. 6 effects per track with their own variable parameters. We used loops because of the size of wav files.

________________add an image or small video with the last version of sliders?___________

Effects:
-  **Moog Diode Ladder Filter**: a digital emulation of this filter. Slider changes the cutoff frequency [(moogvcf)](http://www.csounds.com/manual/html/moogvcf.html).
- **Reverb**: it reverberates an input audio signal with a “natural room” frequency response. Slider changes the reverb time [(reverb)](http://www.csounds.com/manual/html/reverb.html).
- **String Resonator** (our waveguide!): a string resonator with variable fundamental frequency. Slider changes the fundamental frequency values [(streson)](http://www.csounds.com/manual/html/streson.html).
- **Delay**: it delays an input signal by some time interval, with feedback. It's a combination between delayr and delayw. Slider changes the feedback ratio [(delay)](http://www.csounds.com/manual/html/delay.html).
- **Bit-crusher**: Slider controls the bit depth (custom opcode). !!!!link? !!!
- **Harmoniser**: it analyzes an audio input and generates harmonizing "voices" (or frequencies) in synchrony. The slider controls the ‘lowest expected frequency’ of the input [(harmon)](http://www.csounds.com/manual/html/harmon.html).

### Simple interface

The implementation of the Meta Balls from the paper.js
Minimalist user interface: Learn by exploring & Rely on audio feedback


________________add an image or small video with the last version of sliders?___________

### Dynamic interface

Adding dynamic effects and changing colors and adding the instructions. Also adding the possibility of another song. 
Modifying effects without user interaction
Visual feedback of dynamic changes
____add sumary of how to use it i'd say, or at least a few instructions otherwise people are gonna get frustrated____!!!!!

________________add a small demo video with the last version___________

## Where to go and play?

There are two "interfaces" you can choose from: the sliders (mostly useful for debugging, it's not very pretty) and the funky balls (not the best for debugging but very pretty). You can also choose between two songs. You can download the entire repository from github or you can go to these links:

**Github**: it has to have a link to your repository right???????
**Sliders, sliders, sliders**: balls.leighmurray.com/controls
**Funky balls - song 0**: [Leigh's](balls.leighmurray.com)
**Funky balls - song 1**: [Alena's](balls.leighmurray.com/?song=1):not the [epic movie soundtrack](https://cambridge-mt.com/ms/mtk/#NahumStrickland), but a funky drum&bass loop extracted from [Human Mistakes by Skelpolu](https://cambridge-mt.com/ms/mtk/#Skelpolu)

## Future improvements

_Let's play together!_ Although in the beginning we wanted to create a multi-user platform, behind the scene it would have been somewhat similar to Leigh's [Kovid Keyboard](https://mct-master.github.io/digital-audio/2020/11/03/kovid-keyboard.html). That's why we decided to work on other features, such as the dynamic effects instead of taking the time to integrate the already existing code. For the future, however, this connectivity feature is important to implement.

_Wanna see something cool?_ Another feature that helps with the connectivity side is to be able to save the project and send it to someone - and enable them to pick it where you left off. Sharing work and being able to save it for later is important!

_Can I play with my own music?_ Enabling a feature where users can upload their own tracks to play with. This complicates things of course, because we would have to maybe consider a flexible number of instruments/tracks.

_What if I remember the positions?_ To avoid people using the same effects on the same instruments, it would be good to randomise ball positions and colours.
__add more here, i'm not very sure___!!!!!

_What about loops?_ As per Henrik's idea, we can to combine the [Looper](link) with this 
___ad description____!!!!

_How else can I controll the effects?_ It would be interesting to enable camera control. By that we mean that head/hand movements would control for example the direction of the effects. See [this](link) for an example?
____not sure exactly how to explain it here, sorry____ !!!!!
___________you can add the link to that camera thingie you found_____________!!!!!!!!!!!!!!!





