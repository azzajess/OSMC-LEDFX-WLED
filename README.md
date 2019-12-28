# OSMC-LEDFX-WLED
OSMC with LEDFX and WLED

Hi all. So this is a short tutorial for getting LEDFX to work with OSMC on a Raspberry Pi.

Now a note to all, I used a raspberry pi zero and a pcm5102 dac board for this project. Although it worked the raspberry pi zero is not powerful and i reccomend that you choose a more powerful raspberry pi for this project. 
Also to note that some of these options will be different depending the type of device you have and the configuration. An example of this is if you have the inbuilt audio out in raspberry pi 1,2,3 etc and will affect the numbering of some configurations

I will do my best to demonstrate this to you. Im no expert with Linux audio.

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


1. Loopback Module
In terminal (I used PUTTY)
(`user:osmc\pass:osmc`) You should change password but perhaps after we are done configuring since we will need to restart many times)
2. Load module at boot
* `sudo cd nano /etc/modules`
* Add `snd-aloop`
* Save and close (Ctrl+X then Y then enter)
* Reboot

This will add loopback devices to all your current devices and load at boot. 
NOTE: Since this loads at boot it will be device 0 as opposed.

2. OSMC configuring
* In osmc go to ‘my osmc’
* Go to pi-config (on the left)
* Scroll down to hardware Support
* On sound overlay, choose hifiberry-dac-overlay for PCM5102
* Restart osmc
* In OSMC menu, go to Settings/System/Audio
* And choose for Audio Output Device ALSA:snd_rpi_hifiberry_dac, Analog
* You may now test out if audio works. I downloaded a plugin called Radio for internet radio
* However we will need to set this back to ALSA: Loopback(), Loopback PCM in order to test if Reactive LEDs work. Its ok, there is no sound yet.

3. Installing Python 3.6
* Ok so Python 3.5 comes pre-installed with OSMC, But LEDfx requires Python 3.6+, So we have to install Python 3.6. 
WARNING: This process takes a long time, depending on Raspberry Pi. Took 40+ mins for me. So be mindful.
Source below but I will summerise what I did.
HERE

Source: https://tecadmin.net/install-python-3-6-ubuntu-linuxmint/
To install v3.6 of python

4. Inatalling LEDfx (Mattallmighty branch) <-- Hyperlink his github
* The reason for using Mattallmighty's branch instead of the one from ahodges9 (https://github.com/ahodges9/LedFx) is because it has been updated with a few more patterns and improvements/optimisations. You are free to simply use his version by using 
* We need to install GIT with `sudo apt-get install git`
* We clone Mattallmighty's branch with `git clone https://github.com/Mattallmighty/LedFx.git`
* We then go into the directory with `cd LedFx`

* We have to update pip since we updated Python with `sudo pip3 install --upgrade pip`
* Then we need to install setuptools that will build a python program `pip3 install -U setuptools`
* This is a preqreuisite required for linux LEDfx `sudo apt-get install portaudio19-dev`
* `pip3 install -r requirements.txt` to install any other requirements required for ledfx.
* `sudo pip3 install .` to install/Create LEDfx(The dot is included)

* Before starting we have to delete fadecandy python file due to error when starting ledfx
* `cd /usr/local/lib/python3.6/site-packages/ledfx/devices/`
* `sudo rm fadecandy.py`

5. Starting and Configuring LEDfx
* `ledfx --open` to start LEDfx and create the config file and wait until it has booted (It will probably say somthing like couldnt recgonise pcm devices, thats when you know its open.
* In order to access the webui we must manually set the host in the config file that was just created
* Ctrl+C to close LEDfx
* `cd` to return to home directory
* `cd .ledfx` to navigate to ledfx directory where config is stored
* `ls` to ensure that a config file is there
* `sudo nano config.yaml` (You can press tab to finish off name in putty!) 
*


NOTE: I did this out of order due to errors that i went back and installed to fix. So this is hopefully the correct order of installing LEDfx with a new version of Python.

A video tutorial of this for windows is done by ________ (https://www.youtube.com/watch?v=6AiMSeULZ08&feature=youtu.be)
However some of the commands are applicble to Linux

How this works!?
Ok so im no expert.
Few things to consider, snd_aloop module creates a loopback device for EVERY device you have. All we are doing is duplicating or splitting audio through multi(?) and connecting it to the hardware loopback so we can listen on the same audio we are sending out to the capture.
Like I said there is more than likely room for improvement and if you discover anything that can improve performace whether thats using jack or somthing else, please let us know.

Extra Optimisations?
* I reduced the resolution of osmc to 720 for testing purposes
* I lowered the GPU memory in My OSMC/Pi-Config to 64MB since I wont be using a display. Even at 64MB it was usable so this would mostly affect video content?
* Not sure if any of this makes a difference considering that that OSMC + LEDFX is CPU intensive.
