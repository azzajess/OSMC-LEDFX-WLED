# Raspberry Pi 3
(Probably also Other Raspberry Pi's with an inbuilt Audio out)
In terminal (I used PUTTY)
(`user:OSMC\pass:OSMC`) You should change the password but perhaps after we are done configuring since we will need to restart many times)
NOTE: Most of what we need to do must be done via ssh and not on the raspberry pi terminal. If you need to debug or access OSMC terminal via pi, press esc when showing the blue ommc symbol on boot or OSMC exit.
* alsa-tools will be used for figuring out what device number is which device.
* `sudo apt-get install alsa-utils`
* `aplay -l` to list playback devices. We are mainly looking at card 0, device 0 which seems to be the inbuilt audio out of Raspberry Pi
```
osmc@osmc:~$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  Subdevices: 7/7
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 IEC958/HDMI [bcm2835 IEC958/HDMI]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```
* `arecord -l` to list capture devices. You will notice that we dont have any capture devices yet.
```
osmc@osmc:~$ arecord -l
**** List of CAPTURE Hardware Devices ****
```
* `alsamixer` (Note for aslsamixer in Putty you need to use the function keys which normally set to some other command. You have to go to Putty settings, Terminal, Keypad and select 'Xterm R6'. For me I had to set this every time I opened Putty or created a new session.
* `sudo nano /etc/modules`
* Add `snd-aloop`
* Save and close
* Reboot (sudo reboot now)
* You can verify snd_aloop has been loaded with `sudo lsmod |grep snd_aloop`.
```
osmc@osmc:~$ sudo lsmod |grep snd_aloop
snd_aloop              24576  0
snd_pcm               114688  2 snd_aloop,snd_bcm2835
snd                    86016  4 snd_timer,snd_aloop,snd_bcm2835,snd_pcm
```
* Once the module is loaded you can validate it with `aplay -l` which should list a loopback device. This will add loopback devices to all your current devices and load at boot. 
```
osmc@osmc:~$ aplay -l
**** List of PLAYBACK Hardware Devices ****
card 0: Loopback [Loopback], device 0: Loopback PCM [Loopback PCM]
  Subdevices: 8/8
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
  Subdevice #7: subdevice #7
card 0: Loopback [Loopback], device 1: Loopback PCM [Loopback PCM]
  Subdevices: 8/8
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
  Subdevice #7: subdevice #7
card 1: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
  Subdevices: 7/7
  Subdevice #0: subdevice #0
  Subdevice #1: subdevice #1
  Subdevice #2: subdevice #2
  Subdevice #3: subdevice #3
  Subdevice #4: subdevice #4
  Subdevice #5: subdevice #5
  Subdevice #6: subdevice #6
card 1: ALSA [bcm2835 ALSA], device 1: bcm2835 IEC958/HDMI [bcm2835 IEC958/HDMI]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```
You will also notice that we have loopback capture devices for those input devices if you use `arecord -l`
NOTE: Since this loads at boot it will be device 0 and everything will come after
* Note those numbers! Card 0, device 0 = hw:0,0. Card 0 Device 1 = hw:0,1. Card 1, Device 0 = hw:1,0 etc
* Lets configure the asoundrc which allows us to specify how the devices interact with each other.
* `sudo nano ~/.asoundrc`
* Copy and paste the info from [asounrc config]()
* NOTE before continuing there are a few things to change in order to match your configuration, as it may be slightly different to mine. The only two options that need to be edited is `hw:0,0` and `hw:1,0`. Hardware 0,0 is the loopback device created by snd-aloop. Hardware 1,0 is the inbuilt audio out that is listed in `aplay -l` as number 1 because snd-aloop is loaded at boot before dac overlay. This may be different depending on your configuration of devices.
* If you have noticed that there is a lot of subdevices that we won't be using. I believe the subdevices are used for more than one app at a time. To change the amount we can use `sudo nano /etc/modprobe.d/sound.conf`
Assign spot and also reduce number of substreams to 2 for loopback
```
alias snd-card-0 snd-aloop
alias snd-card-1 snd-audio

options snd-aloop index=0 pcm_substreams=2
options snd-audio index=1
```
* Save and reboot.
* This config will assign snd card value(?) and reduce loopback substreams to 2. 

Source: http://www.6by9.net/output-to-multiple-audio-devices-with-alsa/

[Next Step, OSMC configuring](/Configuring%20OSMC.md)
