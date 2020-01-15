# Configuring OSMC and Testing
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



[Next Step, Installing Python 3.6](Installing Python 3.6.md)
