# Configuring OSMC and Testing

If your using the inbuilt audio device of raspberry pi then skip to where it says HERE
* In OSMC go to ‘my OSMC’
* Go to pi-config (on the left)
* Scroll down to hardware Support
* On sound overlay, choose hifiberry-dac-overlay for PCM5102
* Restart OSMC
* In OSMC menu, go to Settings/System/Audio
* And choose for Audio Output Device ALSA:snd_rpi_hifiberry_dac, Analog

HERE
* While also here, in the bottom left there is an option to change the amount of settings avalible to the user. This needs to be on expert to see a couple of settings we need to change. One of them is to turn on ‘send low volume noise’ if not already and 'Keep audio device alive' to always. This ensures it doesnt loose connection when audio isnt playing (This needs validation though). 
* We also need to change 'Output Config' to 'Fixed in order to change the max to sample rate to 48000hz. We then set the resample quality to 'low(fast)' to lighten the load on cpu. You may run into a scenario where the playback is at 44100hz, you would then have to force LEDfx to only pick up 44100hz (Covered later). Another option is to use ALSA to upsample/downsample to the required sample rate if osmc doesnt already do it.
* You may now test out if audio works. I downloaded a plugin called Radio for internet radio via osmc plugins.
* We will need to change the audio device back to 'ALSA: Loopback(), Loopback PCM' in order to test if Reactive LEDs work. You should hear clicks when navigating through OSMC. (Remember, to choose 'ALSA: Loopback(), Loopback PCM' and not the default)

The Following doesnt currently work
-----------------------

Need to verify following...
* Verifying it works! Play some music or flick around the menu on osmc so that audio is being played through the LoopbackPCM
* From OSMC home via ssh `cd Music`. This is the folder for music we will be recording a sample in order to listen and verify audio is being captured.
* In OSMC via monitor play some music via the Loopback pcm audio device (This should be set already)
* `arecord -D hw:0,0 -f dat -d 20 test.wav`
* After it is done, in OSMC navigate to music and back out of current music folder (backspace) and open the file you created and listen to see if that is what you were playing when recording. In the root Music Section of OSMC, Files/Add music.../Browse/HomeFolder/Music then CLick OK, OK again and click no or yes for adding to your libary, Up to you.
----------------------------------------------------------
Lets assume its gonna work :)


[Next Step, Installing Python 3.6](Installing%20Python%203.6.md)

