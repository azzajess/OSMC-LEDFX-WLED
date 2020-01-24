# ahodges9 Master

Just keep in mind that at the time of writing these instructions, This was the most supported. Things can change and may not be the most support version. Refer to [Readme.md](/README.md) for whats recommended.

(Up to date instructions can be found here: https://ahodges9.github.io/LedFx/#linux-installation)

* We have to update pip since we updated Python with `sudo pip3 install --upgrade pip` (May not be needed)

* Then we need to install setuptools that will build a python program `sudo pip3 install -U setuptools`

* This is a prerequisite required for linux LEDfx `sudo apt-get install portaudio19-dev`

`sudo pip3 install ledfx`
That should install all required dependencies

* If you would like to upgrade LEDfx use `pip install ledfx --upgrade`
* You can check which version you are currently running with `ledfx --version`

[Final step, Starting and Configuring LEDfx](/Starting%20and%20Configuring%20LEDfx.md)
