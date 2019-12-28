# OSMC-LEDFX-WLED
OSMC with LEDFX and WLED

High all. So this is a short tutorial for getting LEDFX to work with OSMC on a Raspberry Pi.

Now a note to all, I used a raspberry pi zero and a pcm5102 dac board for this project. Although it worked the raspberry pi zero is not powerful and i reccomend that you choose a more powerful raspberry pi for this project. 
Also to note that some of these options will be different depending the type of device you have and the configuration. An example of this is if you have the inbuilt audio out in raspberry pi 1,2,3 etc and will affect the numbering of some configurations

I will do my best to demonstrate this to you.

Im not going to go through configuring and installing osmc on a raspberry step by step nor wled on a esp32. There is better documentation that will take you through the intallation of those softwares then this one.

Reason for this was to implement a solution that intergrates internet radaio as well as plex collection for audio to wled without the use of a computer through the traditional way.

This sollution uses ALSA module multi

Hardware pre-requisites
* Raspberry pi
* PCM5102
* Esp32 of some kind
* Monitor for connecting pi to monitor (unfornately this cannot be done completely headless)
* Keyboard for pi (and OTG adaptor if pi 0)



Software pre-requisites
* WLED on esp(https://github.com/Aircoookie/WLED) with E1.31 enabled
* OSMC on Raspberry Pi (https://osmc.tv/download/) (Click disk images down the bottom to download Pi image depnding on the device you own
* 


1. OSMC configuring.
