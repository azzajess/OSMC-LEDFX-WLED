# OSMC-LEDFX-WLED
OSMC with LEDfx and WLED (or any other compatible esp firmware!)

Hi all. So this is a short-ish tutorial for getting LEDFX to work with OSMC on a Raspberry Pi.

Now a note to all, I used a Raspberry Pi Zero and a pcm5102 dac board for this project. Although it worked the raspberry pi zero is not powerful and I recommend that you choose a more powerful Raspberry Pi for this project, if you can.
Also to note that some of these options will be different depending on the type of device you have and the configuration. An example of this is if you have the inbuilt audio out in raspberry pi 1,2,3 etc and will affect the numbering of some configurations

I will do my best to demonstrate this to you. I'm no expert with Linux audio.

I'm not going to go through configuring and installing OSMC on a raspberry step by step nor WLED on a esp32. There is better documentation that will take you through the installation of those softwares then this one.

Reason for this was to implement a solution that integrates internet radio as well as plex collection for audio to WLED without the use of a computer through the traditional way.

This solution uses ALSA module multi

Hardware prerequisites
* Raspberry pi
* PCM5102
* Esp32 of some kind
* Monitor for connecting pi to monitor (unfortunately this cannot be done completely headless)
* Keyboard for pi (and OTG adaptor if pi 0)



Software prerequisites
* WLED on esp(https://github.com/Aircoookie/WLED) with E1.31 enabled (https://github.com/Aircoookie/WLED/wiki/E1.31-&-UDP-Realtime-Control)
* OSMC on Raspberry Pi (https://OSMC.tv/download/) (Click disk images down the bottom to download Pi image depending on the device you own
* Putty to ssh into it


1. Load module at boot and implement sound profile in asoundrc
Choose the Raspberry Pi config that suits your needs.

* [If your using a Raspberry Pi Zero with PCM5102](Raspberry%20Pi/Raspberry%20Pi%20Zero%20%2B%20PCM5102.md)
* [If your using a Raspberry Pi 3 with inbuilt audio out](Raspberry%20Pi/Raspberry%20Pi%203.md)
* [If your using a Raspberry Pi 3 with PCM5102]()


2. [OSMC configuring](Configuring%20OSMC.md)


3. [Installing Python 3.6](Installing%20Python%203.6.md)


4. Installing LEDfx


8 We have a few options depending on the route you would like to take.

* Recommended
* ahodges9 Master (https://github.com/ahodges9/LedFx/)

* [Instructions](LEDfx%20Installations/ahodges9%20Master.md)

* Mattallmighty Fork (https://github.com/Mattallmighty/LedFx)

* [Instructions](LEDfx%20Installations/Mattallmighty%20Fork.md)


5. [Starting and Configuring LEDfx](Starting%20and%20Configuring%20LEDfx.md)

NOTE: I did this out of order due to errors that I went back and installed to fix. So this is hopefully the correct order of installing LEDfx with a new version of Python. Please forgive me if you encounter problems due to some misinformation.

How this works!?
Ok so i'm no expert.
Few things to consider, snd_aloop module creates a loopback device for EVERY device you have. All we are doing is duplicating or splitting audio through multi(?) and connecting it to the hardware device so we can listen to the same audio we are sending out to the capture.
Like I said there is more than likely room for improvement and if you discover anything that can improve performance whether that's using jack or something else, please let us know.

Extra Optimisations?
* I reduced the resolution of OSMC to 720 for testing purposes
* I lowered the GPU memory in My OSMC/Pi-Config to 64MB since I won't be using a display. Even at 64MB it was usable so this would mostly affect video content? NOTE: changing vram to 64MB causes the PLEX addon not to work. It must be at least 128MB, to sign in at least...
* Not sure if any of this makes a difference considering that that OSMC + LEDFX is CPU intensive.
