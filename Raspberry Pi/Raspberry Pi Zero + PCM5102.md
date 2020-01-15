# Raspberry Pi Zero + PCM5102

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
* [Copy and paste the info from asounrc config](ALSA%20Sound%20Profiles/Multi/Raspberry%20Pi%200%20+%20PCM5102/.asoundrc)
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
