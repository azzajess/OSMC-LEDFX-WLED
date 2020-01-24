# Starting and Configuring LEDfx

* `ledfx --open` to start LEDfx and create the config file and wait until it has booted (It will probably say something like couldn't recognise pcm devices, that's when you know its open or webserver active, 
* In order to access the webui we must manually set the host in the config file that was just created
* Ctrl+C to close LEDfx
* `cd` to return to home directory
* `cd .ledfx` to navigate to ledfx directory where config is stored
* `ls` to ensure that a config file is there
* `sudo nano config.yaml` (You can press tab to finish off name in putty!) 
* Here we will be changing a few options. At the very beginning of the list add this in 
```
audio:
  device_index: 1
 ```
* This manually sets the capture device to use instead of default one, which for me didn't work. You may try it by leaving it out and opening ledfx. You will need to iterate through the numbers to get the right assuming your configuration is different to mine. Mine was set to 1. 
* You may add `mic_rate: 44100` underneath `device_index 1` if you would like to force the the mic to capture audio at a 44100hz input which you may also need to change the osmc settings to match. HOWEVER doing this you loose the PitchSpectrum Effect in LEDfx. (That will be fixed in future version)
* To access the webui from another computer (Which you will need as we can't access it from host OSMC machine) then you will need to add the host to `hosts:0.0.0.0` or the IP address of the Raspberrt Pi.
* Ok so close and save file and `ledfx --open`
* Once loaded try accessing the page through your computer and now you should have ledfx up and running! But first we need to make sure it actually works and LEDs go flashy. First create a device and find which IP your WLED (Or whatever it is your using) which you can use the WLED app for. Again make sure your have configured WLED to use e1.31. You can also change a few settings in the advanced settings if you want. Also WLED needs to be turned on in app or web ui if it isn't :)
* You will want to make sure that you have the 'energy (reactive)' effect for music to actually see if you get anything some aren't as reactive at default. 
* Start playing music on OSMC and see if you can see the LEDs flash.

* You may of noticed that you will need to close and open LEDfx if you don't/want to use it. You can automate this process with various home automations like node-red, however I won't go into that here. Yet?
A video tutorial of this for windows is done by Gabriel Dahl (https://www.youtube.com/watch?v=6AiMSeULZ08&feature=youtu.be)
However some of the commands are applicable to Linux
