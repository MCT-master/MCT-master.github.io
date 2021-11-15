---
layout: post
title: " Don't stop the music please, but please do "
date: 2021-10-25 22:00:00 +0200
categories: ​​scientific-computing
author: Arvid Falch, Joseph Clemente, Kristian Wentzel, Jakob Høydal
image: /assets/image/2021_10_25_jakobhoy_StopTheMusicFavicon.png
keywords:
excerpt: "How we stopped the music, sliced, and restarted it to make a re-synthesized song using onset detection and spectral centroid."
---



<figure>
  <img src="/assets/image/2021_10_25_jakobhoy_StopTheMusicHeader.png" alt="Figure 1" width = "100%" allign="middle" />
  <figcaption> </figcaption>
</figure>

# Introduction
<br>
<b>Our task was to take two audio files, write a Python program to slice them up, and join the segments from both audio files based on some features we were free to choose ourselves. We quickly agreed on choosing an original song and a cover version of the song, and made up our minds what features of the audio we would use to slice and join the audio.</b>
<br>

“Don’t stop the music” was first recorded and released in 2007 by Rihanna. The song was written by Tawanna Dabney and Norwegian production team StarGate. Jamie Cullum recorded a cover of the song in 2009, and it is these two versions that Team B decided to chop up and put together based on features of the audio.

Simply put, we used librosa to separate the harmonic and percussive content of the songs, and then ran onset detection on the percussive tracks. The audio files were then sliced up into segments based on the onsets. The harmonic versions of the segments were analysed to find the spectral centroid, and sorted in ascending order based on the spectral centroid average.

In the end we added the segments together one by one, creating a new audio file with all the segments from the two original tracks, but rejoined in a completely new way based on the analysis described above.   

In our design, we decided to create a class for each segment, which includes the audio array and the spectral centroid average value. After creating a class for each segment, we sorted each segment based on the spectral centroid average value in the combined pandas dataframe, then used the indexes in the dataframe to extract the correct audio segment from the class array.
<br>
<br>

# Results

<p style="text-align:center;">The two songs chosen for our re-synthesis:</p>

<figure>
    <iframe src="https://open.spotify.com/embed/track/0ByMNEPAPpOR5H69DVrTNy" width="100%" height="80" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>
    <figcaption><i> Rihanna - Don’t Stop The Music </i> </figcaption>
</figure>

<figure>
    <iframe src="https://open.spotify.com/embed/track/7hMPR0V7cC0hln0SN2afLL" width="100%" height="80" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>
    <figcaption><i>Jamie Cullum - Don’t Stop The Music (Cover)</i></figcaption>
</figure>


By using two songs that’s not really that similar but have the same foundation, we get some interesting artefacts in the re-synthesised audio. Since Rihanna’ original song is a dance-song, it emphasises the quarter notes. Cullum’s version uses sub-divisions to carry the song. This leads to the snare-drum from Cullum’s song appearing in some interesting places.

Since the re-synthesized song is arranged in ascending order based on the spectral centroid, it leads to sounding a bit musical and rhythmic. It sounds like a duet between Rihanna and Cullum, fighting each other for the spotlight. The duet-fight ends with Rihanna naturally winning the battle by bringing a dance song to the fight.
<br>

<figure align="middle">
    <audio controls>
      <source src="https://drive.google.com/uc?&id=1vxwyz1undNGhDsD6lHSshDLtX9MqJjiK
" type="audio/wav">
      Don’t Stop The Music, combined.
    </audio>
    <figcaption> <i> A thirteen seconds audio sample of our combined audio, taken in the middle of the full audio where there is more of a balance between Rihanna and Cullum’s contributions.</i></figcaption>
  </figure>
<br>

# Challenges

Unfortunately, due to the time it took to run the HPSS function and to create the combined audio array, we could not use the full songs in the final output. Instead, we shortened each song to one minute and 52 seconds.


When it came to plotting the re-sequenced audio, showing how segments from the different files contributed to the new version, we had a race against both the CPU and the clock. A race — or a Marathon might be more fitting, as it was a long and somewhat tedious journey — we were doomed to lose. The reason for the struggle was that the whole program should clock in at under 5:00 min from start to end.
<br>

<figure>
  <img src="/assets/image/2021_10_25_kriswent_ZeroesMusic.png" alt="Plot attempt 1" width = "100%" allign="middle" />
  <figcaption><i> Here you can see how each new segment also consists of the previous one(s). Rihanna's first segments shown in red, with opacity set to 5%. (Cullums segments in gush green are still on top of each other.)</i> </figcaption>
</figure>
<br>

We had an idea to plot the final audio as a traditional waveform, but with each segment color-coded to show it’s origin. This happened to be a much more complex task than anticipated. In the lack of managing to offset the coordinates along the X-axis so each new segment started where the previous left off, we approached it by filling in lots of zeros to offset them instead. Those zeros seemed so innocent, but oh my, how they teamed up against the CPU. It would often result in the Jupyter Notebook just shutting down by turning it’s back against us, not even bothering to tell us it wouldn’t work for us any more. Restart kernel to the rescue! We later figured out that even if we could find a way to process and plot it all, which seemed close in sight several times, it would take too much time in the end.

<figure>
  <img src="/assets/image/2021_10_25_kriswent_SubplotsStop.png" alt="Plot attempt 2" width = "100%" allign="middle" />
  <figcaption><i> 5 subplots with 100 segments each. The timer above shows that it processed each subplot in good time. But below the plot you can see it took a lot of time to actually show the final plot.</i> </figcaption>
</figure>

<br>

We tried different approaches. They were all based on using subplots with parts of the whole, to “reset” the ever-ascending pile of zeros lurking behind the scenes. In the figure above you can see how displaying 500 segments was doable, but this was almost as far as we could go without the Jupyter Notebook shutting down. There were 987 segments in total, and we managed to plot 600 of them in about 3:30 minutes. We actually got confirmation – by printing a message for every 100 segments – that we were able to process them all. But there was NO way it would show us the plot in the end. It took about 4:00 minutes to process, and small artifacts started to appear across the screen showing the struggle. Another approach was to make the subplots aligned horizontally without an axis and as close to each other as possible (with just a slight gap). This was the aesthetically most pleasing approach, and ended up being the way we visualized it in our assignment. Though for our final version, we thought it looked best with a “zoomed in” plot of only 100 segments. We found that different approaches had slight variations in time spent, but they all faced the same limits in the end. The process of finding a solution to make it work taught us a valuable lesson regarding CPU and time-consumption, and it forced us to think more cost-efficient overall.

<figure>
  <img src="/assets/image/2021_10_25_kriswent_SubplotsMusic.png" alt="Plot attempt 3" width = "100%" allign="middle" />
  <figcaption><i> 2 subplots of 100 segments each, with their axis removed. You can see a slight gap between the subplots. We thought this approach was the most aesthetically appealing.</i></figcaption>
</figure>