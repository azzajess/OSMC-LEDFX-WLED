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
In terminal (I used PUTTY)
(`user:OSMC\pass:OSMC`) You should change the password but perhaps after we are done configuring since we will need to restart many times)
NOTE: Most of what we need to do must be done via ssh and not on the raspberry pi terminal. If you need to debug or access OSMC terminal via pi, press esc when showing the blue ommc symbol on boot or OSMC exit.
* alsa-tools will be used for figuring out what device number is which device.
* `sudo apt-get install alsa-tools`
* `aplay -l` to list playback devices
* `arecord -l` to list capture devices
* `alsamixer` (Note for aslsamixer in Putty you need to use the function keys which normally set to some other command. You have to go to Putty settings, Terminal, Keypad and select 'Xterm R6'. For me I had to set this every time I opened Putty or created a new session.
* `sudo cd nano /etc/modules`
* Add `snd-aloop`
* Save and close
* Reboot
* You can verify snd_aloop has been loaded with `sudo lsmod |grep snd_aloop`.
* Once the module is loaded you can validate it with `aplay -l` which should list a loopback device. This will add loopback devices to all your current devices and load at boot. 
NOTE: Since this loads at boot it will be device 0 and everything will come after
* Note those numbers! 0 = hw:0,0, 1 = hw:1,0, 2 = hw:2,0 etc
* Lets configure the asoundrc which allows us to specify how the devices interact with each other.
* `sudo nano ~/.asoundrc`
* Copy and paste the info from asounrc config (https://github.com/azzajess/OSMC-LEDFX-WLED/blob/master/.asoundrc)
* NOTE before continuing there are a few things to change in order to match your configuration, as it may be slightly different to mine. The only two options that need to be edited is `hw:0,0` and `hw:1,0`. Hardware 0,0 is the loopback device created by snd-aloop. Hardware 1,0 is the pcm5102(hifi berry dac overlay) that is listed in `aplay -l` as number 1 because snd-aloop is loaded at boot before dac overlay. This may be different depending on your configuration of devices.
* If you have noticed that there is a lot of subdevices that we won't be using. I believe the subdevices are used for more than one app at a time. To change the amount we can use `sudo nano /etc/modprobe.d/sound.conf`
Assign spot and also reduce number of substreams to 2 for loopback
```
alias snd-card-0 snd-aloop
alias snd-card-1 snd-pcm5102

options snd-aloop index=0 pcm_substreams=2
options snd-pcm5102 index=1
```
* Save and reboot.
* This config will assign snd card value(?) and reduce loopback substreams to 2. 

Source: http://www.6by9.net/output-to-multiple-audio-devices-with-alsa/


2. OSMC configuring
* In OSMC go to ‘my OSMC’
* Go to pi-config (on the left)
* Scroll down to hardware Support
* On sound overlay, choose hifiberry-dac-overlay for PCM5102
* Restart OSMC
* In OSMC menu, go to Settings/System/Audio
* And choose for Audio Output Device ALSA:snd_rpi_hifiberry_dac, Analog
* While also here, in the bottom left there is an option to change the amount of settings avalible to the user. This needs to be on expert to see a couple of settings we need to change. One of them is to turn on ‘send low volume noise’ if not already and 'Keep audio device alive' to always. This ensures it doesnt loose connection when audio isnt playing (This needs validation though). We also need to change 'Output Config' to 'Fixed in order to change the max to sample rate to 44100hz. We then set the resample quality to 'low(fast)' to lighten the load on cpu. 
* You may now test out if audio works. I downloaded a plugin called Radio for internet radio via osmc plugins.
* We will need to change the audio device back to 'ALSA: Loopback(), Loopback PCM' in order to test if Reactive LEDs work. You should hear clicks when navigating through OSMC.

Need to verify following...
* Verifying it works!
* From OSMC home via ssh `cd Music`. This is the folder for music we will be recording a sample in order to listen and verify audio is being captured.
* In OSMC via monitor play some music via the Loopback pcm audio device (This should be set already)
* `arecord -D hw:0,0 -d 30 -f S16_LE test.wav`
* After it is done, in OSMC navigate to music and back out of current music folder (backspace) and open the file you created and listen to see if that is what you were playing when recording.

3. Installing Python 3.6
* Ok so Python 3.5 comes pre-installed with OSMC, But LEDfx requires Python 3.6+, So we have to install Python 3.6. 
WARNING: This process takes a long time, depending on Raspberry Pi. Took 40+ mins for me. So be mindful.
Source below but I will summarise what I did.
* Prerequisites
* `sudo apt-get install build-essential checkinstall`
* `sudo apt-get install libreadline-gplv2-dev libncursesw5-dev libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev`
* Download Python 3.6
* `cd /usr/src`
* `sudo wget https://www.python.org/ftp/python/3.6.9/Python-3.6.9.tgz`
* `sudo tar xzf Python-3.6.9.tgz`
* Compile Python Source
* `cd Python-3.6.9`
* `sudo ./configure --enable-optimizations` (Takes a while)
* `sudo make install` (Takes even longer) (For my installation I chose to use normal install instead of `sudo make altinstall`. Normal install replaces Python 3.5 with Python 3.6. I did that because I didn't want to deal with two Python installations. But if you know/want to install Python 3.6 along side Python 3.5 then use `altinstall`. Just keep in mind that commands may be different for the following if you do. With that said, I have not run into any issues yet with a python 3.6 install on OSMC.
* You can test if you have Python 3.6 by using `python3 -v` or `python3.6 -v` for alt install
Source: https://tecadmin.net/install-python-3-6-ubuntu-linuxmint/


4. Installing LEDfx [Mattallmighty Fork](https://github.com/Mattallmighty/LedFx)
* The reason for using Mattallmighty's Fork instead of the one from ahodges9 (https://github.com/ahodges9/LedFx) is because it has been updated with a few more patterns and improvements/optimisations. You are free to simply use adhodges version by using `pip3 install ledfx`. If so you can skip to Step 6 after doing `sudo pip3 install --upgrade pip`
* We need to install GIT with `sudo apt-get install git`
* We clone Mattallmighty's branch with `git clone https://github.com/Mattallmighty/LedFx.git`

* We have to update pip since we updated Python with `sudo pip3 install --upgrade pip`
* Then we need to install setuptools that will build a python program `pip3 install -U setuptools`
* This is a prerequisite required for linux LEDfx `sudo apt-get install portaudio19-dev`
* We then go into the directory with `cd LedFx`
* `pip3 install -r requirements.txt` to install any other requirements required for ledfx.
* `sudo pip3 install .` to install/Create LEDfx(The dot is included)

* Before starting we have to delete fadecandy python file due to error when starting ledfx (This may not be a needed step in the future, You can test with simply trying to launch ledfx and looking if fadecandy.py causes an error)
* `cd /usr/local/lib/python3.6/site-packages/ledfx/devices/`
* `sudo rm fadecandy.py`

5. Starting and Configuring LEDfx
* `ledfx --open` to start LEDfx and create the config file and wait until it has booted (It will probably say something like couldn't recognise pcm devices, that's when you know its open or webserver active, 
* In order to access the webui we must manually set the host in the config file that was just created
* Ctrl+C to close LEDfx
* `cd` to return to home directory
* `cd .ledfx` to navigate to ledfx directory where config is stored
* `ls` to ensure that a config file is there
* `sudo nano config.yaml` (You can press tab to finish off name in putty!) 
* Here we will be changing a few options. At the very beginning of the list add this in 
`audio:
  device_index: 1
  mic_rate: 44100`
* This manually sets the capture device to use instead of default one, which for me didn't work. You may try it by leaving it out and opening ledfx. You will need to iterate through the numbers to get the right assuming your configuration is different to mine. Mine was set to 1. The mic rate forces the capture device to take in 44100hz input which matches osmc settgngs we set eariler.
* To access the webui from another computer (Which you will need as we can't access it from host OSMC machine) then you will need to set the host to `0.0.0.0` or the IP address of the Raspberrt Pi.
* Ok so close and save file and `ledfx --open`
* Once loaded try accessing the page through your computer and now you should have ledfx up and running! But first we need to make sure it actually works and LEDs go flashy. First create a device and find which IP your WLED (Or whatever it is your using) which you can use the WLED app for. Again make sure your have configured WLED to use e1.31. You can also change a few settings in the advanced settings if you want. Also WLED needs to be turned on in app or web ui if it isn't :)
* You will want to make sure that you have the 'energy (reactive)' effect for music to actually see if you get anything some aren't as reactive at default. 
* Start playing music on OSMC and see if you can see the LEDs flash.

* You may of noticed that you will need to close and open LEDfx if you don't/want to use it. You can automate this process with various home automations like node-red, however I won't go into that here. Yet?
A video tutorial of this for windows is done by Gabriel Dahl (https://www.youtube.com/watch?v=6AiMSeULZ08&feature=youtu.be)
However some of the commands are applicable to Linux

NOTE: I did this out of order due to errors that I went back and installed to fix. So this is hopefully the correct order of installing LEDfx with a new version of Python. Please forgive me if you encounter problems due to some misinformation.

How this works!?
Ok so i'm no expert.
Few things to consider, snd_aloop module creates a loopback device for EVERY device you have. All we are doing is duplicating or splitting audio through multi(?) and connecting it to the hardware device so we can listen to the same audio we are sending out to the capture.
Like I said there is more than likely room for improvement and if you discover anything that can improve performance whether that's using jack or something else, please let us know.

Extra Optimisations?
* I reduced the resolution of OSMC to 720 for testing purposes
* I lowered the GPU memory in My OSMC/Pi-Config to 64MB since I won't be using a display. Even at 64MB it was usable so this would mostly affect video content?
* Not sure if any of this makes a difference considering that that OSMC + LEDFX is CPU intensive.
